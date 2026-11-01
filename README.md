# genai

Central, version-controlled home for everything that configures the AI
coding agents on this machine — skills, rules, and (once any exist) hooks —
plus reusable learning workflows. The actual per-agent config directories
(`~/.codex/`, `~/.claude/`, …) point here via symlinks, so there's one
place to edit and one git history for all of it.

## Layout

- [`skills/`](skills/README.md) — agent skills, flat — one folder per
  skill, regardless of which tool it came from or runs in.
- [`rules/`](rules/README.md) — agent permission/automation rules.
- `hooks/` — not created yet; no agent on this machine has a real hooks
  config at the moment (Codex has no hooks concept, Kiro's is empty,
  Claude's isn't set). Add it the same way as `skills/`/`rules/` the day
  one exists.
- [`learn-project-workflow/`](learn-project-workflow/README.md) — a
  separate, self-contained skill set for bootstrapping learn-by-building
  projects (PM/Senior roles, Trello + git scaffolding). Not an agent
  config snapshot — kept here because it's still "genai workflows."

Only genuinely user-authored content lives here — no vendor/built-in
agent defaults (e.g. Claude's account-synced skill catalog, Codex's
`.system/` skills, Kiro's bundled app features), and no project-specific
`AGENTS.md`/`KNOWLEDGE.md` docs that belong to one repo, not the whole
machine.

## Centralizing via symlinks

Move each real skill/rule out of the tool's own directory and into this
repo, then symlink it back:

```sh
# Codex skills — repeat per skill, or loop over all of skills/*
# (skills/ is flat and tool-agnostic; this just happens to be where
# Codex's skills currently live)
for d in ~/Documents/projects/genai/skills/*/; do
  name=$(basename "$d")
  rm -rf ~/.codex/skills/"$name"          # the tool's own copy
  ln -s "$d" ~/.codex/skills/"$name"       # now a symlink into genai
done

# Codex rules
rm -f ~/.codex/rules/default.rules
ln -s ~/Documents/projects/genai/rules/codex/default.rules ~/.codex/rules/default.rules
```

After symlinking, edit the files under `~/Documents/projects/genai/`
(or through the symlink — same inode) and commit from here; the agent
picks up changes immediately since it's reading through the symlink.

This repo does **not** perform that migration automatically — the steps
above haven't been run against the live `~/.codex/` config yet. Run them
yourself, or ask for them to be run, once you're ready to cut over.
