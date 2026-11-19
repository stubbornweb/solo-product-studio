# Studio — agent rules

This repository holds the shared documents of the studio: the words in `GLOSSARY.md`, the system decisions in `docs/adr/`, and the one plugin marketplace in `.claude-plugin/marketplace.json`. It is also the studio plugin (ADR 0008): `.claude-plugin/plugin.json` and the skills in `skills/` that cover more than one engine. The studio is a project too. Its meta repo is `stubbornweb/solo-product-studio-meta`, with the idea and the system brief in `1-idea/`.

## Shared rules

1. Write a failing test first for any code with logic.
2. Run `claude plugin validate .` before you open a pull request.
3. Write all text in plain English (Simplified Technical English style, CEFR B1).

## Repo rules

- Use the words in `GLOSSARY.md`.
- A proposal for an engine follows ADR 0005.
- `docs/sources/` holds read-only snapshots. Decisions go in an ADR or an issue.
- Read an engine's files from `stubbornweb/<engine>` on GitHub, or from its local clone after `git pull` on `main`. The plugin cache in `~/.claude/plugins/cache/` can be older than `main`.
- Each engine plugin has one entry in `.claude-plugin/marketplace.json`, with no `version` field, so every merge to `main` of the engine repo is a release.
- [`ask-studio`](skills/ask-studio/SKILL.md) is the router that maps every skill the owner can reach. When a change adds, renames or removes a skill of any engine, or changes its place in the flow, re-read `skills/ask-studio/SKILL.md` and update it in the same change. When the change is in an engine repo, update it in a second pull request in this repo. A router that names a missing skill, or misses a new one, gives wrong answers.
