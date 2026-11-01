---
name: bootstrap-learning-project
description: "Bootstrap a new learn-by-building project: scaffolds a git repo (roadmap, architecture, conventions, working agreement, glossary docs) and a Trello board (epics, backlog, a fully-detailed first milestone in Ready), then installs the pm-sync and senior-review skills so the learner can drive their own progress with Claude acting as PM + Senior/Architect. Use when the user wants to start learning a new language, tool, or topic by building a real project, and can give (or get) a Trello board URL and a git repo URL."
---

## What this does

Reproduces, for any topic, the setup pattern built for a Rust CLI TODO
project: Claude acts as **PM** (planning, Trello, roadmap) and
**Senior/Architect** (design, conventions, review); the user is the
**Junior** who implements every task themselves. Claude never writes the
project's own code — only the planning docs, process scaffolding, and the
two sub-skills that let the user keep driving progress afterward.

See `reference/rust-todo-cli-example/docs/` for the actual worked example
this is generalized from — read it for tone and level of detail before
writing a new project's docs from scratch.

## Inputs

Parse from the invocation args, or ask (`AskUserQuestion`) for whatever's
missing:

1. **Topic** — the language/tool/framework to learn (e.g. "Go", "Kubernetes", "Terraform", "React").
2. **Project idea** — something concrete to build that exercises it hands-on.
   If the user didn't give one, propose 2-3 options sized for a multi-week
   learning project (not a toy, not a rewrite-the-world) and let them pick,
   or take one they already have in mind.
3. **Trello board URL** — must already exist (ask the user to create an
   empty board if they haven't). Read it first — if it already has
   lists/cards beyond a bare empty board, stop and ask before
   restructuring it.
4. **Git repo URL** — must already exist (e.g. a fresh GitHub repo). Check
   it's actually empty before pushing; don't overwrite someone's existing
   project.
5. **Local path** — where to put the clone. Default to
   `~/Documents/projects/<repo-name>`; confirm if that directory already
   exists and isn't empty.

## Hard rule: never pre-build the learning

Do not write language-specific config, a dependency manifest, a "hello
world," linter/formatter config, or CI **on the learner's behalf** and
commit it for them — installing the toolchain and making those first
decisions **is** the first milestone. This was a direct correction learned
the hard way on the Rust project (it got built as pre-made scaffolding
first, then torn out and turned into tasks): anything that teaches
toolchain or config literacy stays a task, never a handed-over file. The
only files this skill creates directly are the planning docs, the PR
template, and the two sub-skills — nothing in the target language itself.

## Steps

### 1. Design the milestone roadmap

Write 5-8 milestones for the topic/project, same shape as the reference
example:
- **M0 is always environment & toolchain.** Install the toolchain, run its
  native "init"/"new" command by hand, make the first config decisions
  (formatter/linter if the ecosystem has one), stand up CI, do one full
  branch → PR → CI → review → merge rehearsal. Break it into granular
  tasks (4-6) — don't compress "install and configure everything" into one
  task; each config decision is its own task with its own acceptance
  criteria, same as `0.3`/`0.4` in the reference example.
- **Middle milestones build the project idea incrementally**: a
  bare-bones working version first (even if ephemeral/in-memory), then
  state/persistence if relevant, then UX polish, then tests/robustness,
  then advanced features. Adapt this shape to what the topic actually
  needs — don't force a mismatched template onto it (a Kubernetes learning
  project doesn't have "persistence" the way a CLI tool does; use
  judgment about what the real equivalent milestones are).
- **The last milestone is always packaging/sharing**: something you could
  hand to someone else, demo, or point at as proof of the skill.
- Each milestone needs: a one-sentence goal, 3-5 tasks (`<milestone>.<task>`
  IDs, e.g. `1.3`), and a "Learning focus" line naming the 2-4 concepts it
  exercises.

### 2. Write the docs (in the git repo)

In the local path: `git init -b main`, then write these, in the voice of
the reference example (terse, reasons-not-rules, teaching-oriented):

- `docs/ROADMAP.md` — the milestone plan from step 1, plus an empty "Retro
  log" section.
- `docs/ARCHITECTURE.md` — a concrete design proposal for the project
  idea: component layout, the one or two boundaries worth protecting
  (domain vs. I/O, or whatever the topic's real equivalent is), and *why*
  — tie it to what it teaches, not just what's "proper."
- `docs/CONVENTIONS.md` — formatting/linting policy (name the actual
  tools once the topic's ecosystem has obvious ones; if the choice itself
  is part of M0, say the decision gets made there — don't pre-decide it),
  testing approach, commit/branch/PR conventions
  (`feature/<task-id>-<slug>`, Conventional Commits referencing the task
  ID), and a policy on adding dependencies.
- `docs/WORKING_AGREEMENT.md` — roles (PM/Senior = Claude, Junior = the
  user), Trello board anatomy, Definition of Ready / Definition of Done,
  review turnaround, and a "How we progress" section naming `/pm-sync` and
  `/senior-review`.
- `docs/GLOSSARY.md` — empty, with a one-line note that it fills up during
  reviews.
- `README.md` — what the project is, links to the docs and the Trello
  board, and a note that there's no code yet on purpose.
- `.github/pull_request_template.md` — Trello card link, what changed, how
  it was tested, a DoD checklist mirroring `docs/WORKING_AGREEMENT.md`.

Commit (docs + PR template only — see the hard rule above), add the given
remote, push to `main`.

### 3. Install the sub-skills into the new project

Copy this repo's `.claude/skills/pm-sync/SKILL.md` and
`.claude/skills/senior-review/SKILL.md` into the target project's
`.claude/skills/pm-sync/` and `.claude/skills/senior-review/`, replacing
`{{TRELLO_URL}}`, `{{PROJECT_NAME}}`, and `{{LANGUAGE}}` with the real
values for this project. Commit and push this as its own commit.

### 4. Build the Trello board

- Create 6 lists if they don't already exist: `Roadmap (Epics)`,
  `Backlog`, `Ready`, `In Progress`, `In Review`, `Done`.
- One epic card per milestone in `Roadmap (Epics)`, each with a `Tasks`
  checklist listing that milestone's task IDs/names.
- Fully detail M0's tasks as cards in `Ready`: a description with
  Context / numbered Instructions / Learning objective, plus a separate
  `Acceptance Criteria` checklist — see the reference example's `0.1`-`0.5`
  cards for the exact shape and phrasing level.
- Note in the final report (not on every card) if card assignment isn't
  available through the Trello integration in use.

### 5. Report back

Summarize what was created (repo URL, board URL, the two installed
skills), and name the first task to start on. Don't start implementing it.

## Guardrails

- This skill never writes the target project's own code, even "just to
  get started." That line is the entire point of the workflow.
- If the Trello board or git repo already has real content, stop and ask
  before restructuring or overwriting anything.
- Keep the milestone count and task granularity close to the reference
  example (5-8 milestones, 3-6 tasks each) — thinner plans don't give the
  PM/Senior loop enough to work with, and much larger ones stop being
  "just-in-time" refined.
