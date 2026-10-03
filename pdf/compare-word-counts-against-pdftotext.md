# Compare word counts against pdftotext to catch extractors losing text

A converter that drops text doesn't error. A cheap regression check: compare its word count with plain `pdftotext`, which is dumb but complete.

```sh
for f in *.pdf; do
  a=$(pdftotext -q "$f" - | wc -w)
  b=$(my-extractor "$f" | wc -w)        # your converter's text/markdown output
  awk -v f="$f" -v a="$a" -v b="$b" 'BEGIN { printf "%s\t%d\t%d\t%+.1f%%\n", f, a, b, (b - a) * 100 / (a ? a : 1) }'
done
```

A difference of a few percent is markup and hyphenation; more than that, look at the document.

This caught an extractor upgrade that silently dropped text, glued words together across line breaks, and repeated whole sentences (a *higher* count can be wrong too).

For a stricter check, compare the sets of words, not just the totals: a document can lose a table and gain a duplicated paragraph and still come out with the same count.

Sources:
* https://poppler.freedesktop.org/
