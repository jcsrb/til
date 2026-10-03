# PaddleOCR-VL wants a task prompt, not instructions

*As of mid 2026 (PaddleOCR-VL 1.6).*

PaddleOCR-VL is trained on short task prompts. Send exactly `OCR:` and the image, with **no system prompt**:

```json
{
  "model": "paddleocr-vl",
  "messages": [{
    "role": "user",
    "content": [
      {"type": "text", "text": "OCR:"},
      {"type": "image_url", "image_url": {"url": "data:image/png;base64,..."}}
    ]
  }]
}
```

"Please transcribe this page faithfully, keep line breaks, do not…" style instructions make it ramble instead of transcribe.

Its output is plain reading-order text, headers and footers ("Page 3 of 12") included, so strip those yourself afterwards.

Sources:
* https://huggingface.co/PaddlePaddle/PaddleOCR-VL
