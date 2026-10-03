# Marker hangs forever on the first page that needs OCR

*As of mid 2026 (marker-pdf 2.x).*

Marker's OCR path starts a `llama-server` binary from your `PATH` to run its OCR model (surya). If that llama.cpp is too old to know the model's architecture (`qwen35`), the server crashes while loading and marker **waits forever** on the first page that needs OCR. No error, no timeout.

* build 6810: hangs
* build 10809: works

```sh
llama-server --version
brew upgrade llama.cpp
```

Also clean up after it: marker can leave `surya.*.server` processes and lock files under `~/.cache/datalab/surya/` behind.

Sources:
* https://github.com/datalab-to/marker
