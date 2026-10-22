# Mortaro's Spite

The canon of every Spite decision, for every Spite repository (the language, the compiler, the standard library,
the spec programs, the benchmarks):

- [`DECISIONS.md`](DECISIONS.md): every decision, with its date, who made it and why. Append-only: a row is never
  edited, renumbered or removed; a decision that changes an earlier one is a new row naming the one it supersedes.
  Every repository adds its rows here, with the next free number, and nowhere else.
- [`mortaros_notes.md`](mortaros_notes.md): Mortaro's inbox. Each note is moved into a decision (a row here, and
  the rule into the spec programs) or acted on, then deleted, so the inbox holds only what nobody has handled.
- [`mortaros_missing_decisions.md`](mortaros_missing_decisions.md): the questions only Mortaro can answer, and the
  agents' own decisions waiting for him to confirm.

Commit messages start with a gitmoji, are lowercase and imperative, and end with the name of the model that wrote
them (`Co-authored-by: ...`). No em dashes.
