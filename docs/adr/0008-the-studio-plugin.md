# The studio plugin

The `solo-product-studio` repo is also a Claude Code plugin, named `studio`. It holds the skills that cover more than one engine. Its first skill is `ask-studio`, a router: the owner tells it the situation, and it names the next skill, the file that skill writes, and the step after it. The marketplace lists the studio plugin with `"source": "./"`, next to the engine plugins.

This changes one rule of ADR 0002. That rule said only engine repos are plugins, and the studio repo only holds the marketplace. The new rule: a skill that works for one stage goes into its engine repo, and a skill that covers more than one engine goes into the studio plugin.

## Considered options

- **The router inside `idea-engine`.** Rejected: an engine does the work of one stage, but the router covers all stages.
- **A private skill in the owner's `~/.claude`.** Rejected: it is not versioned with the skills it maps, and it is not public like the other studio repos.
- **A flow section in `AGENTS.md` instead of a skill.** Rejected: agents in a meta repo do not read the studio `AGENTS.md`, and a meta repo is where the owner runs the engine skills.

## Consequences

- The map in `ask-studio` names every skill the owner can reach. It does not change by itself, so `AGENTS.md` of the studio repo and of each engine repo has a rule: a change that adds, renames or removes a skill, or changes its place in the flow, also updates `ask-studio`. A change in an engine repo then needs a second pull request in the studio repo.
- `ask-studio` reads a skill's `SKILL.md` before it says what the skill does. The one-line summaries in its map are only for orientation.
- `ask-studio` maps only studio skills. For the build stage it points to `/ask-matt`.
- `claude plugin validate --strict .` now checks the studio plugin as well as the marketplace.
