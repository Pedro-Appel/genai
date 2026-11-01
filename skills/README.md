# skills/

Agent skills, centralized here — flat, one folder per skill, no
per-tool grouping. A skill is a skill regardless of which agent
(Codex, Claude, or anything else) originated or consumes it; the tool's
real skills directory becomes a symlink into the matching folder here
(see root `README.md` for the exact commands).

Currently populated from Codex (`~/.codex/skills/`, everything except
`.system/`, which is Codex's own built-in set and stays vendor-managed
there):

`bce-diagrams`, `continuous-testing`, `drawio`, `enterprisifier`,
`java-cli-app`, `java-cli-script`, `java-distiller`, `mermaid`,
`microprofile-server`, `python-to-java`, `readme`, `showtime`,
`web-components`, `web-static`, `zargs`, `zb`, `zcfg`, `zcl`, `zunit`.

No Claude Code skills exist here yet — there's no user-authored one on
this machine to add (`~/.claude/skills/synced/` is only Anthropic's own
account-synced catalog, vendor content, not centralized). The day a real
custom Claude skill shows up, it lands in this same flat folder, named
like any other — not under a `claude/` subfolder.
