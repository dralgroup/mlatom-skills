# Contributing to MLatom Skills

Thank you for helping make MLatom easier to use with AI agents! This repo is meant to be **easy and welcoming to contribute to** — you do not need to be an open-source veteran.

## What makes a good skill

A skill is a single Markdown file (`SKILL.md`) that helps an AI agent (or a person) do **one MLatom task** reliably. Good skills are:

- **Specific** — one task ("run an AIQM3 geometry optimization"), not "everything about MLatom".
- **Reproducible** — you actually ran the steps and they work.
- **Self-contained** — all steps, inputs, and a way to check success are in the file.
- **Open** — they rely only on publicly available software (MLatom, the public add-ons, standard open packages). Do not include private or unpublished tools, credentials, or internal infrastructure.

## Add a skill in 4 steps

1. **Fork** this repository and create a branch.
2. **Copy the template:** `cp -r skill-template skills/<your-skill-name>`
3. **Fill in `SKILL.md`** — keep the section headings, replace the content, and state which MLatom version you tested with.
4. **Open a pull request** describing what the skill does and how you tested it.

A maintainer will review it, maybe suggest small edits, and merge. That's it.

## Skill format

Each skill lives in `skills/<name>/SKILL.md` and starts with a small YAML front matter block:

```yaml
---
name: installing-mlatom
description: Install MLatom and the AIQM3 add-ons in a clean environment.
mlatom: ">=3.23"        # MLatom version(s) you tested with
tags: [installation, aiqm3]
---
```

followed by these sections (drop any that don't apply):

- **When to use** — a line or two so an agent knows when this skill is relevant.
- **Prerequisites** — OS, Python version, packages, external programs.
- **Steps** — the actual commands / code, in order.
- **Verify** — how to confirm it worked (expected output).
- **Troubleshooting** — common failures and their fixes.
- **Cite** — papers to cite if the skill comes from published work.

Write for both an AI agent and a human: short, imperative, copy-pasteable. Keep facts (versions, requirements) that also live in the [MLatom docs](http://mlatom.com/docs/) consistent with them, and link out rather than duplicating long explanations.

## Reporting problems

Found a skill that no longer works, or did MLatom change under it? Please [open an issue](../../issues) — that's a contribution too.

## Code of conduct

By participating you agree to our [Code of Conduct](CODE_OF_CONDUCT.md). Be kind.
