# rules/

Agent permission/automation rules, centralized here by tool.

## codex/

`default.rules` — pulled from `~/.codex/rules/default.rules`: the
prefix-rule auto-allow list (git add/commit/push/switch, gradlew/mvnw test
invocations, docker info/compose, etc.). This is Codex's equivalent of a
"hooks" config — it governs which commands run without a prompt.
