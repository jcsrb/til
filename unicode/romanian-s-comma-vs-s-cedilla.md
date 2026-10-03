# Romanian `ș ț` (comma below) are not `ş ţ` (cedilla)

They look almost the same, but they are different letters:

| correct (Romanian) | | lookalike (Turkish, Gagauz, …) | |
|---|---|---|---|
| `Ș` U+0218 | `ș` U+0219 | `Ş` U+015E | `ş` U+015F |
| `Ț` U+021A | `ț` U+021B | `Ţ` U+0162 | `ţ` U+0163 |

Romanian uses the comma below. The cedilla versions became the stand-in because the old encodings only had those: Windows-1250, ISO-8859-2 and early Unicode had no comma-below letters at all. They only got an 8-bit home in 1998 (Romanian standard SR 14111), published as ISO-8859-16 (Latin-10) in 2001. So a lot of Romanian text, old and new, still uses the cedilla ones.

Even the state got it wrong: Romanian passports said `PAŞAPORT` (cedilla) on the cover from 2011 until the new design of September 2024, which quietly switched to `PAȘAPORT`.

## Normalise for search and matching

Map the cedillas to commas, but only for Romanian text (Turkish needs its `ş`):

```ruby
text.tr("ŞşŢţ", "ȘșȚț")   # "Şi ţara, în ştiri" → "Și țara, în știri"
```

## `ºi` and `þara`: the Windows 98 classic

Romanian text saved in Windows-1250 and shown as Windows-1252 turns `Şi ţara, în ştiri` into `ªi þara, în ºtiri`. Anyone who watched films with Romanian subtitles on Windows 98 has seen `ºi`. Fix it by re-decoding, not by stripping the diacritics:

```ruby
"ªi þara, în ºtiri".encode("Windows-1252").force_encoding("Windows-1250").encode("UTF-8")
# => "Şi ţara, în ştiri"   (then .tr as above for the comma versions)
```

Sources:
* https://en.wikipedia.org/wiki/S-comma
* https://en.wikipedia.org/wiki/ISO/IEC_8859-16
* https://brandient.com/kit-on-romanian-diacritics (the passport story, and much more)
