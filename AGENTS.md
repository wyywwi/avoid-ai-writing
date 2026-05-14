# AGENTS.md

Guidance for Codex and other coding agents working in this repository.

## What this is

A Codex-ready writing skill (`SKILL.md`) that audits and rewrites content to remove AI writing patterns. There is no build system and no runtime dependency. The product is the skill text plus optional Codex UI metadata in `agents/openai.yaml`.

## Repository structure

- `SKILL.md` - the skill itself. All rules, tiers, profiles, and output formats live here.
- `agents/openai.yaml` - optional OpenAI/Codex UI metadata for skill lists and default prompts.
- `README.md` - public-facing docs, Codex installation instructions, pattern reference table, and examples.
- `CHANGELOG.md` - version history inherited from the upstream project.

## How to make changes

Edit `SKILL.md` directly. When changing behavior:

- Keep the `SKILL.md` frontmatter Codex-compatible: only `name` and `description` unless a Codex validator explicitly permits more.
- Make the description trigger-oriented. Include when to use the skill, not a summary of the workflow.
- Update `agents/openai.yaml` if the skill name, UI description, or default prompt becomes stale.
- Update `README.md` if installation, usage, output format, or the pattern count changes.
- Keep the README pattern count table, currently "36 Patterns Detected", in sync with `SKILL.md`.
- Run the Codex skill validator before finishing:

```bash
python3 /home/rx/.codex/skills/.system/skill-creator/scripts/quick_validate.py .
```

## Architecture of the skill

The skill has two modes (`rewrite` default, `detect` flag-only) and processes text through this pipeline:

1. Context profile detection: auto-detects or accepts a profile hint (`linkedin`, `blog`, `technical-blog`, `investor-email`, `docs`, `casual`).
2. Pattern matching: 36 categories across content, language, structure, communication, and meta patterns.
3. Vocabulary flagging: 3-tier system: Tier 1 always flag, Tier 2 flag in clusters, Tier 3 flag at high density.
4. Severity classification: P0 credibility killers, P1 obvious AI smell, P2 stylistic polish.
5. Output: rewrite mode uses four sections including a second-pass audit; detect mode uses two sections with problem vs. judgment-call assessment.

## Key constraints

- Preserve the self-reference escape hatch so quoted examples in the skill are not flagged as violations.
- Keep word replacement table entries specific; avoid vague "rephrase" guidance when a concrete replacement is possible.
- Preserve technical-blog profile exceptions for terms with legitimate technical meaning.
- Preserve the defined meanings of "extra strict" and "skip" in the tolerance matrix.
