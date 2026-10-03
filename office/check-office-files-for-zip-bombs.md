# Check DOCX/ODT files for zip bombs before converting them

DOCX, XLSX, PPTX, ODT… are zip files. A small upload can expand to gigabytes when LibreOffice (or any converter) opens it.

The zip's central directory declares each entry's uncompressed size, so you can check without decompressing anything:

```python
import os, zipfile

MAX_ENTRIES = 10_000
MAX_TOTAL = 500 * 1024 * 1024   # 500 MiB uncompressed
MAX_RATIO = 100                 # uncompressed : file size

def check(path):
    with open(path, "rb") as f:
        if f.read(4) != b"PK\x03\x04":
            return "not a zip (e.g. legacy .doc), skip"
    with zipfile.ZipFile(path) as z:      # reads the central directory only
        infos = z.infolist()
    total = sum(i.file_size for i in infos)
    ratio = total / max(1, os.path.getsize(path))
    if len(infos) > MAX_ENTRIES or total > MAX_TOTAL or ratio > MAX_RATIO:
        return f"reject: {len(infos)} entries, {total} bytes, ratio {ratio:.0f}:1"
    return "ok"
```

```
t.docx     ok
bomb.docx  reject: 1 entries, 600000000 bytes, ratio 1030:1
```

* a broken or truncated zip raises `zipfile.BadZipFile`: reject that too, and don't retry, it won't get better
* declared sizes can be faked, so this catches ordinary bombs, not crafted ones

The real protection is isolation: one conversion per container (concurrency 1), with a timeout and a memory limit, so a pathological file only takes down its own instance.

Sources:
* https://en.wikipedia.org/wiki/Zip_bomb
* https://docs.python.org/3/library/zipfile.html#zipfile-objects
