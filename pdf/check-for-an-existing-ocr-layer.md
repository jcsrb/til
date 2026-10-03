# Old scans often already have an OCR layer

Before re-OCRing a scanned PDF, look at who made it:

```sh
pdfinfo scan.pdf | grep -E 'Creator|Producer'
```

Values like `PFU ScanSnap Organizer`, `Adobe PDF Scan Library` or Acrobat Paper Capture mean the scanner software already ran OCR. Even documents from the 1980s, scanned later with that kind of software, carry a hidden text layer; `pdftotext` gets ~1k characters per page out of them.

That layer is usually good enough for search, with typos from the OCR of its day. Decide whether better text is worth the cost before re-OCRing:

* keep it: nothing to do
* replace it: `ocrmypdf --redo-ocr` (keeps visible text, redoes the hidden layer) or `--force-ocr` (rasterizes everything)

Sources:
* https://ocrmypdf.readthedocs.io/en/latest/advanced.html
