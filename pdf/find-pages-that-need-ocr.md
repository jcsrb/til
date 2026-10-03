# Find the pages of a PDF that need OCR (count per page, not per document)

A whole-document character count hides hybrids. A 100-page PDF can have plenty of text overall while its last 50 pages are a scanned appendix with no text at all.

Count non-whitespace characters per page with poppler:

```sh
f=document.pdf
n=$(pdfinfo "$f" | awk '/^Pages:/ {print $2}')
for p in $(seq 1 "$n"); do
  c=$(pdftotext -q -f "$p" -l "$p" "$f" - | tr -d '[:space:]' | wc -m)
  printf '%s\t%d\t%d\n' "$f" "$p" "$c"
done
```

A page with under ~100 characters is a page without text.

PDFs that "have no text" come in four kinds:

* **Text layer that was never read.** Surprisingly common: the text is there, it just never got extracted. Check before you OCR anything.
* **Hybrid.** Digital pages plus scanned pages. Typical: a scanned cover page first, a scanned signature page last (printed, signed, scanned back in), or a scanned appendix or exhibit. Only the scanned pages need OCR.
* **Image only.** No fonts at all (`pdffonts` prints an empty table), e.g. 1-bit CCITT scans at 300 dpi.
* **Thin layer.** Under 100 characters on every page despite page-sized images: a stamp or header line on top of a scan. Watch out, `ocrmypdf --skip-text` skips those: [ocrmypdf-skip-text-skips-stamped-scans](ocrmypdf-skip-text-skips-stamped-scans.md).

`pdffonts f.pdf` (no fonts = image only) and `pdfimages -list f.pdf` (which pages carry images) help tell the kinds apart.

The same goes for converters: a pipeline that routes on the *share* of bad pages can't protect a single page. One scanned signature page in a 19-page document is 5%. On the no-OCR path that page (with who signed it) simply disappears from the output, with no error. Any unreadable page that carries an image should send the document to OCR.

Sources:
* https://poppler.freedesktop.org/
