# Running a VLM OCR model in Ollama or LM Studio

*As of mid 2026 (Ollama 0.32, LM Studio with llama.cpp engine 2.28).*

## The default context silently truncates pages

The context has to hold the image *and* the answer: ~1.3k vision tokens + `num_predict` (e.g. 4096). Ollama's default context is 4096, and it truncates without an error. Set it per request:

```json
{"model": "...", "options": {"num_ctx": 8192, "num_predict": 4096}}
```

LM Studio's just-in-time loading uses a small default context **and splits it across its 4 parallel slots**. Load the model explicitly:

```sh
lms load paddleocr-vl-1.6 --context-length 32768 -y
```

## The vision projector file name matters

A VLM GGUF comes as two files: the model and the vision projector (`mmproj`). LM Studio only pairs them when the projector file name starts with `mmproj-`; it doesn't pair a file named `<model>-mmproj.gguf`. Rename it to `mmproj-<model>.gguf` in the models folder.

Ollama's `ollama pull hf.co/PaddlePaddle/PaddleOCR-VL-1.6-GGUF` failed with `Error: 400:` after downloading, probably the same naming problem; a community repack on ollama.com (`ollama pull AuditAid/PaddleOCR-VL-1.6-0.9B`) worked. Its tensors are byte-identical to the official GGUF.

Sources:
* https://docs.ollama.com/modelfile#parameter
* https://lmstudio.ai/docs/cli
