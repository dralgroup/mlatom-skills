# MLatom Skills

A community collection of **AI-agent skills for [MLatom](https://github.com/dralgroup/mlatom)** — the open-source package for atomistic simulations with machine learning and quantum chemistry.

A *skill* here is a short, self-contained Markdown recipe that teaches an AI agent — Claude, Cursor, GitHub Copilot, Aider, or **[Protomia](https://aitomistic.com/protomia)** (the Aitomistic AI assistant for atomistic simulations) — or a human — how to do one MLatom task correctly: install it, run a method, reproduce a published workflow.

Skills are plain Markdown. No framework, no lock-in: any agent that can read a file can use them, and you can read them yourself.

## Using a skill

Point your AI agent at the skill file for the task you want, for example:

> "Follow `skills/installing-mlatom/SKILL.md` to set up AIQM3 in a new environment."

Or drop the skill into your project so your agent picks it up automatically — e.g. reference it from your `AGENTS.md`, or copy it into `.cursor/rules/` or `.claude/skills/`. Each skill's front matter says which MLatom version it targets.

Prefer not to install anything? MLatom and many of these methods run online, registration-free, on the [Aitomistic Hub](https://aitomistic.xyz) and [Aitomistic Lab@XMU](https://atom.xmu.edu.cn) (only for the users with academic email), where the [Protomia](https://aitomistic.com/protomia) AI assistant can run them for you.

## Catalog

| Skill | What it does | MLatom |
|-------|--------------|--------|
| [`installing-mlatom`](skills/installing-mlatom/SKILL.md) | Install MLatom and the AIQM3 add-ons in a clean environment; diagnose the common install failures | ≥ 3.23 |

_More coming — and yours is welcome (see below)._

## Contributing

This repository is **open to everyone.** If you have an MLatom workflow worth sharing — a method setup, an analysis recipe, the pipeline behind a paper — please add it. See **[CONTRIBUTING.md](CONTRIBUTING.md)**; it takes about ten minutes to copy the template and open a pull request.

First-time open-source contributors are especially welcome. If anything is unclear, open an issue and ask.

## Related

- [MLatom on GitHub](https://github.com/dralgroup/mlatom) · [Documentation](http://mlatom.com/docs/) · [MLatom.com](http://mlatom.com)

## License

Released under the [Apache License 2.0](LICENSE); copyright Pavlo O. Dral and contributors (see [NOTICE](NOTICE)). Contributions are accepted under the same license. Skills that accompany a publication should cite that paper — see each skill's **Cite** section and [CITATION.cff](CITATION.cff).
