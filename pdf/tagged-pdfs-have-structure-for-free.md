# Tagged PDFs come with their structure for free

Many PDFs exported from Word (and anything made accessible) are **tagged**: besides the text, they carry a structure tree with headings, paragraphs, lists, tables and footnotes, like HTML. No layout guessing needed.

```sh
pdfinfo f.pdf | grep Tagged
# Tagged:          yes

pdfinfo -struct-text f.pdf 2>/dev/null | grep -v '^ */'   # hide layout attributes
# Document
#   H1 (block):
#     "Annual Report"
#   P (block):
#     "This report covers the year in review."
#   L (block):
#     LI (block)
#       Lbl (block)
#         "1."
#       LBody (block)
#         P (block):
#           "Revenue grew."
```

`pdfinfo -struct` gives the same tree without the text, handy for a quick look at which tags are used:

```sh
pdfinfo -struct f.pdf 2>/dev/null | sed 's/^ *//' | sort | uniq -c | sort -rn
```

Gotchas:

* **quality varies a lot.** One "tagged" document was 99 `P` and a single `H1`; others have proper lists, tables and `Note` elements for footnotes. Look at the tag counts before trusting it, and fall back to layout analysis when it's flat.
* **a `Span` is not a word, and not a paragraph.** PDFs exported by Word for Microsoft 365 can wrap every text run in its own `Span`, thousands per document (7,086 spans in 49 paragraphs in one 12-page file). Run boundaries fall anywhere, even mid-word:

  ```
  P (block)
    Span (inline)
      "The Fir"
    Span (inline)
      "st and Second Notices are "
    Span (inline)
      "referred to"
  ```

  Treat each `Span` as its own block and you get one word (or half a word) per paragraph. Concatenate the spans of a `P` as-is, without adding spaces or line breaks; the spaces are already in the text.
* **Acrobat puts IDs into the tag names**: `Span <AD000000-0000-0000-ADBE-        1742> (inline)`. Strip them before matching on tag names: `line.gsub(/<AD.*?>\s/, '')`
* poppler prints hundreds of `Syntax Warning: Wrong Attribute 'LineHeight' in element P` lines to stderr on perfectly usable files, hence the `2>/dev/null`
* footnote references show up as a number inside a `Link (inline)`, the footnote text itself as `Note`

Sources:
* https://accessible-pdf.info/en/basics/general/overview-of-the-pdf-tags/
* `man pdfinfo` (`-struct`, `-struct-text`)
