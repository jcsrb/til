# Gemini Flash as an OCR fallback

*As of October 2026. Prices move fast, check the current ones.*

A big hosted model is a good fallback for the pages a small local OCR model gets wrong (rotated, messy, looping).

## Cost is the output, so turn reasoning down

* a page image is ~1,100 input tokens, whatever the resolution (see [vlm-ocr-cost-does-not-depend-on-dpi](vlm-ocr-cost-does-not-depend-on-dpi.md))
* with default reasoning, a page produced 950–5,000 output tokens; in one sample 740 of 1,083 were reasoning tokens. The gateway reported about $0.004–0.02 per page.
* with `"reasoning_effort": "minimal"` (via the OpenAI-compatible endpoint) the same transcription took 330–460 output tokens (differences: blank lines only), about 3× cheaper

Transcription doesn't need thinking.

## Cache, but never cache an empty answer

* cache by PDF hash + page number (+ model + prompt), so reruns never pay twice
* failed calls can come back as empty pages; don't store them, or the cache keeps the failure forever

## Beware of request logging

If an LLM gateway or proxy logs request content, every base64 page image ends up in its logs. At 2–3 MB per page that adds up fast. Send small renders (150 dpi PNG, ~250 KB, same token cost, see [vlm-ocr-cost-does-not-depend-on-dpi](vlm-ocr-cost-does-not-depend-on-dpi.md)) or turn content logging off.

Sources:
* https://ai.google.dev/gemini-api/docs/pricing
* https://ai.google.dev/gemini-api/docs/openai
