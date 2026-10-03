# Clean up Word → Markdown conversion artifacts

*As of October 2026.*

Converting DOCX/ODT to Markdown (here with [xberg](https://github.com/xberg-io/xberg), the successor of kreuzberg) leaves some odd output behind:

* Word tabs (`<w:tab/>`, ODT `<text:tab>`) as a literal `&#9;`: `**Name:**&#9;Alice Example`. Only the Markdown output had this. Reported as xberg #2039 and fixed on main after 1.3.2.
* nested bold + underline as `**__Note__**`
* underline as escaped HTML, `\<u\>Costs\</u\>`, inside table cells

```sh
perl -pe 's/&#9;/\t/g; s/\*\*__(.+?)__\*\*/**$1**/g; s/\\<\/?u\\>//g' in.md > out.md
```

```
**Name:**	Alice Example
**Note**: see below
| Costs | 100 |
```

A quick way to find artifacts like these: `grep -c '&#'` and `grep -c '\\<'` over the whole output, not just a sample.

Sources:
* https://github.com/xberg-io/xberg/issues/2039
