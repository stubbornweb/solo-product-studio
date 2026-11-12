# Repo layout: tools, notes and code apart

Each part of the studio has its own repo. Every idea has one **meta repo** for its notes and the output of every stage. Every product gets one **code repo** for its code. All studio repos belong to `stubbornweb`. The repos are:

| Repo | Holds | Visibility |
| --- | --- | --- |
| `solo-product-studio` (today `studio`) | The main entrance of the studio: the one plugin marketplace, the glossary, the system ADRs and the wayfinder maps. It is the code repo of the studio project, and the engine repos are its parts | public |
| `hub` | The Hub. Made when the Hub build starts | public |
| `idea-engine` | Idea Engine skills, as one plugin | public |
| `prototype-engine` | Prototype Engine CLI, skills, templates and patterns, as one plugin | public |
| `design-engine` | Design Engine tools. Made only when the engine has its first tool | public |
| `clients-meta` | Client folders. Made when the first client arrives | private |
| `<slug>-meta`, one per idea | The notes, research, sources and the output of every stage | private |
| `<slug>`, one per product | The product code and the documents that explain it. Made when the build starts | private for a client project, public or private for an own project |

Rules:

- **The studio is a project too**, with the slug `solo-product-studio`. Its meta repo `solo-product-studio-meta` holds its idea, its brief and its history, like the meta repo of any project.
- **Engine repos hold only tools.** No engine keeps output inside its repo. The example prototype `demo-studio` and the patterns in `patterns/` are part of the tool, so they stay. Remove client content from a pattern before it goes into the repo.
- **A meta repo is made when work on an idea starts:** the first `idea-grill`, a quick experiment, or an existing product that joins. It stays the same repo when the idea becomes a project, so nothing moves at PROCEED. Each stage has its own top folder, in stage order.
- **A REJECT verdict archives the meta repo.** PARK and REFINE keep it open, because the owner comes back to the idea. An archived repo is read-only, and `gh repo list --no-archived` hides it.
- **A code repo is made when the build starts.** It holds the code and the documents that explain how the code works, such as `README.md`, the ADRs and a glossary. The project history (idea, research, plans, stage output) stays in the meta repo.
- **Client folders are in `clients-meta`**, because one client can order several projects.
- **Each engine repo is a Claude Code plugin**, so its skills work inside any meta repo or code repo. The `solo-product-studio` repo holds the one marketplace that lists them.

## Considered options

- **One big repo for all engines, or one skills repo for all skills.** Rejected: the engines change at different speeds, and the vision doc keeps them apart.
- **Studio documents in the `hub` repo.** Rejected: the Hub is one part. Studio documents inside it would break the rule that the Hub only reads from the other parts.
- **Prototype output in `prototype-engine/prototypes/<slug>/`.** Rejected: output then lives inside a tool repo.
- **One project repo with the notes and the code together, made at PROCEED.** Rejected: the code repo then holds many Markdown files, and the idea files must move between repos at PROCEED.
- **One shared meta repo for all ideas and projects.** Rejected: a client cannot get access to only their own project. One meta repo for each project is also the pattern that the studio makes for every project, and the studio follows it for itself. Most ideas die, but an archived meta repo costs nothing.
- **A local scratch folder with no git for quick experiments.** Rejected: the experiment has no backup, and it must move when its idea gets PROCEED.
- **Every repo public.** Rejected: meta repos, `clients-meta` and client code repos hold raw client messages and client code. A public repo cannot be fully made private again.

## Consequences

- The 8 web skills move from `prototype-engine/.claude/skills/` into the Prototype Engine plugin.
- The ideas in `idea-workspace` move into their own meta repos. Then `idea-workspace` is archived.
- `studio` is renamed to `solo-product-studio`. The system brief moves to `solo-product-studio-meta/1-idea/PROJECT-DESCRIPTION.md`.
- The portfolio repo `stubbornweb/yevhenii-portfolio` goes into `yevheniibuilds-meta/3-prototype/site/`, and then it is archived.
- ADR 0003 gives the folder names, the project file, and how a meta repo is made.
