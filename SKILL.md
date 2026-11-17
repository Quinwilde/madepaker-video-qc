---
name: video-qc-gate
description: Pre-upload quality gate for AI-produced Shorts and documentaries — file-stability check, sync standard (TTS + whisper-timed subtitles), loudness window, subtitle completeness across languages, motion check on visuals, and description honesty. Use before publishing any generated video; a failed check means no upload.
---

# Video QC Gate

A generated video is not done when it renders. It is done when it passes
every gate below, in order. This gate was distilled from daily production
of Shorts and documentaries on two channels — every rule here exists
because skipping it once shipped a defect.

## Gate 0 — File stability (before any measurement)

A cancelled production job can leave an orphan encoder process still
rewriting the "final" file. QC on a file that is still being written is
worthless.

- Confirm no encoder process is writing the file.
- Confirm the file size is stable across two checks.
- Only then probe and measure.

## Gate 1 — Format

- Documentary: **1080p** master, kept on disk at full quality.
- Upload copy: encoded to fit the platform pipeline limit (for us:
  ≤ 120 MB for a ~10-minute documentary, e.g. 1080p ~1.45 Mbps video +
  copied audio) — the master is never the upload copy.
- Shorts: vertical 1080×1920.

## Gate 2 — Sync standard

- One TTS voice, fixed speed (our standard: speed 90).
- Clip audio muted; only TTS + synthetic ambience in the mix.
- Subtitles timed from **speech-recognition utterance timestamps**
  (whisper), never from script line estimates.
- Verify segment by segment: subtitle in/out must match the spoken words
  on spot checks across the whole runtime, not just the opening.

## Gate 3 — Loudness

- Integrated loudness within **−16 … −13 LUFS** (our Shorts ship around
  −15.5 LUFS). Measure the final mix, not the voice stem.
- Decode the full file end-to-end: no errors, no frozen frames
  (freeze-detect), no silent gaps.

## Gate 4 — Subtitles & languages

- Documentary: full caption track; burned-in text only at key moments
  (≈10–15% of duration max).
- Shorts: full burned subtitles, sized ≈55–60% smaller than documentary
  burned text.
- Every language track (our standard: 19 languages) carries **identical
  timing** — same segments, same in/out, translated text.
- No literal escape artefacts (a visible `\n` in burned captions is an
  automatic fail).

## Gate 5 — Visuals

- Real moving clips only — run a motion check; a slideshow of stills
  fails.
- No photorealistic AI humans (channel rule; disclosed exceptions stay
  disclosed, see Gate 6).

## Gate 6 — Honesty of the listing

- Description must match reality: if a scene is AI-generated, the
  description says so; never claim stock/Commons footage that is not
  actually in the video.
- Title/thumbnail claims must be countable and true (the same standard
  as a spec sheet: if it says "9 different realms", there are nine).

## Verdict

- **PASS** — all gates green: publish.
- **FAIL** — name the gate, fix, re-render, re-run the *whole* gate
  (a fix in one place drifts another: re-encoding changes loudness,
  re-timing changes subtitle segments).

Report the measured values (LUFS, duration, size, segment count) with
the verdict — never "looks fine".
