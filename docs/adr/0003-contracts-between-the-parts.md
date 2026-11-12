# Contracts between the parts

Each part of the studio writes plain files, and the next part reads them. These files are the contracts. The Hub reads only files that are pushed to the default branch on GitHub, and it never changes them. No engine needs the Hub.

## Slug

- A slug uses only `a-z`, `0-9` and single dashes. It starts and ends with a letter or a digit, and it has 40 characters or fewer.
- Every tool rejects a wrong slug and shows this rule. No tool changes a slug by itself.
- A slug never changes after its meta repo is made. The public name of a product can change. It is written in the notes, not in a repo name.
- A project slug must not be the name of a studio repo (`hub`, `idea-engine`, `design-engine`, `prototype-engine`), must not be `clients`, and must not end in `-meta`.
- Client slugs are a separate list, so a client slug can be the same as a project slug.

## Local paths

Every tool uses these fixed paths and no environment variables:

- `~/www/<slug>-meta/` is the meta repo of one idea or project.
- `~/www/<slug>/` is the code repo of one product.
- `~/www/clients-meta/<client-slug>/` is the folder of one client.
- `~/www/prototype-engine/` is the local clone of the Prototype Engine. `extract-pattern` writes a new pattern into its `patterns/` folder, and the pattern reaches the plugin through a pull request.

## Stages

The stages are `idea`, `design`, `prototype`, `build`, `live` and `closed`. A project can start at any stage, skip stages and move back. An idea before PROCEED is always in the `idea` stage.

## Meta repo

A meta repo has this layout. A stage folder exists only when its stage has output.

```
<slug>-meta/
├── README.md           includes the entry note when the project joined at a later stage
├── AGENTS.md
├── CLAUDE.md           only `@AGENTS.md`
├── .gitignore          excludes build output, such as node_modules/
├── PROJECT.md          the project file, from PROCEED on
├── 1-idea/             IDEA-DECISION.md, research/, PROJECT-DESCRIPTION.md, references/ (ADR 0007)
├── 2-design/
├── 3-prototype/        prototype.yaml, EXPERIENCE.md, site/, qa/
├── 4-build/            notes about the build, such as plans. No code
└── 5-live/             results checks
```

## Code repo

A code repo has no project file and no stage folders:

```
<slug>/
├── README.md
├── AGENTS.md           names the meta repo: "The notes of this project are in `stubbornweb/<slug>-meta`."
├── CLAUDE.md           only `@AGENTS.md`
├── LICENSE
├── .github/workflows/  CI (ADR 0004)
└── package.json, src/ ...
```

## Project file

`PROJECT.md` is the only file the Hub needs from a project:

```markdown
# Acme booking site

**Format:** 1
**Stage:** build
**Client:** acme
**Code:** stubbornweb/acme-booking
```

- The `**Client:**` line exists only for a client project. An agent finds the client folder at `~/www/clients-meta/<client-slug>/`.
- The `**Code:**` line exists only when the project has a code repo. It gives the full `<owner>/<repo>` name, because a client's existing repo can be in another GitHub account.
- The repo name is `<slug>-meta`, so the file has no slug line.
- A `PROJECT.md` at the root of a meta repo marks a project. The Hub finds every idea and project as the `stubbornweb` repos whose names end in `-meta`, and it skips archived repos.
- `setup-meta-repo` sets the stage that the project enters at. Each engine sets its own stage when it starts work. The owner sets `build`, `live` and `closed` by hand.

An idea without the PROCEED verdict has no project file. The Hub reads its verdict from the `**Verdict:**` line of `1-idea/IDEA-DECISION.md`.

## Product decisions

`idea-grill` writes the product decisions of an idea in the "Product decisions" section of `1-idea/IDEA-DECISION.md`, after "Why this and not the alternative". The section has one line for each topic, such as `**Price:** about 29 PLN per month`. It always shows the current product decisions.

At PROCEED, `project-brief` copies this section into the "Product decisions" section of `1-idea/PROJECT-DESCRIPTION.md`. Later stages read the product decisions in the brief.

## When the verdict changes

A revision round of `idea-grill` can change the verdict of an idea, for example after `idea-research`.

- The `**Verdict:**` and `**Date:**` lines of `IDEA-DECISION.md` show the current verdict and the day it was given. A change edits these two lines.
- A revision round can also change a product decision. It edits the line in "Product decisions", and its dated section under "Revisions" gives the old choice, the new choice and the reason.
- Everything else in the file only grows. The "First verdict" section keeps the reason for the first verdict. Each change adds a dated section under "Revisions" with the reason for the new verdict.
- Only `idea-grill` gives a verdict. `idea-research` saves its findings in `1-idea/research/`, and a revision round of `idea-grill` reads them.
- After each verdict, `idea-grill` runs `setup-meta-repo`. While the stage is `idea`, `setup-meta-repo` makes the project file match the verdict: PROCEED has a `PROJECT.md`, and REFINE and PARK have none. After the stage `idea`, the Idea Engine does not change the verdict.
- REJECT is final. `setup-meta-repo` archives the repo, and no skill opens it again. To reopen a rejected idea, the owner unarchives the repo by hand and runs a revision round.

## Format versions

`IDEA-DECISION.md` and `PROJECT.md` are the files the Hub parses. Each has a `**Format:** 1` line under the title. Raise the number only when a change would break the Hub. Other files have no version.

## Setting up a meta repo

`setup-meta-repo <slug>` is an Idea Engine skill. It checks one meta repo, adds what is missing and removes a project file that the verdict does not allow, so it is safe to run again:

1. If `stubbornweb/<slug>-meta` does not exist, it makes it as a private repo and clones it to `~/www/<slug>-meta/`.
2. It adds `README.md`, `AGENTS.md`, `CLAUDE.md` and `.gitignore` if they are missing.
3. If the verdict is PROCEED and there is no `PROJECT.md`, it writes `PROJECT.md` with `**Stage:** idea`. If the verdict is REFINE or PARK and `PROJECT.md` has `**Stage:** idea`, it removes `PROJECT.md`.
4. If there is no decision record and no `PROJECT.md`, it asks the owner if this is an existing product that joins. It skips the question when the caller says that this is a new idea or a quick experiment. For a product that joins, it asks for the stage and the code repo, then writes `PROJECT.md` and the entry note in `README.md`.
5. If the verdict is REJECT, it asks the owner, then archives the repo.
6. It reports what it changed. If nothing was missing, it changes nothing.

Each time it writes `PROJECT.md`, it asks the owner for the client, or none, so that every client project has its `**Client:**` line.

`idea-grill` runs it at the start and after each verdict. The owner runs it for an existing product, and before a quick experiment from a pitch. After PROCEED, `project-brief` writes `1-idea/PROJECT-DESCRIPTION.md`.

## Prototype Engine input

The Prototype Engine runs inside a meta repo and writes into `3-prototype/`:

- In a meta repo with a project file, it reads `1-idea/PROJECT-DESCRIPTION.md` and sets the stage to `prototype`. A project has one prototype. A new direction replaces the old one, and git keeps the old one.
- In a meta repo with no project file, `--pitch "<one line>"` makes a quick experiment.

`prototype.yaml` has no `status:` key. Its `pitch:` key exists only for a quick experiment.

## Considered options

- **Stage and client as GitHub repo topics, with no file.** Rejected: topic names are always public, even on a private repo, so a client topic shows the client's name.
- **Project facts in `AGENTS.md`, or in `docs/agents/` with a link from `AGENTS.md`.** Rejected: `AGENTS.md` holds instructions, not data that changes. A linked file is still a file, plus a link that the Hub must follow.
- **YAML frontmatter for the format version.** Rejected: no file uses frontmatter today. Every file uses bold lines such as `**Verdict:**`.
- **A record of the tool version on every file.** Rejected: every skill must then know its version, and no repo has release tags.
- **A `--from` option on `prototype-engine new`.** Rejected: the current folder already shows where the brief is.
- **A `promote` command in the Prototype Engine.** Rejected: PROCEED is an Idea Engine verdict, and many projects have no prototype at PROCEED.
- **A separate skill for each event: one for PROCEED and one for an existing product that joins.** Rejected: two skills then make repos, and nothing repairs a meta repo that misses a file.
- **The project file written by `project-brief`.** Rejected: `project-brief` also runs for existing projects that have no brief.
- **A slug that changes with the product's public name.** Rejected: links, the Hub, `**Code:**` lines and local folders then break. ADR 0001 keeps brand names apart from working names for this reason.
- **Several prototypes per project.** Rejected: every tool must then ask which prototype to use.
- **Tools that change a wrong slug into a right one.** Rejected: a quietly changed slug breaks the rule of one slug everywhere.
- **A `**Verdict:**` line that never changes, with the current verdict in the newest dated section.** Rejected: the Hub and `setup-meta-repo` must then read sections, not one line.
- **A new decision record for each round.** Rejected: readers must then find the newest file.
- **Product decisions that only grow, with the changed ones in each dated section.** Rejected: readers must then merge the sections to find the current choice for each topic.
- **Product decisions in a separate file in `1-idea/`.** Rejected: one more file for each idea, and the reasons for a change then live in another file.
- **Product decisions only in the brief.** Rejected: the brief exists only after PROCEED, so the choices of a REFINE round have no place.
- **`project-brief` sorts each product decision into the sections of the brief that exist.** Rejected: two runs can then put the same choice in different sections.
- **`idea-research` also gives a verdict.** Rejected: the verdict logic is then in two skills, and the two copies can drift apart.
- **PROCEED can change only to REJECT.** Rejected: it forces a REJECT where REFINE is the honest verdict.
- **A project file that stays after the verdict moves away from PROCEED.** Rejected: the Hub then shows a project that is not a project.

## Consequences

- The Idea Engine gets the `setup-meta-repo` skill and loses `project-start`. `idea-grill`, `project-brief` and `client-intake` use the paths above.
- The Prototype Engine drops `PROTOTYPE_ENGINE_ROOT`, renames `--brief` to `--pitch`, rejects a wrong slug, runs inside a meta repo and sets the stage.
- `idea-grill` gets a revision round for an idea that has a decision record. `idea-research` sends its findings to that round. `setup-meta-repo` removes a project file that the verdict does not allow.
- ADR 0006 decides the format of a results check in `5-live/`.
