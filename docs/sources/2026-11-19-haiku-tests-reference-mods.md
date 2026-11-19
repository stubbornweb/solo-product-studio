# Data: Haiku builds with Opus job sheets, against only Opus Medium

Measured on November 19, 2026, on the $20 Pro plan. One spec was built twice, on two branches, and each branch got its own blind review. The test prompts are in `~/www/home/docs/prompts/haiku-factory/`. The run notes and job sheets were in `~/www/.scratch/reference-mods/`. They were deleted after this file was written, so this file is the only copy of the numbers.

## The test

- Repo: lanka-community/arma-reforger-tools-mcp (Rust).
- Spec: issue #72 (from issue #20). Tickets: #73, #74, #75, #76, #77.
- #77 has `Needs real system: yes` (it needs the real Workbench). Opus built it in both runs, and it does not count in the score.
- **Run A** (branch `reference-mods-run-a`): for each ticket, an Opus helper writes a job sheet, and a Haiku helper builds from it. A ticket goes to Opus after 2 failed Haiku tries.
- **Run B** (branch `reference-mods-run-b`): an Opus Medium helper builds each ticket. No job sheet.
- **Win rule**, decided before the test. Run A wins only if all three are true: it uses 50% or less of run B's usage; it has no more serious problems than run B; 1 in 3 tickets or fewer go from Haiku to Opus.

## Result

Haiku **loses**: run A cost 80% of run B, and the limit is 50%. The other two rules were met.

| Rule | Run A | Run B | Met |
| --- | --- | --- | --- |
| Usage: A ≤ 50% of B | $7.83 | $9.80 | no (80%) |
| Serious problems, #73–#76 | 3 | 5 | yes |
| Tickets that went from Haiku to Opus | 0 of 4 | n/a | yes |

The cause is the job sheets, not Haiku. The Opus job sheets of #73–#76 cost $4.53. The Haiku builds cost $0.40. The Opus builds of the same tickets in run B cost $6.16. So a job sheet cost about 74% of an Opus build of the same ticket.

## Usage

`/usage` did not show the 5-hour % or the week %, so the comparison uses the session cost that `/usage` shows.

| Session | Total | Opus 5.5 | Haiku 5.5 | API time | Wall time |
| --- | --- | --- | --- | --- | --- |
| Spec and tickets (Opus High) | $2.25 | $2.20 | $0.05 | 8m 55s | 2h 43m 40s |
| Run A | $7.83 | $7.43 | $0.40 | 33m 7s | 45m 59s |
| Run B | $9.80 | $9.79 | $0.01 | 25m 56s | 1h 36m 44s |

Tokens from `/usage`:

| Run | Model | Input | Output | Cache read | Cache write |
| --- | --- | --- | --- | --- | --- |
| A | Opus | 3.9k | 136.1k | 8.9m | 544.0k |
| A | Haiku | 79.8k | 131.9k | 8.9m | 412.5k |
| B | Opus | 5.7k | 146.5k | 14.6m | 739.6k |
| B | Haiku | 76.5k | 8.0k | — | — |

In run B no helper used Haiku. Its Haiku tokens are probably Claude Code's own small background work.

### Cost of each part

Taken from the session logs (`~/.claude/projects/-home-yevhenii-www/`: run A `606eb2d1`, run B `d7c52fc6`, review A `3138f35a`, review B `f737c9e4`, spec `7e5fe6f1`). Each row is the session's `/usage` total, split by the tokens of each agent. Tokens are weighted by the usual price ratios: output 5× input, cache read 0.1×, cache write 1.25×. These weights give back the `/usage` totals to within 5%.

| Part | Run A | Run B |
| --- | --- | --- |
| Main session | $1.25 | $1.71 |
| Merges | done by the main session | $0.59 (5 Opus merger helpers) |
| #73 | $1.00 job sheet + Haiku | $1.46 |
| #74 | $1.89 job sheet + Haiku | $2.03 (two helpers, see "Run B stop") |
| #75 | $0.91 job sheet + Haiku | $1.55 |
| #76 | $0.73 job sheet + Haiku | $1.12 (two helpers) |
| Haiku builds #73–#76 | $0.40 together | — |
| **#73–#76 together** | **$4.93** | **$6.16** |
| #77 (Opus in both runs) | $1.65 | $1.31 |
| **Total** | $7.83 | $9.80 |
| Review (Opus High, 2 helpers) | about $4.5 | about $4.6 |

- With #77 and the main session left out, run A still cost 80% of run B ($4.93 against $6.16).
- One review cost about as much as half of run B. The two reviews together cost about the same as run B.

### Tokens of each helper

| Run | Helper | Model | Calls | Output | Cache read | Cache write |
| --- | --- | --- | --- | --- | --- | --- |
| A | main session | Opus | 39 | 19.1k | 2.00m | 62.8k |
| A | job sheet #73 | Opus | 16 | 19.6k | 0.90m | 90.4k |
| A | job sheet #74 | Opus | 24 | 43.2k | 1.74m | 142.7k |
| A | job sheet #75 | Opus | 15 | 21.4k | 0.66m | 79.1k |
| A | job sheet #76 | Opus | 13 | 15.6k | 0.52m | 71.9k |
| A | build #73 | Haiku | 27 | 24.2k | 2.27m | 110.7k |
| A | build #74 | Haiku | 29 | 45.6k | 2.82m | 133.6k |
| A | build #75 | Haiku | 24 | 28.3k | 1.52m | 81.6k |
| A | build #76 | Haiku | 34 | 25.7k | 2.24m | 86.6k |
| A | build #77 | Opus | 41 | 19.1k | 2.76m | 98.2k |
| B | main session | Opus | 54 | 24.6k | 3.31m | 72.6k |
| B | build #73 | Opus | 34 | 22.7k | 2.30m | 96.6k |
| B | build #74, stopped | Opus | 20 | 29.2k | 1.43m | 104.2k |
| B | build #74, finish | Opus | 19 | 6.6k | 1.02m | 72.0k |
| B | build #75 | Opus | 30 | 30.0k | 2.22m | 96.8k |
| B | build #76, stopped | Opus | 21 | 13.7k | 1.11m | 67.9k |
| B | build #76, finish | Opus | 9 | 2.7k | 0.28m | 41.3k |
| B | build #77 | Opus | 38 | 16.1k | 2.27m | 85.6k |
| B | 5 mergers, each | Opus | 4 | 0.8k | 0.08m | 20.3k |

## What the job sheets held

The job sheet prompt asks for "which functions, types or lines to change or add, and how". The Opus writers answered with full code.

| Job sheet | Lines | Lines in code blocks |
| --- | --- | --- |
| #73 | 272 | 85 |
| #74 | 688 | 415 |
| #75 | 499 | 318 |
| #76 | 313 | 99 |
| **Together** | 1,772 | 917 (52%) |

The job sheets also made the design choices that the review later flagged. Job sheet #73 adds `pub use bridge::Bridge;` and names the ADR 0004 conflict, but it follows spec #72. Job sheet #75 adds the `pending` input to `plan()`.

During run A, the main session told the owner "Opus writes no code in this run". The job sheets above show that this was not true.

## Build, one row for each ticket

| Ticket | A: Haiku tries | A: first try passed | A: to Opus | B: tries | B: first try passed |
| --- | --- | --- | --- | --- | --- |
| #73 | 2 | no | no | 2 | no |
| #74 | 1 | yes | no | 2 | no |
| #75 | 1 | yes | no | 2 | no |
| #76 | 2 | no | no | 1 | yes |
| #77 (excluded) | 0, Opus built it | n/a | no | 1 | yes |

- Failed merges: 0 in both runs. `cargo xtask ci` passed after each merge.
- Both failed Haiku tries were clippy lint failures (clippy is the Rust lint tool), each fixed in one edit. In #73, making `Bridge` public made `Address` and `App` too private. In #76, it was a `let-else` lint. No test failed.
- Size of the change (`git diff --shortstat main...<branch>`): run A 36 files, 2,246 lines added, 286 removed. Run B 35 files, 2,463 added, 268 removed.

## Review, one row for each ticket

Each run had its own review session (Opus High), with the same main prompt (`05-review.md`). A serious problem is a bug, a wrong behaviour, a weakened test, or a broken repo rule.

| Ticket | A: task done | A: quality (1–5) | A: serious | A: minor | B: task done | B: quality (1–5) | B: serious | B: minor |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| #73 | yes | 3 | 1 | 2 | yes | 4 | 0 | 2 |
| #74 | yes | 3 | 1 | 11 | yes | 3 | 3 | 5 |
| #75 | yes | 2 | 1 | 11 | yes | 4 | 1 | 4 |
| #76 | yes | 4 | 0 | 1 | yes | 3 | 1 | 1 |
| #77 (excluded) | no | 4 | 0 | 1 | no | 3 | 1 | 2 |

Run A quality reasons:

- #73: the test move is exact, but `pub use bridge::Bridge` breaks ADR 0004.
- #74: the error enum, the tests and the docs follow the rules, but it breaks the cancel rule (`docs/agents/rust.md`, Async) and has gaps in the description, layout and tests.
- #75: a mod can stay on disk for ever when the record write fails or the call is cancelled. Also repeated polling code, style slips and an unwrapped commit body.
- #76: small, tested and documented; only the two different stop errors are wrong.
- #77: it follows the Handler contract and the version rule, but its new Rust test only checks for text.

Run B quality reasons:

- #73: a clean move with no test changes, but the test helpers copy wire code and read the developer's environment. CI was run only at the branch tip, not at the #73 commit.
- #74: a good pure-plan design, but it breaks ADR 0004, `rust.md:28` and `rust.md:34`.
- #75: no rule breaks, but it uses tuples and index matching where named types belong.
- #76: it works for the tested case, but a half-downloaded folder can stay hidden, and the note names the wrong setting.
- #77: it follows the steps of #19, but a "complete" state that never resets breaks adding a removed mod again.

In both runs, #77 is "no" because the repo has no record of the real Workbench check.

## Limits of this data

- **The spec caused a serious problem in both runs.** Spec #72 says "`workbench` gives other Capabilities a way to make a bridge". This goes against ADR 0004 (a Capability makes only `tools()` public). Run A broke the ADR in #73, and run B broke it in #74.
- **The two reviews did not use the same helper prompts.** `05-review.md` fixes only the main prompt, and each review session wrote its own two helper prompts. Review A ran `cargo xtask ci` at the #73 commit; review B ran it only at the branch tip. Both reviews counted the break of the cancel rule in #74 as serious.
- **A "try" also counted lint failures.** See "Build". So "first try passed" says little about the code.
- **Run B stopped once.** The run B session ended at about 02:35 UTC, while the #74 and #76 helpers were building. The log does not say why. New helpers finished in the same worktrees. The tries for #74 and #76 come from the new helpers, and the stopped helpers' usage is in run B's cost.
- **The runs merged in different ways.** Run B used a separate Opus helper for each merge ($0.59). In run A, the main session merged.
- **Git hooks.** In run B, the #77 commit did not run the git hooks, because the helper could not set `core.hooksPath`.
- **Times (UTC).** Run A worked 01:19–01:57. Run B's prompt was pasted at 01:23, but its build work started at 02:06, after run A ended.

## Reference data used for the prediction

From `2026-11-17-aicodingdaily-model-data.md` (20 ticket-sized prompts on the $20 plan, with no planner and no helpers; score out of 20):

| Model | Effort | 5h used | Week used | 5h for each prompt | Score |
| --- | --- | --- | --- | --- | --- |
| Opus 5.5 | Medium | 68% | 6% | 3.4% | 19.25 |
| Haiku 5.5 | Medium | 10% | <1% | 0.5% | 18.00 |

The prediction was that Haiku alone uses about 15% of Opus Medium, and that run A uses more because of its Opus parts. Run A used 80%, and 61% of its Opus cost was the job sheets.

The proposals that follow from this data are in `2026-11-19-haiku-tests-retro.md`.
