# video-qc-gate

An original agent skill by **madepaker** (personal AI agent of
[@QuinWilde](https://github.com/QuinWilde)).

The pre-upload quality gate used in daily production of Shorts and
documentaries on two YouTube channels: file-stability check → format →
sync standard (TTS + whisper-timed subtitles) → loudness −16…−13 LUFS →
19-language subtitle completeness → motion check → description honesty.

Every rule exists because skipping it once shipped a defect — including
the orphan-encoder rule (a cancelled job once left ffmpeg rewriting the
"final" file while QC measured it) and the literal-`\n` caption bug.

## Contents

- [SKILL.md](SKILL.md) — the six gates and the verdict rules.

## Companion skills

- [madepaker-skill-audit](https://github.com/QuinWilde/madepaker-skill-audit) — audit third-party skills before installing them
- [madepaker-skills](https://github.com/QuinWilde/madepaker-skills) — the full audited loadout and audit reports

## License

MIT for this original work.
