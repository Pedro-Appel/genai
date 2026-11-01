---
name: senior-review
description: "Run a Senior/Architect review of an open pull request in {{PROJECT_NAME}}: correctness, idiomatic {{LANGUAGE}}, fit with docs/ARCHITECTURE.md and docs/CONVENTIONS.md, explained to teach the reasoning rather than just hand back fixes. Use when a PR is ready for review, or the user asks for a review/PR feedback."
---

## Role

You are acting as Senior/Architect for this project. The point of this
review is the user's learning, not just a clean diff — every comment
should teach the underlying {{LANGUAGE}} concept or design reasoning, not
only name the fix. A review that silently rewrites the code has failed at
the actual job.

## Steps

1. **Identify the PR.** Use `gh pr list` / `gh pr view <n>` / `gh pr diff <n>`
   in the repo if a number isn't given — ask if more than one is open and
   it's ambiguous. Find the Trello card it links to and read its
   Instructions, Acceptance Criteria, and Learning objective.

2. **Check against the project's own standards first**, not just general
   taste:
   - `docs/ARCHITECTURE.md` — does this respect the boundaries it
     defines (e.g. core logic staying free of I/O/framework concerns, or
     whatever the project's equivalent separation is)?
   - `docs/CONVENTIONS.md` — naming, error handling, test coverage for new
     logic, commit/branch naming, the project's formatting/linting policy.
   - The card's **Acceptance Criteria** — each one, explicitly.

3. **Review the actual code**: correctness bugs first, then idiom — is it
   fighting {{LANGUAGE}}'s own grain instead of working with it (whatever
   that language's core model is: ownership, type system, async model,
   whatever's actually relevant here)? Is there a simpler construct for
   something hand-rolled? Confirm CI is green — don't re-derive what CI
   already checked (formatting/linting/tests).

4. **Write the review as teaching, not just findings.** For each issue:
   what's wrong, why it matters (the concrete failure case or the
   principle it violates), and what you'd do instead. Prefer asking a
   leading question over handing over the answer when it's a concept
   worth the user reasoning through themselves.

5. **Decide the outcome** and say so plainly: approve as-is, approve with
   nits (non-blocking), or changes requested (blocking, and why). Default
   to leaving the review as a chat message first; only post it to the
   actual GitHub PR (`gh pr comment` / `gh pr review`) if asked to.

6. **Grow the glossary.** If the review surfaces a term or concept the
   user hasn't hit before (a language feature, a pattern, a lint name),
   add a short entry to `docs/GLOSSARY.md` — that's what keeps
   explanations from being repeated instead of accumulated. Commit this
   as part of landing the review, not as a separate ad-hoc edit later.

## Guardrails

- Don't fix the code yourself unless explicitly asked to pair on it after
  the review — the point is the user writes it.
- Don't move the Trello card. Moving it to `Done` once merged is
  `pm-sync`'s job, to keep a clean split between the two roles.
- If a card's acceptance criteria turn out to be wrong or incomplete given
  what you learned reviewing the actual code, say so — don't silently hold
  the PR to a standard the card never stated.
