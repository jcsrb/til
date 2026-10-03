# Fix a bad OCR page by re-running it at a lower DPI

When a small VLM OCR model loops or leaks CJK characters on a page (see [detect-small-vlm-ocr-failures](detect-small-vlm-ocr-failures.md)), re-OCR **just that page** with a different image, and keep the first result that passes the checks:

```python
DPI_LADDER = [100, 150, 80]   # original run was at 200 dpi, longest side capped at 2048 px

for dpi in DPI_LADDER:
    text = ocr_page(pdf, page_no, dpi)
    if not flags(text):
        splice(page_no, text)
        break
else:
    keep_original_and_log(page_no)
```

`flags` is the detector from [detect-small-vlm-ocr-failures](detect-small-vlm-ocr-failures.md); `ocr_page`, `splice` and `keep_original_and_log` stand for your own OCR call and bookkeeping.

* the ladder fixed 60 of 63 flagged pages (46 at 100 dpi, 5 at 150, 9 at 80)
* clean pages came out byte-identical at 100 and 200 dpi, so going lower costs no quality
* the ladder must change the pixels: with a 2048 px cap, anything above ~175 dpi on A4 is the same image

Keep a **do-not-retry list**. One "stubborn" page was a genuine list of 11 identical bullet points; a loop detector will flag real repetition forever.

The last few pages can go to a bigger model, or be checked by hand.
