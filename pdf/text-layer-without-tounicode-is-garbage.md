# A PDF can have a text layer that is pure garbage

A font embedded without a `ToUnicode` CMap extracts as raw glyph codes, not characters:

```
'\x00\x01\x02\x01\x03\x01\x04\x05\x01\x07\n\x07\x08\t\n\x01\x03\x07\n\x0b\x0c'
```

pdftotext, PyMuPDF and pdftext all see the same thing. The danger is in routing: "does this page have text?" says yes, the page skips OCR, and the garbage lands in your output with no error. Converting those documents properly (with OCR) roughly doubled the word count on some of them, so the bad pages also lost text.

How to detect it:

* **Control characters** are the reliable signal: nothing else puts them in extracted text. Flag a page where they make up ≥5% of the text.
* Some fonts shift into printable ASCII instead: "The Notices" came out as `7KH 1RWLFHV` (every character shifted down by 29 code points: a glyph-index offset). A high share (≥40%) of vowel-less words catches that.
* A **low word count is not evidence**: tables of contents (dot leaders) and cover pages look empty. One test of that kind flagged 178 documents, of which only 7 were really broken; the control-character test found exactly those 7.

```python
import re
CONTROL = re.compile(r"[\x00-\x08\x0b\x0c\x0e-\x1f]")

def is_garbled(text):
    return len(text) >= 200 and len(CONTROL.findall(text)) / len(text) >= 0.05
```

Don't strip the control characters to tidy the output. One of these documents failed a database write because of a NUL byte. That error was the only reason the garbage wasn't stored. Stripping NULs would have turned one loud failure into silent corruption.

Fix: send those pages to OCR, the image is fine.

Sources:
* PDF 1.7 spec, 9.10.3 "ToUnicode CMaps"
