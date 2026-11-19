# Retro: Haiku builds with Opus job sheets

Written on November 19, 2026, after the test in `2026-11-19-haiku-tests-reference-mods.md`. That file has the numbers. This file has what to change in the test setup, most serious first, and the options for the next test. The test prompts are in `~/www/home/docs/prompts/haiku-factory/`.

**Short answer:** Haiku was not the problem. In run A, Opus wrote most of the code inside the job sheets, so Opus still did the expensive work. The spec also caused one serious problem in both runs. The two reviews did not use the same rules, so their numbers do not compare exactly.

## Proposals

### 1. The job sheet format makes Opus write the code

- **Evidence:** 917 of the 1,772 lines in the 4 job sheets are code blocks. A job sheet cost about 74% of an Opus build of the same ticket. Step 3 of the job sheet in `03-implement-spec-haiku.md` asks for "which functions, types or lines to change or add, and how".
- **Fix:** limit a job sheet to about 60 lines, with type and function signatures only and no function bodies. Or drop the job sheet (option 1 below).

### 2. The spec went against an ADR

- **Evidence:** spec #72 says "`workbench` gives other Capabilities a way to make a bridge". ADR 0004 of the test repo says a Capability makes only `tools()` public. Both runs broke ADR 0004, and both reviews counted it as serious.
- **Fix in the spec step (`01-to-spec.md`, or the to-spec skill):** check each implementation decision against `docs/adr/`. Then follow the ADR, or write the ADR change first.
- **Fix in the test repo:** this is a mechanical rule, so add an ast-grep rule to `lint/ast-grep/` (ast-grep is a pattern check that `cargo xtask ci` already runs). The rule: a Capability's `mod.rs` has only one `pub use`, `tools::tools`. Then CI catches the break, not the reviewer.

### 3. The two reviews did not use the same helper prompts

- **Evidence:** `05-review.md` fixes only the main prompt. Each review session wrote its own two helper prompts. Review A ran `cargo xtask ci` at the #73 commit; review B ran it only at the branch tip.
- **Fix:** put the two helper prompts (Standards and Spec) in `05-review.md` word for word. Each prompt must:
  - give the rules for serious and minor problems;
  - tag each finding with its ticket;
  - run CI at each ticket commit.

### 4. "Failed try" counted lint failures

- **Evidence:** both failed Haiku tries were clippy lint failures, each fixed in one edit. No test failed.
- **Fix:** count a try as failed only when a test fails. The helper fixes lint failures without counting them.

### 5. The two runs did not have the same conditions

- **Evidence:**
  - Run B lost its #74 and #76 helpers when its session ended, and new helpers finished their work.
  - Run B used a separate Opus helper for each merge ($0.59), but run A's main session merged by itself.
  - In run B, the #77 commit did not run the git hooks.
- **Fix in `03` and `04`:** name who merges, in the same way for both runs. Run `git config core.hooksPath .githooks` once, before the first helper starts.

### 6. The review costs about as much as half a build

- **Evidence:** one review cost about $4.5. Run B cost $9.80.
- **Fix:** for normal work, review only big changes, as the model table in `~/.claude/CLAUDE.md` already says. For this test, keep both reviews.

## Options for the next test

### Option 1: run C, Haiku builds straight from the tickets (recommended)

- **What it is:** no job sheets. Haiku builds each ticket from the ticket text, and the ticket goes to Opus after 2 failed tries. Proposals 2–5 are fixed first.
- **Pros:** it removes the job sheet cost ($4.53 in run A). Expected cost: about $1.3 for the main session, $0.5–1.0 for Haiku, plus any ticket that goes to Opus. That can be under 50% of run B. In the AI Coding Daily data, Haiku without a planner scored 18 of 20.
- **Cons:** one more run and one more review, about $6–7 together. Haiku makes the design choices by itself, so it may need more tries.
- **Why this is the best choice:** the data shows that the cost is in the job sheet, not in Haiku, and this run tests exactly that.

### Option 2: run A again, with short job sheets

- **What it is:** the same run A, with job sheets of about 60 lines and no function bodies.
- **Pros:** Opus still makes the design choices.
- **Cons:** each job sheet writer read 0.5–1.7m cache tokens before it wrote a line. Short job sheets will probably still cost about $2–3, so the 50% rule is hard to meet.

### Option 3: stop now

- **What it is:** accept "Haiku loses" and keep Opus Medium for tickets.
- **Pros:** no more cost.
- **Cons:** the result tests a job sheet format that does the Opus work twice. It does not test Haiku.
