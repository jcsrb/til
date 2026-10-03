# `ocrmypdf --skip-text` skips scanned pages that have a digital stamp

`--skip-text` skips OCR on "any pages that already contain text". A scanned page with a digital page number (or a "Page 3 of 40" footer, or a "received" stamp) stamped on top *does* contain text: those few characters. So it's skipped, and the scan stays unsearchable.

Find the pages that really need OCR (see [find-pages-that-need-ocr](find-pages-that-need-ocr.md)) and force OCR on just those:

```sh
ocrmypdf --force-ocr --pages 51-100 in.pdf out.pdf
```

* `--pages` takes ranges or a comma-separated list, e.g. `--pages 1,3,51-100`
* `--force-ocr` rasterizes the page before OCR, so the stamp ends up in the image and is recognised along with everything else
* the other pages stay untouched, so their digital text stays exact

Sources:
* `ocrmypdf --help` (16.8)
* https://ocrmypdf.readthedocs.io/en/latest/advanced.html
