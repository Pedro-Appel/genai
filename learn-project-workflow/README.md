# learn-project-workflow

A reusable "learn-by-building" setup: Claude acts as **PM** (planning,
Trello, roadmap) and **Senior/Architect** (design, conventions, code
review); you're the **Junior** who implements every task yourself. This
repo packages that pattern as a Claude Code skill, generalized from the
first project it was built for (a Rust CLI TODO manager — kept as a worked
example below).

Three skills live in `.claude/skills/`:

- **`bootstrap-learning-project`** — the entry point. Give it a topic, a
  project idea, a Trello board, and a git repo, and it scaffolds the docs,
  the Trello board, and installs the other two skills into the new project.
- **`pm-sync`** — installed into *each new project* this bootstraps. Run it
  there to reconcile Trello against real progress and refine the next
  milestone into Ready.
- **`senior-review`** — installed into each new project too. Run it there
  to get a PR reviewed against that project's own architecture/convention
  docs, explained to teach rather than just fix.

## What it does NOT do

It never writes the target project's own code — not even a starter
"hello world," a dependency manifest, or a linter config. Installing the
toolchain and making those first calls **is** the first milestone
(`M0`). This repo only ever produces: planning docs, a PR template, Trello
structure, and the two process skills. See the hard rule in
`bootstrap-learning-project/SKILL.md` for why — it's there because the
first time this was built, those files got pre-made and then had to be torn
back out.

## How to use it

### One-time setup

You need:
1. An **empty Trello board** (create one, grab its URL).
2. An **empty git repo** (e.g. a fresh GitHub repo, grab its clone URL).
3. A topic you want to learn, and ideally a project idea to learn it
   through (the skill will help pick one if you don't have one).

### Running the bootstrap

From inside this repo (so Claude Code can see the skill), or after
copying/symlinking `.claude/skills/bootstrap-learning-project` into
`~/.claude/skills/` for global access, invoke:

```
/bootstrap-learning-project topic="<language/tool>" project="<what to build>" trello="<board url>" repo="<git url>"
```

Any argument you leave out, Claude will ask for. It will then:
1. Design a 5-8 milestone roadmap for your topic/project.
2. Write `docs/ROADMAP.md`, `docs/ARCHITECTURE.md`, `docs/CONVENTIONS.md`,
   `docs/WORKING_AGREEMENT.md`, `docs/GLOSSARY.md`, `README.md`, and
   `.github/pull_request_template.md` into a local clone of your repo, and
   push them.
3. Install `pm-sync` and `senior-review` into that new project's
   `.claude/skills/`.
4. Build out your Trello board: `Roadmap (Epics)`, `Backlog`, `Ready`,
   `In Progress`, `In Review`, `Done` — with every milestone as an epic,
   and the first milestone's tasks fully detailed in `Ready`.
5. Tell you the first task to start on.

### After bootstrapping

In the new project, you drive the loop yourself:
- Pick a card from `Ready`, implement it on its own branch, open a PR.
- Run `/senior-review` there to get it reviewed — as teaching, not just a
  diff of fixes.
- Once merged, run `/pm-sync` there to update the board and pull the next
  task into `Ready`.
- Repeat until the roadmap's done.

## Worked example: Rust CLI TODO manager

This is the project the pattern was built for — a concrete reference for
tone and structure, kept at
[`reference/rust-todo-cli-example/docs/`](reference/rust-todo-cli-example/docs/):

- **Topic**: Rust. **Project**: a CLI TODO list manager.
- **Trello board**: `CLI-Manager` (6 lists, 7 milestone epics, M0's 5 tasks
  fully detailed in `Ready` with acceptance-criteria checklists).
- **Repo**: `cli-todo-manager`, docs-only on the first commit, no `Cargo.toml`
  or `src/` — `cargo init` and all toolchain config are task `0.1`-`0.5`.
- **Milestones**: `M0` Environment & Hello World → `M1` Core Domain &
  In-Memory CRUD → `M2` Persistence → `M3` CLI UX → `M4` Testing &
  Robustness → `M5` Advanced Features → `M6` Packaging & Distribution.

Reading `reference/rust-todo-cli-example/docs/ARCHITECTURE.md` and
`CONVENTIONS.md` is the fastest way to see the level of detail and voice
`bootstrap-learning-project` aims to reproduce for a new topic.

## Placeholders used in the sub-skill templates

`pm-sync/SKILL.md` and `senior-review/SKILL.md` in this repo contain
`{{PROJECT_NAME}}`, `{{TRELLO_URL}}`, and `{{LANGUAGE}}` placeholders —
`bootstrap-learning-project` fills these in with real values when it
copies the files into a new project. If you ever copy them by hand, do
the same substitution yourself.
