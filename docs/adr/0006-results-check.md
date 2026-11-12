# Results check

A results check records the real results of a live project and the owner's results decision: `GROW`, `CHANGE` or `STOP`. The Idea Engine skill `results-check` writes it. The owner runs the skill by hand, inside the meta repo, whenever they want.

## Which projects

- Own projects and client projects get results checks, from the `live` stage on.
- A project that skipped stages, such as an existing client site that started at `build`, gets results checks the same way.

## The skill

- `results-check` is an Idea Engine skill. Judging is the work of the Idea Engine, at the start and at the end of a project.
- Each run writes one new file, `5-live/<YYYY-MM-DD>-results.md`. Older checks stay as they are.
- The owner types each number and its source. The skill reads no analytics or payment API.
- The skill asks for the live site URL, because `prototype-engine publish` does not save it.
- The skill changes no stage and no engine. For `STOP`, the owner sets `closed` in `PROJECT.md` by hand (ADR 0003).
- The file goes in through a pull request, like every change (ADR 0004).

## Targets

`project-brief` writes a "Success" section in the brief: 1 to 3 numbers to reach, each with a date. For a client project, a number is what the client counts, such as leads or bookings. The results check shows each target next to its result.

## The file

```markdown
# Results check: Acme booking site

**Format:** 1
**Date:** 2026-12-04
**Period:** 2026-11-04 to 2026-12-04
**Live site:** https://acme.example
**Decision:** GROW

## Numbers

| Number   | Target           | Result | Source |
|----------|------------------|--------|--------|
| Bookings | 40 by 2026-12-31 | 31     | Stripe |

## Feedback

- <what customers or the client said, with where it came from>

## Practices

| Rule | Engine | Answer |
|------|--------|--------|
| <rule name, linked> | Design Engine | helped / did not help / not used here |

## Why this decision

<2 to 4 sentences>

## Next steps

- <what the owner does now. For CHANGE, the stage the work goes back to>
```

- The period starts on the day after the last check, or on the launch day for the first check.
- The Hub may read the `**Date:**` and `**Decision:**` lines of the newest check. So the file has a `**Format:** 1` line (ADR 0003).

## Practices

- The skill finds every rule with the line `_Worth testing. Source: <link>_` in the engine repos, with `gh`.
- For each rule, the owner answers "helped", "did not help" or "not used here". The skill writes the answers in the "Practices" table.
- The owner decides separately when a practice is proven (ADR 0005). The results check is the evidence for this decision.

## Considered options

- **Results checks for own projects only.** Rejected: client projects also test practices, and one way of working is simpler.
- **A new Live Engine.** Rejected: one more repo with its own CI, licence and `AGENTS.md`, for one skill.
- **A check at a fixed time after launch, with a reminder.** Rejected: the Hub only reads, so nothing can send the reminder.
- **One file that each check updates.** Rejected: older results are then only in the git history.
- **The skill reads GA4 and Stripe through their APIs.** Rejected for now: it needs credentials for each project and code for each tool. A client's accounts are often closed to the owner.
- **`STOP` sets the stage to `closed`.** Rejected: the owner sets `build`, `live` and `closed` by hand.
- **The skill opens the pull request that marks a practice proven or removes it.** Rejected: the check then changes engine repos, which mixes two jobs.
- **Targets set at the first check.** Rejected: targets set after the first results can follow the results.
- **No targets.** Rejected: the results decision then has nothing fixed to compare against.
- **The Hub ignores `5-live/`.** Rejected: the board needs the date and the decision of the newest check, and a later format change would mean updating the old checks.

## Consequences

- Nothing changes by this decision. After the map, these changes follow:
  - The Idea Engine gets the `results-check` skill.
  - `PROJECT-DESCRIPTION-FORMAT.md` gets the "Success" section, and `project-brief` asks for the targets.
- Any tool that counts sign-ups or feedback works, because the owner types the numbers in.
