# Data: AI Coding Daily model scores and subscription usage

Taken on November 17, 2026, from three pages by Povilas Korop (AI Coding Daily). Only the Claude rows and the method text are copied. The pages change often, so check the live pages before you use these numbers for a new decision.

- Leaderboard: https://aicodingdaily.com/leaderboard (last update November 17, 2026)
- Method: https://aicodingdaily.com/article/llm-coding-leaderboard-my-methodology-and-scoring-formulas (November 11, 2026)
- Subscription usage: https://aicodingdaily.com/subscription-usage (last update November 13, 2026)
- Effort levels: https://aicodingdaily.com/compare/effort-levels (last update November 14, 2026). It joins the two pages above.

## Method, quoted

- "Each model receives 8 projects to be tested, with a total of 33 prompts."
- "The projects cover Laravel/PHP, React + TypeScript, Flutter/Dart, Go and Python, and every one of them starts from existing code, not a blank folder."
- "The model is the implementer. One coding agent (Claude Code, Codex CLI, OpenCode, Cursor…) gets one prompt and a repository, and does the work itself."
- "It's not orchestration. There are no subagent swarms, no multi-agent pipelines, and no human feedback in between."
- "The tasks are not long-running. The median task takes 4–11 minutes, depending on the project. These are focused, ticket-sized jobs"
- "No retries or coaching. Every prompt starts in a fresh copy of the project, and the model never sees the hidden tests or the scoring rubric."
- "The same prompt is launched several times, each in a fresh copy of the project". A consistency penalty is subtracted when runs on the same project are far apart.
- "Prices are calculated with API costs".
- Subscription usage: "Every model and effort level ran the same four coding prompts, repeated five times (20 prompts in total)". "The usage meters in each provider's settings were read before and after the run." "Providers show usage in whole percents, so <1% means the weekly meter did not move."

## Leaderboard, Claude rows (all in Claude Code)

Total is out of 90. Cost is the API price for each prompt.

| Rank | Model (effort) | Total | Cost | Time | Laravel /20 | React-TS /20 | Bug finding /10 | Python /20 | Edge cases /20 | Penalty |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | Opus 5.5 (High) | 84.95 | $0.83 | 03:42 | 18.68 | 19 | 9.1 | 18.17 | 20 | 0 |
| 2 | Fable 5.2 (Medium) | 84.7 | $1.42 | 03:03 | 19.13 | 19.5 | 9.52 | 17.8 | 18.75 | 0 |
| 6 | Opus 5.5 (Medium) | 83.15 | $0.57 | 02:20 | 18.95 | 18.5 | 7.98 | 18.5 | 19.5 | −0.28 |
| 8 | Sonnet 5.5 (High) | 80.58 | $0.34 | 01:53 | 18.68 | 18.5 | 7.6 | 16.83 | 19 | −0.03 |
| 11 | Opus 5.5 (Low) | 78.19 | $0.37 | 01:21 | 17.37 | 18.17 | 6.63 | 18.1 | 18.5 | −0.58 |
| 16 | Haiku 5.5 (High) | 76.3 | $0.05 | 03:37 | 17.3 | 18 | 7.22 | 17.23 | 17.25 | −0.70 |
| 17 | Haiku 5.5 (Medium) | 75.84 | $0.03 | 02:37 | 17.32 | 17 | 7.09 | 17.43 | 17 | 0 |
| 19 | Sonnet 5.5 (Medium) | 75.44 | $0.20 | 00:58 | 17.88 | 18.33 | 6.18 | 16.63 | 17.25 | −0.83 |

## Subscription usage, Anthropic, 20 prompts

"5h" is the part of the 5-hour limit used. "Week" is the part of the weekly limit used. Score is out of 20.

$20 plan:

| Model | Effort | 5h | Week | Score | Time | API price | Input | Cached | Output |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Opus 5.5 | Low | 46% | 4% | 19.00 | 29m 47s | $7.89 | 461.3K | 3.2M | 178.1K |
| Opus 5.5 | Medium | 68% | 6% | 19.25 | 47m 14s | $11.25 | 584.9K | 5M | 279K |
| Opus 5.5 | High | 99% | 9% | 19.75 | 70m 31s | $16.24 | 763.7K | 7.8M | 428.3K |
| Sonnet 5.5 | Medium | 33% | 3% | 15.50 | 22m 06s | $4.08 | 480.6K | 3.2M | 183.8K |
| Sonnet 5.5 | High | 53% | 4% | 18.25 | 38m 40s | $5.89 | 609.8K | 4.3M | 302.7K |
| Haiku 5.5 | Medium | 10% | <1% | 18.00 | 58m 04s | $0.73 | 1.1M | 12.1M | 765.1K |
| Haiku 5.5 | High | 13% | 1% | 18.50 | 83m 12s | $1.33 | 1.5M | 17.6M | 1.1M |

$100 plan:

| Model | Effort | 5h | Week | Score | Time | API price | Input | Cached | Output |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Fable 5.2 | Medium | 67% | 5% | 19.50 | 63m 48s | $29.40 | 766.5K | 4.2M | 407.6K |
| Opus 5.5 | Low | 18% | 1% | 19.00 | 61m 48s | $14.46 | 745.5K | 2.7M | 398K |
| Opus 5.5 | Medium | 32% | 2% | 19.50 | 97m 09s | $21.10 | 977.1K | 5M | 614.2K |
| Opus 5.5 | High | 48% | 3% | 19.75 | 131m 06s | $27.33 | 1.2M | 7.4M | 814.4K |
| Sonnet 5.5 | Medium | 6% | 1% | 15.50 | 23m 32s | $3.82 | 450.6K | 2.6M | 175.3K |
| Sonnet 5.5 | High | 10% | 1% | 18.25 | 32m 45s | $5.49 | 579K | 3.6M | 282.3K |
| Haiku 5.5 | Medium | 2% | <1% | 18.00 | 58m 31s | $0.66 | 1.1M | 8.9M | 713.6K |
| Haiku 5.5 | High | 3% | <1% | 18.50 | 78m 00s | $1.02 | 1.4M | 16.3M | 1M |
