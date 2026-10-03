# Get the page size of a PDF the way `pdfinfo` does

```sh
pdfinfo f.pdf | grep -E 'Page (size|rot)'
# Page size:       595.276 x 841.89 pts (A4)
# Page rot:        0
```

## Not all pages are the same size

By default `pdfinfo` only reports **page 1**. Merged PDFs mix sizes all the time: a letter cover on an A4 document, a landscape table, a scanned page at whatever size the scanner picked. List every page with `-l -1`:

```sh
pdfinfo -l -1 f.pdf | grep -E '^Page +[0-9]+ (size|rot)'
# Page    1 size:  612 x 792 pts (letter)
# Page    1 rot:   0
# Page    2 size:  595.276 x 841.89 pts (A4)
# Page    2 rot:   0
# Page    3 size:  612 x 792 pts (letter)
# Page    3 rot:   90
```

Anything that assumes page 1's size for the whole document (rendering, cropping, text coordinates) breaks on the odd page. Page 3 above shows up landscape, because of its `/Rotate`.

## What `pdfinfo` actually does (`utils/pdfinfo.cc`)

* uses the **CropBox** (which defaults to the MediaBox when missing), not the MediaBox
* does **not** swap width and height for `/Rotate`; the rotation is printed on its own line
* names the paper size only for:
  * `letter`: within 1 pt of 612 × 792
  * `A0`–`A6`: within 0.3% of the *ideal* ISO 216 series (A0 long side = 2^¼ m), tolerance shrinking with each size
  * both orientations

So A5 (419.5 × 595.3) and A6 print with no name, in either orientation: the real sizes are rounded to whole mm and fall outside the shrinking tolerance. A3 and A4 are fine.

## Same thing in Python with pypdf

```python
import math
from pypdf import PdfReader

def paper_name(w, h):
    if (abs(w - 612) < 1 and abs(h - 792) < 1) or (abs(w - 792) < 1 and abs(h - 612) < 1):
        return "letter"
    h_iso = math.sqrt(math.sqrt(2)) * 7200 / 2.54   # A0 long side in pt
    w_iso = h_iso / math.sqrt(2)
    tol = h_iso * 0.003
    for n in range(7):                              # A0..A6
        if (abs(w - w_iso) < tol and abs(h - h_iso) < tol) or (abs(w - h_iso) < tol and abs(h - w_iso) < tol):
            return f"A{n}"
        h_iso, w_iso, tol = w_iso, w_iso / math.sqrt(2), tol / math.sqrt(2)

for n, page in enumerate(PdfReader("f.pdf").pages, 1):   # every page, not just the first
    box = page.cropbox                           # pypdf falls back to the MediaBox
    w, h = float(box.width), float(box.height)   # no swap for page.rotation
    name = paper_name(w, h)
    print(f"{n}: {w:g} x {h:g} pts" + (f" ({name})" if name else ""), "rot:", page.rotation)
```

Use `pdfinfo -box f.pdf` to see all boxes (Media, Crop, Bleed, Trim, Art).

Sources:
* https://gitlab.freedesktop.org/poppler/poppler/-/blob/master/utils/pdfinfo.cc
