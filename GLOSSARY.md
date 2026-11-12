# The studio

The shared words of the owner's one-person product studio: the tools that take an idea to a product, and the Hub that shows their results.

## Language

### People

**Owner**:
The one person who runs the studio, on any of their accounts.
_Avoid_: stubbornweb, YevheniiVolosiuk, Stubborned (these are accounts, not people)

**Client**:
A person or business that pays the owner for work, or may pay soon.
_Avoid_: customer (for the one who pays the owner)

**Customer**:
A person who buys from or uses the client's product or website.
_Avoid_: client, visitor (for the client's buyer)

### The studio and its parts

**Studio**:
The whole system of the owner: the engines, the meta repos, the Hub and the projects together. It is a plain working name, not a brand.
_Avoid_: ecosystem, system, Stubborn Web

**Engine**:
A set of tools that does the work of one stage and keeps the best current practices for that stage. Each engine works alone, without the Hub.

**Idea Engine**:
The engine that finds ideas, judges them and writes the brief.
_Avoid_: Finding Engine, Product Engine, product skills

**Design Engine**:
The engine that designs a project before it is prototyped.

**Prototype Engine**:
The engine that builds a prototype from a brief.

**Practice**:
A way of working that one engine follows. It is "proven" or "worth testing".

**Source**:
An article, video, book, case or piece of code that a practice comes from.

**Practice proposal**:
A proposal from the `learn-from-source` skill whose target is an engine. It changes how the engine works, and it has the label "proven" or "worth testing" (ADR 0005). The owner approves it or rejects it.

**Reference**:
An example, such as a website or a prompt, that one project uses for its design or build, with what to take from it. It does not change any engine. A link that no project uses yet is not a reference.
_Avoid_: practice, pattern (for an example that only one project uses)

**Work sample**:
A real piece of the client's current work, such as an order, a form, a price list or their current site, with the customers' personal data removed. The AI uses it to understand the work.
_Avoid_: example (an example site the client likes is a reference)

**Pattern**:
A reusable part of a site, such as an interaction or a layout, that an engine keeps for many projects.
_Avoid_: reference (for a part that many projects use)

**Hub**:
The part of the studio that shows the results of every engine in one place. It reads the engines' files and never changes them.
_Avoid_: Project Hub, Cockpit

### Ideas and projects

**Idea**:
A possible product or piece of work that does not have the PROCEED verdict.

**Decision record**:
The written result of judging an idea: its verdict, the reasons, the product decisions and the open assumptions.

**Product decision**:
A choice the owner makes in the grilling about what the product is, such as who pays, the price, the input, the data rules, or what the first version does. It is not an assumption: an assumption is a claim that the idea depends on and that the outside world can prove wrong.
_Avoid_: the owner's answer, first version (for the list of these choices)

**Revision round**:
A new round of judging an idea that already has a decision record. It can change the verdict.

**Verdict**:
The result of judging an idea: PROCEED, REFINE, PARK or REJECT.
_Avoid_: state, status

**Project**:
An idea that has the PROCEED verdict, or an existing product that joins the studio. A project that comes from an idea keeps the slug of its idea.

**Meta repo**:
The private repo of one idea. It is made when work on the idea starts. It holds the notes, research, sources and the output of every stage, and it stays the same repo when the idea becomes a project.
_Avoid_: project repo, idea folder, idea workspace

**Code repo**:
The repo that holds only the code of one product. It is made when the build starts.

**Project file**:
The small file in a meta repo that marks a project and names its stage, its client and its code repo.
_Avoid_: status file

**Client project**:
A project that a client pays for.

**Diagnosis**:
A paid first step for a client. The owner studies the client's work and returns a ranked list of what to fix, with the money math and a demo. Its fee counts toward the build.
_Avoid_: assessment, audit

**Care plan**:
A monthly paid plan after a client project is live: the owner keeps the product working and makes small changes.
_Avoid_: retainer, maintenance plan, support contract

**Own project**:
A project whose product the owner owns.

**Product**:
What a project delivers for customers to use: a website, an app, a tool or an AI workflow.
_Avoid_: deliverable

**Prototype**:
A rough working version of a product, made to test an idea or to show a client.
_Avoid_: project (for a prototype)

**Quick experiment**:
A prototype made from a pitch, before its idea becomes a project. It is kept in the meta repo of its idea.

**Site**:
The website files of a prototype or a product.

**Live site**:
A site that customers can open on the internet.
_Avoid_: website, production site

**Results check**:
A dated record of the real results of a project after launch, with the owner's decision on what comes next. The owner makes one whenever they want, for an own project or a client project.
_Avoid_: report, review

**Results decision**:
The owner's decision in a results check: GROW, CHANGE or STOP.
_Avoid_: verdict (for a live project)

**Stage**:
The step an idea or a project is in now: idea, design, prototype, build, live or closed. An idea before PROCEED is always in the idea stage. Each engine serves one stage.
_Avoid_: status, mode

**Slug**:
The one short name of an idea. The idea keeps it when it becomes a project, and every part of the studio uses the same slug for it.

**Brief**:
The written description of a project: what is built, for whom, and what is out of scope. Only a project has a brief.
_Avoid_: project description, pitch

**Pitch**:
A one-line description of a quick experiment that has no decision record.
_Avoid_: brief
