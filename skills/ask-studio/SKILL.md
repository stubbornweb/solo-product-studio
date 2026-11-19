---
name: ask-studio
description: Ask which studio skill or flow fits your situation. A router over the skills of the studio engines.
disable-model-invocation: true
---

# Ask Studio

The owner tells you the situation. Name the next skill, the file it writes, and the step after it.

Before you say what a skill does, or recommend that the owner skip a step, run `git pull` on `main` in the local clone of its engine and read that skill's `SKILL.md` there. The plugin cache can be older than `main`. The summaries in this map are only for orientation.

| Engine | Local clone | Skill files |
|---|---|---|
| Idea Engine | `~/www/idea-engine` | `skills/product/<skill>/SKILL.md` |
| Prototype Engine | `~/www/prototype-engine` | `skills/<skill>/SKILL.md` |

Client files go into `~/www/clients-meta/`. The files of an idea or a project go into its meta repo, `~/www/<slug>-meta/`.

## Start situations

Each one joins the main flow below.

- **A client post or message** (a job post, a client's message or reply) → `client-intake`.
- **A client accepted the diagnosis** option of a proposal → `client-diagnosis`.
- **A raw idea**, or a pain in `PAINS-INDEX.md` that reached 3 clients → `idea-grill`.
- **New facts for an idea that has a verdict** (research findings, a new fact, a PARK trigger that fired) → a revision round of `idea-grill`.
- **An existing product joins the studio** → `setup-meta-repo`, then `project-brief`.
- **A quick experiment with no project** → `setup-meta-repo` (tell it that this is a quick experiment), then `prototype-engine new --pitch "<one line>"` in the meta repo.

## The main flow: client → prototype

1. **`client-intake`** writes `<client-slug>/INTAKE.md` and `<client-slug>/PROPOSAL.md`, and adds rows to `PAINS-INDEX.md`, all in `clients-meta`. Next: a pain at 3 clients → `idea-grill`; the client accepts the diagnosis → `client-diagnosis`.
2. **`client-diagnosis`** writes `<client-slug>/DIAGNOSIS.md` and `<client-slug>/demos/`, adds the build to `PROPOSAL.md`, and adds rows to `PAINS-INDEX.md`. It runs `prototype-engine` for a demo of a site or an app. Next: a pain at 3 clients → `idea-grill`.
3. **`idea-grill`** writes `1-idea/IDEA-DECISION.md` with the verdict. It runs `setup-meta-repo` at the start and after the verdict. Next: PROCEED → `project-brief`; REFINE, or a load-bearing assumption still open → `idea-research`; PARK or REJECT → stop.
4. **`idea-research`** writes `1-idea/research/RESEARCH-QUESTIONS.md` and one findings file for each kill-question in `1-idea/research/`. Next: a revision round of `idea-grill`.
5. **`setup-meta-repo`** makes or repairs the meta repo `stubbornweb/<slug>-meta`. After PROCEED it writes the project file `PROJECT.md`. After REJECT it archives the repo. `idea-grill` usually runs it for the owner.
6. **`project-brief`** writes the brief, `1-idea/PROJECT-DESCRIPTION.md`. It needs `PROJECT.md` and a PROCEED verdict.
7. **`prototype-engine`**: run `prototype-engine new` in the meta repo. It writes `3-prototype/`, with the QA note in `3-prototype/qa/RESULT.md`, and sets the stage in `PROJECT.md` to `prototype`.

## Loaded by prototype-engine

The owner does not call these. `prototype-engine` loads only the ones that the prototype needs.

- `web-design`: the visual direction.
- `web-patterns`: reuse a pattern from the engine's pattern registry.
- `web-motion`: motion and scroll choreography.
- `web-3d`: a 3D scene, only when 3D shows what words and images cannot.
- `web-video`: hero or background video.
- `web-visual-qa`: check the rendered site in a real browser.
- `web-production`: promote a prototype to production quality, only for a client delivery or when the owner asks.

## Matt's skills underneath

- `idea-grill` runs `/grilling` for the interview.
- `idea-research` runs `/research` once for each kill-question.

## Exit: the build stage

This map ends at the prototype. For the build stage (spec, tickets, implement), run `/ask-matt`.
