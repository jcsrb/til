# Fix and translate subtitles with ASR + LLMs

*As of September 2026. Model names and prices move fast.*

I had an older TV series with English subtitles only, some of them badly out of sync, and wanted German and Romanian ones as well.
Subtitles are often made for a different release or cut of the same video: offset, drifting (25 vs 23.976 fps), or even for the wrong episode.
Transcribing the audio first fixes all of that, and the corrected text can then be translated with an LLM, keeping the timings.
The result: ~110 episodes retimed and translated into two languages within a day, for about $10, and the subtitles hold up while watching.

## 1. Transcribe with word timestamps

* extract one mono 16 kHz audio track, Opus at 32k is plenty (~11 MB per 45 min episode)
* MacWhisper Pro (paid) ships a CLI; Parakeet v2 does 45 min of audio in ~30 s on an M3 Max
* free alternatives with word timestamps: [parakeet-mlx](https://github.com/senstella/parakeet-mlx), [WhisperKit](https://github.com/argmaxinc/WhisperKit), whisper.cpp

```sh
ffmpeg -i episode.mkv -map 0:a:1 -ac 1 -ar 16000 -c:a libopus -b:a 32k episode.opus
/Applications/MacWhisper.app/Contents/MacOS/mw transcribe \
  --model parakeet-pro:nvidia_parakeet-v2 --language en --no-speakers --format json -o episode.json episode.opus
```

## 2. Check you have the right subtitle at all

* compare word trigrams of the transcript with the subtitle text
* the right file overlaps ~70-85%, a wrong one ~2% — this caught two episodes whose subtitles were swapped

## 3. Retime from the transcript

* align subtitle words to transcript words with `difflib.SequenceMatcher(autojunk=False)`
* each cue takes the start/end of its matched words; cues without a match get the offset of their nearest neighbour
* handles fixed offsets, fps drift and different cuts in one go
* `ffsubsync` (voice-activity based) gave garbage on a music-heavy show; windows scored negative and the offsets just hit its ±60 s limit

## 4. Translate cue by cue

* send numbered batches of ~40 cues: `[1] text`, `[2] text`, … plus 4 cues before/after as untranslated context
* encode line breaks as `<br>`, and tell the model to keep `<br>`, `<i>` and leading `- ` speaker dashes
* add a glossary of names/places and the register (du/Sie, tu/dumneavoastră)
* validate that every `[n]` came back; retry, then split the batch in half — only ~0.15% of batches needed it
* reuse the original timing line for every cue, never let the model touch timestamps
* strip blank lines inside a translated cue, otherwise the SRT parser ends the cue early and every cue after it shifts

The prompt per batch (context lines are the source cues just before/after the batch, so short lines like "Go on." or "OK." get translated in context, and a speaker's "you" stays consistent across batch borders):

```text
Translate these TV subtitle lines from English to German.

<one line about the show and setting>. Keep names as they are: <main characters, places, recurring things>.
Friends say 'du' to each other; use 'Sie' only for clearly formal situations.
Use natural spoken German like dubbed TV, not literal translation.

Rules:
- Output exactly one line per input line, starting with the same [n] marker, nothing else.
- Keep <br> (subtitle line break), <i>…</i> tags and a leading "- " (speaker change) where they are.
- Keep it short enough to read as a subtitle.

Context before (do not translate):
<previous 4 cues>

Context after (do not translate):
<next 4 cues>

Lines to translate:
[1] Don't get me wrong. <br> I'm not kidding myself.
[2] - You know what I would do? <br> - About what?
...
```

For Romanian the register line becomes: natural spoken Romanian with full diacritics (ș, ț with comma below, not cedilla), friends use 'tu'.
Parse the reply with `^\s*\[(\d+)\]\s?(.*)$`, turn `<br>` back into newlines, and reject the batch if any number is missing or empty.

## Model notes (EN → DE / RO, TV dialogue)

Based on a 38-line sample through every model plus one full episode side by side, judged by a native speaker of both languages. An impression, not a benchmark.

| model | verdict |
|---|---|
| Gemini Flash | best value, most natural German, very good Romanian |
| Gemini Pro | best Romanian, but thinking tokens (~2k/batch, billed as output) make it ~8x the price of Flash |
| Gemini Flash-Lite | fine German, clumsy Romanian |
| Gemma 4 26B local (LM Studio) | stiff German, real errors in Romanian, ~20 min per episode for both languages |
| TranslateGemma 12B | literal, dropped speaker dashes, mixed up singular/plural "you" |

Gotchas:

* LM Studio: Gemma 4 "thinks" by default; `"reasoning_effort": "none"` took a request from 18 s to 0.8 s
* LM Studio sometimes returns HTTP 400 "does not match the expected peg-gemma4 format": treat it as a failed attempt and retry
* Gemini: Flash-Lite takes `thinkingLevel: "minimal"`, Flash only `"low"`; pin a concrete model name, the `-latest` aliases move
* The Gemini Batch API is 50% cheaper (results within 24 h, a 30-request test took minutes): 2 languages × ~83k cues ≈ 4,200 requests ≈ $10 with Flash

## Before you ship

* name the files `<video name>.<lang>.srt` next to the video, so Plex and Bazarr pick them up (Bazarr then stops searching for that language)
* check every output: same number of cues as the English, identical timing lines, no empty cues, comma-below ș/ț in Romanian
* lines left in English are not automatically a bug: foreign-language scenes, names, song and show titles stay as they are
* translated subtitles follow the original audio, not a dub; that is normal, even for professional subtitles: a dub is written for lip sync, subtitles for reading speed, so the two never match word for word
