# Detect when a small VLM OCR model got a page wrong

Small vision-language OCR models (PaddleOCR-VL, GLM-OCR, ~1B params) are fast and mostly near-verbatim, but fail in recognisable ways:

* **CJK leaking into English**: `shall be平等并符合合同`, `照片` for "photos"
* **decode loops**: one sentence repeated until the token limit, or counters like `{308} {309} {310}…`
* **gibberish bursts**: a few lines of nonsense inside an otherwise perfect page

Cheap per-page detectors catch all three:

```python
import collections, re

WORDS = {w.strip().lower() for w in open("/usr/share/dict/words")}
CJK = re.compile(r"[぀-ヿ㐀-鿿가-힯]")  # kana, CJK ideographs, Hangul
ACCENTS = re.compile(r"[À-ÖØ-öø-ÿ]")              # accented Latin letters

def flags(text):
    found = []
    if len(CJK.findall(text)) >= 3:
        found.append("cjk")
    if len(re.findall(r"\{\d+\}", text)) >= 5:
        found.append("counter-loop")
    toks = text.split()
    grams = collections.Counter(tuple(toks[i:i + 8]) for i in range(len(toks) - 7))
    if grams and grams.most_common(1)[0][1] >= 3:
        found.append("repeats")
    # gibberish comes in bursts: score 25-word windows, not the whole page
    words = [w.lower() for w in re.findall(r"[A-Za-z]{4,}", text)]
    bad = [w not in WORDS and w.rstrip("s") not in WORDS for w in words]
    letters = max(1, len(re.findall(r"[A-Za-z]", text)))
    other_language = len(ACCENTS.findall(text)) > 0.01 * letters
    if not other_language and any(sum(bad[i:i + 25]) > 12 for i in range(max(1, len(bad) - 24))):
        found.append("gibberish")
    return found
```

Lessons from tuning it:

* a whole-page dictionary ratio (35%) missed a burst of garbage inside a good page, hence the sliding window
* pages in another language trip the English dictionary check, so skip pages where accented letters (é, ü, ñ, ç, …) are over 1% of the letters; adjust for your languages
* "repeats" fires on legitimate repeated headers and lists, so only run the checks on pages that went through OCR, not on digital text
* only use the CJK check if your documents aren't supposed to contain CJK

What to do with a flagged page: [re-ocr-bad-pages-at-lower-dpi](re-ocr-bad-pages-at-lower-dpi.md).
