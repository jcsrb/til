# llama.cpp: "The model produced output that does not match the expected peg-native format"

*As of August 2026.*

On some perfectly fine pages, PaddleOCR-VL served over the OpenAI-compatible `/v1/chat/completions` endpoint fails with a 500:

```
The model produced output that does not match the expected peg-native format
```

* llama.cpp's chat endpoint parses the model output for tool calls. Before mid-2026 a parse failure just gave an empty turn; since then it throws.
* the GitHub issue (#25339) was closed by its reporter without a fix
* **Ollama ≥ 0.32.3** recovers on its side and returns the raw completion; older Ollama: use the native `/api/generate` endpoint instead of the chat endpoint
* **LM Studio** is still broken (llama.cpp engine 2.28.2), and shows the engine's 500 as an HTTP 400. It has no `/api/generate`-style bypass.
* vLLM serves the same model fine over the OpenAI API

Sources:
* https://github.com/ggml-org/llama.cpp/issues/25339
