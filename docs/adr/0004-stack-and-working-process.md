# Stack and working process

One person maintains every part of the studio. So all code uses one toolchain, every repo uses the same few rules, and a rule exists only when it protects something now. The owner ships first. Stricter rules come at one fixed point, before the first client code or client credential goes into a repo.

## Code

- All studio code uses strict TypeScript on Node 26 or later (`"engines": { "node": ">=26" }`), npm with a committed lockfile, and `vite-plus`. `vp check` runs format, lint and type checks. `vp test` runs the tests.
- A script in any repo, an engine repo included, uses this same baseline. Any other file format is decided when a repo needs it.
- A new code repo with TypeScript code uses the same baseline. The Prototype Engine's `docs/stack-policy.md` chooses its framework. An existing site that joins later keeps its own stack.

## Branches and merges

- Now: agents work on a branch and open a pull request. Only the owner merges. The `AGENTS.md` rules tell agents this.
- Before the first client code or client credential goes into a repo:
  - A Claude Code hook on the owner's computer blocks every `git push` to `main`, in all repos.
  - On public repos, GitHub protects `main`: changes come only through a pull request, and CI must pass. No review is required, because GitHub does not let the owner approve their own pull request.

## CI

Each repo has one GitHub Actions workflow with `permissions: contents: read`:

| Repo | CI runs |
| --- | --- |
| `prototype-engine`, `hub`, code repos | `npm ci`, `vp check`, `vp test` on Node 26 |
| `idea-engine` | `npm ci`, `vp check`, `vp test` on Node 26, then `claude plugin validate` on the plugin |
| `solo-product-studio` | `claude plugin validate --strict` on the marketplace |
| meta repos, `clients-meta` | no CI |

## Releases

- No `plugin.json` and no marketplace entry has a `version` field. Claude Code then uses the commit ID as the version, so every merge to `main` is a release.
- No tags, no CHANGELOG and no release step. The git history is the record.
- The owner turns on auto-update for the studio marketplace.

## Licence

Every public repo has an MIT `LICENSE` file.

## Security

- Now: secret scanning with push protection is on for every public repo.
- Before the first client code or client credential goes into a repo: a global gitleaks pre-commit hook, Dependabot on repos with code, actions pinned to commit SHAs, an approval list for agents, and the rest of the checklist in [the handoff](../sources/2026-11-03-handoff.md).

## Agent rules

Every repo has its own short `AGENTS.md`, because an agent in one repo cannot see the others. Each one starts with these shared rules, then adds the rules for that repo:

1. Work on a branch and open a pull request. Never push to `main`.
2. Write a failing test first for any code with logic.
3. Run the repo's check before you open a pull request.
4. Write all text in plain English (Simplified Technical English style, CEFR B1).

`AGENTS.md` holds all the rules. `CLAUDE.md` has only one line, `@AGENTS.md`, so Claude Code loads the same rules as every other agent.

Skills and prototype sites have no unit tests. A prototype site is checked in a real browser.

## New engine checklist

A new engine repo has:

- the plugin folder (`.claude-plugin/plugin.json` with no `version` field), listed in the `studio` marketplace
- an `AGENTS.md` with the shared rules
- the CI workflow for its kind of repo
- an MIT `LICENSE` file
- push protection turned on
- no project output inside the repo

## Considered options

- **Node 25.** Rejected: it reached end of life on June 1, 2026. Node 26 is the LTS version until April 2029.
- **Each repo picks its own tools.** Rejected: one person cannot maintain several toolchains.
- **Only one shared rules file, linked from each repo.** Rejected: agents often do not follow links into another repo.
- **`CLAUDE.md` as a symlink to `AGENTS.md`, or as a second rules file.** Rejected: a symlink can break on Windows and in some git setups, and a second rules file hides its rules from agents other than Claude.
- **GitHub Team, to protect `main` on private repos.** Rejected: the hook on the owner's computer blocks pushes to `main`.
- **The push hook and branch protection now.** Rejected: the owner ships first, and the `AGENTS.md` rules already keep agents off `main`.
- **Version numbers and tags for each release.** Rejected: when `version` is set and not raised, users do not get new commits. A forgotten step then loses the update without a warning.
- **`claude plugin validate --strict` on an engine plugin.** Rejected: `--strict` makes every warning an error, and the check warns about two choices in this ADR, the plugin with no `version` field and the `CLAUDE.md` at the plugin root. Without `--strict`, errors still fail CI. The marketplace has no such warnings, so its check keeps `--strict`.
- **Markdown lint and link checks in CI.** Rejected: more config and failed pull requests about style, and they check nothing that breaks a tool.
- **The full security checklist now.** Rejected: no repo holds client code or credentials yet, and the owner ships first. Push protection is the exception, because a leaked key in a public repo cannot be taken back.
- **No licence, so that the code keeps all rights for a sale.** Rejected: the engine repos are public so that their plugins can be used. A sale is about the brand, not the code.
- **A template repo or an `engine-start` skill.** Rejected for now: only two engines exist, and they are very different. Make one when the Design Engine starts, from real repos.

## Consequences

- Nothing changes by this decision. After the map, the setup follows: the CI workflows, the `AGENTS.md` and `CLAUDE.md` files, the `LICENSE` files, the push protection settings, and the removal of `"version": "0.1.0"` from the skills `plugin.json`.
- The `engines` fields in the `prototype-engine` `package.json` files change to `>=26`.
- "Every feature is a command" from the handoff checklist is a rule for the Hub's design, so the Hub build map decides it.
