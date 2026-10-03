# Marker silently drops content

*As of mid 2026 (marker-pdf 2.x).*

[Marker](https://github.com/datalab-to/marker) is a good PDF → Markdown converter for text-layer prose, but some losses come with no error:

## Blocks classified out of existence

`PageFooter` and `PageHeader` blocks have `ignore_for_output` set, and render as an empty string. A signature block (name, role, date) at the foot of the last page, next to the page number, is exactly what the layout model calls a footer, so it's gone. Same for a document's own citation printed at the top of page one ("running header").

* `MarginaliaProcessor` also sets `ignore_for_output` on plain `Text` blocks by position, without changing the block type
* `keep_pagefooter_in_output` is not the fix: it keeps every page number too
* the output can still be *longer* than the PDF's text layer, so length checks won't catch it

Write a processor that un-ignores blocks you can recognise by content. When something is missing, check `ignore_for_output` first.

## Tables gutted

A Table block can end up with no `TableCell` children, the grid only in `block.html`. A 42-row table came out as 11 rows; a 5-column table vanished entirely. Nothing downstream can tell a complete table from a gutted one.

Count the figures (amounts, numbers) in the text layer vs the Markdown to find them; route table-heavy documents to something else.

## U+0002 instead of a hyphen

Inside table cells marker sometimes writes `\x02` where the PDF has `-`: `father-in-law` becomes `father\x02in\x02law`. Rare (about 1 document in 150), so sampling never shows it; only a byte sweep over the whole output finds it.

Sources:
* https://github.com/datalab-to/marker
