# Fake bold: letters printed twice come out twice

Some PDF producers don't use a bold font. They paint the same text two or three times, shifted by a fraction of a point, so it *looks* bold (or has a drop shadow). PDFBox's docs say it plainly: "Word paints the same character several times in order to make it look bold."

Not to be confused with *overprint* in the print-production sense (`/OP`, overprint mode), which is about how colour separations mix, not about text.

Every copy is real text in the PDF, so what you get depends on the extractor. The same line painted 3 times, 0.3 pt apart:

```
pdftotext             Fake Bold
pdfplumber            FFFaaakkkeee BBBooolllddd
pdfplumber, deduped   Fake Bold
PyMuPDF               Fake Bold
                      Fake Bold
                      Fake Bold
```

* **pdftotext** removes overlapping duplicates itself; `pdftohtml` doesn't (poppler issue #321)
* **pdfplumber**: call `page.dedupe_chars()` before extracting. It drops chars with the same text, font name and size within `tolerance=1` pt.
* **PDFBox**: `setSuppressDuplicateOverlappingText(true)`, on by default
* **PyMuPDF**: you get each painted copy as its own line, dedupe yourself

The opposite failure exists too. Two neighbouring `l`s in some fonts overlap enough to look like a duplicate, so an aggressive check turns "called" into "caled" and "all" into "al" (poppler issue #165, for `pdftohtml`). Compare the character *and* its position, keep the tolerance small, and spot-check words with double letters.

Sources:
* https://pdfbox.apache.org/docs/2.0.13/javadocs/org/apache/pdfbox/text/PDFTextStripper.html#setSuppressDuplicateOverlappingText-boolean-
* https://gitlab.freedesktop.org/poppler/poppler/-/issues/321 ("Some PDFs draw text multiple times to emulate bold text or drop shadows")
* https://gitlab.freedesktop.org/poppler/poppler/-/issues/165
* https://github.com/jsvine/pdfplumber (`.dedupe_chars()`)
