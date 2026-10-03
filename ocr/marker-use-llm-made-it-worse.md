# Marker's `use_llm` made the output worse

*As of mid 2026 (marker-pdf 2.x with a frontier LLM).*

Marker can send its layout-correction processors through an LLM (`--use_llm`). On a test document scored against a reference conversion, it got worse:

| | order similarity | content overlap |
|---|---|---|
| marker default | 0.934 | 0.949 |
| marker + `use_llm` | **0.683** | 0.958 |

* it emitted one paragraph twice (nothing invented, but a duplicated paragraph in a legal document is a correctness bug)
* it promoted a line to a heading while leaving the original line in place
* 8 s per page, and it hit the provider's rate limits, because marker runs its six LLM processors in parallel

Worst part: it **fails silently**. With a bad API key every processor logs an error, marker still exits 0 and writes the degraded Markdown. If you use it in a pipeline, treat LLM errors in the log as a failure yourself.

Sources:
* https://github.com/datalab-to/marker
