# Gemini refuses to OCR some public documents: `finishReason: RECITATION`

*As of October 2026.*

Asking Gemini to transcribe a scanned page can return no text at all:

```json
{"candidates": [{"finishReason": "RECITATION"}]}
```

* RECITATION isn't a copyright or licence check: it blocks output that matches text the model has seen verbatim
* so public, widely republished text (judgments, statutes, standard contract boilerplate) is *more* likely to trip it, not less
* it's not random: retrying the same request usually gives the same result
* `safetySettings` don't turn it off, they only cover the harm categories

Sources:
* https://ai.google.dev/api/generate-content#FinishReason
