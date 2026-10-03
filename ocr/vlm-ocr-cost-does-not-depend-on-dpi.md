# A VLM OCR page costs about the same at any normal DPI

*As of mid 2026.*

Vision-language models resize the image to a pixel budget internally, so once your render is above that cap, a page costs roughly the same number of vision tokens whatever you send:

* PaddleOCR-VL 0.9B: ~1,200 vision tokens per page
* Gemini Flash: ~1,100 input tokens per page image; a 150 dpi render cost the same as a 2–3 MB one

So **lowering the DPI doesn't make it faster or cheaper** (until you go below the model's cap, and then you lose detail). On CPU, PaddleOCR-VL took ~145 s per page, of which ~130 s was the vision encoder prefill and only ~13 s generation. On born-digital pages its output was byte-identical at 100, 150, 200 and 300 dpi.

What a smaller render *does* save is bandwidth and memory along the way (base64 payloads, gateways, logs). 150 dpi PNG (~250 KB) is plenty for OCR.

What drives cost is output: see [gemini-flash-as-ocr-fallback](gemini-flash-as-ocr-fallback.md).
