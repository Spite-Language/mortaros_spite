# Working on the canon of decisions

This repository holds `DECISIONS.md` for every Spite repository, Mortaro's inbox and the questions only he can
answer. These rules are for an agent working here or recording a decision from any other repository.

## The inbox

- **`mortaros_notes.md` is an inbox and nothing else.** Mortaro drops notes there. An agent moves each one into the
  specification and the docs (the rule on the spec page, the teaching on the docs page, a row in `DECISIONS.md`
  and, until it is built, a line in the to-do list) or acts on it, then deletes the note, so the file holds only
  what nobody has handled. Agent-owned state belongs in an agent-owned file, never in the inbox.
- Questions only Mortaro can answer go to `mortaros_missing_decisions.md`.

## Never lose a decision

- Every decision goes, when it is made, into the specification page that states that part of the language, into the
  docs page that teaches it where the teaching changes, and into `DECISIONS.md` as a new row. A decision that exists
  only in a conversation is a decision that will be argued again.
- The specification is normative: when anything else disagrees with it, the specification wins.

## Deciding or asking

- Mortaro decides language semantics and anything that cannot be undone cheaply.
- An agent decides what can be changed later without losing work, records it as "decided by Claude", and lists it
  under "To confirm" in `mortaros_missing_decisions.md`. A proposal not yet decided is marked "(proposed by Claude,
  unconfirmed)". Record who actually decided each thing; never present an agent's proposal as Mortaro's decision.

## Shared files

`DECISIONS.md`, `mortaros_notes.md` and `mortaros_missing_decisions.md` are written by every session. Whole-file
overwrites have already lost a day of decisions once (a mirroring restore rolled them back an hour).

- Make targeted edits that match on surrounding text, never on line numbers.
- Re-read a file immediately before writing it.
- `DECISIONS.md` is append-only: rows are never edited, renumbered or reordered. A decision that changes an earlier
  one is a new row naming the one it supersedes.
- Never restore these files from a backup, and exclude them from any mirroring or restore.
- Stage explicit paths; never `git add -A` or `git add .` while another session may be working.

## Writing

- Commit messages start with a gitmoji, are lowercase and imperative, and end with `Co-authored-by:` naming the model
  that wrote them.
- No em dashes, in prose or in code, and no two hyphens between spaces standing in for one.
- The default branch is `master`; do not rename it or suggest renaming it.
