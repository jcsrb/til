# Pre-1997 Word `.doc` files fail with "Invalid DOC magic number"

Not every `.doc` is the same format. Word 97–2003 files identify themselves with `0xA5EC`; older Word 6.0/95 files use `0xA5DC`. Many extractors only support the former and fail with:

```
Invalid DOC magic number: ..., expected 0xA5EC
```

LibreOffice still reads them, so convert those first:

```sh
soffice --headless --convert-to docx old.doc
```

In one set of older documents, about 2% were Word 6/95.
