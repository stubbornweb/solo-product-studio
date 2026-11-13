# How proposals enter an engine

The owner's general `learn-from-source` skill turns a source into proposals for any repo (ADR 0001 in `stubbornweb/home`). This ADR holds the extra rules for a proposal whose target is an engine: what the skill reads, the "worth testing" label, and how a practice becomes proven. An engine uses a "worth testing" practice in real projects, because only use can prove it.

## What the skill reads for an engine

- An engine has no meta repo of its own. Its goal is the studio's goal: the system brief, `1-idea/PROJECT-DESCRIPTION.md` in `solo-product-studio-meta`.
- The engine list is `.claude-plugin/marketplace.json` in this repo. An idea that fits another engine names that engine. An idea that fits no engine names the missing stage or tool, and the owner can start it as a new idea with `idea-grill`.
- The skill reads both on GitHub with `gh`, or from the local clone after `git pull` on `main`. An old copy gives old answers.

## The proposal

A proposal for an engine adds one part to the parts from ADR 0001: the label "proven" or "worth testing". A practice from a source is "worth testing" unless the source check rates it "measured" and the owner agrees that it is proven.

Each approved proposal is one pull request on the engine repo. The owner merges it.

## "Worth testing" and "proven"

- A "worth testing" practice lives in its normal rule file, with one label line: `_Worth testing. Source: <link>_`.
- A rule without this label is proven.
- The owner decides when a practice is proven, at any time. If it helped, a pull request removes the label line. If it failed, a pull request removes the rule.

## Considered options

- **"Worth testing" practices in a separate file that no skill reads.** Rejected: nothing uses them, so nothing proves them.
- **A practice becomes proven after a fixed number of projects.** Rejected: it needs a record of use per project.
- **A new meta repo for each idea that fits no engine.** Rejected: the skill then writes outside the target repo, and one source rarely justifies a whole engine.

## Consequences

- The results check asks the owner about each "worth testing" rule (ADR 0006).
