# OCR of rotated scans returns nothing

A page scanned sideways (a portrait page lying in a landscape box) OCRs to next to nothing (empty or a few garbage characters) with plain tesseract/ocrmypdf. Let ocrmypdf detect the orientation and turn the page upright first:

```sh
ocrmypdf --rotate-pages in.pdf out.pdf
```

`--rotate-pages` uses tesseract's orientation and script detection, which needs the `osd` traineddata:

```sh
tesseract --list-langs        # should list "osd"
sudo apt install tesseract-ocr-osd   # Debian/Ubuntu, if it's missing
```

The same applies to vision-language OCR models: they need the page upright too. On a rotated page a small model (PaddleOCR-VL 0.9B) produced CJK-flavoured gibberish, while a large one (Gemini Flash) read it fine. Rotate before you send the image, don't rely on the model.

Sources:
* https://ocrmypdf.readthedocs.io/en/latest/cookbook.html
* https://tesseract-ocr.github.io/tessdoc/
