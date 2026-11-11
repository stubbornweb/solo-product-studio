# Studio — agent rules

This repository holds the shared documents of the studio: the words in `GLOSSARY.md`, the system decisions in `docs/adr/`, and the one plugin marketplace in `.claude-plugin/marketplace.json`. The studio is a project too. Its meta repo is `stubbornweb/solo-product-studio-meta`, with the idea and the system brief in `1-idea/`.

## Shared rules

1. Work on a branch and open a pull request. Never push to `main`.
2. Write a failing test first for any code with logic.
3. Run `claude plugin validate --strict .` before you open a pull request.
4. Write all text in plain English (Simplified Technical English style, CEFR B1).

## Repo rules

- Use the words in `GLOSSARY.md`.
- `docs/sources/` holds read-only snapshots. Decisions go in an ADR or an issue.
- Read an engine's files from `stubbornweb/<engine>` on GitHub, or from its local clone after `git pull` on `main`. The plugin cache in `~/.claude/plugins/cache/` can be older than `main`.
- Each engine plugin has one entry in `.claude-plugin/marketplace.json`, with no `version` field, so every merge to `main` of the engine repo is a release.

## Issue tracker

Issues and wayfinder maps live in GitHub Issues for `stubbornweb/solo-product-studio` (`gh` CLI). A map has the label `wayfinder:map`, and its tickets are its sub-issues.
