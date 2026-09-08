# Development standards

## How code is written
- Conventional commits via hooks; branch and PR title carry the ticket id —
  checked by the gate, so nothing lands untracked.
- Pre-push: format + lint + type-check locally, repeated in CI. A red main is
  an outage. Style is the machine's job; human review is for behaviour,
  design and risk.
- Tests mirror the service structure; **decision logic extracted into
  dependency-free modules** so it is actually testable.
- Config layered (`.env` + local overrides) with maintained examples.
- Schema changes flow one way: forward-only migrations, expand→contract for
  breaking changes. Data that describes spend or history **outlives its
  parent rows** (`SetNull` + snapshot columns + a delete trigger).

## How a change is reviewed
- Every PR read in full by the AI gate before merge; blocking findings as
  *file:line · why it matters · the fix*; advisory notes separate.
- Green merges itself (auto-complete / auto-merge) below production —
  humans are saved for decisions only they can make.
- The gate re-runs on every push; a conflicted PR never queues — rebase,
  resolve, push, never wait.
- Override is a decision on the record: a named person, a written reason,
  scoped to the push it was written after.
- PRs are small and self-verifying: one concern; description states what
  changed, how it was verified, who asked.
- **Code ownership is declared** (CODEOWNERS per area): review routing is
  automatic, boundary-touching PRs reach the architect without anyone
  remembering to add them.

## Hardened standards (each shipped as a bug first)
failure is never empty · done means the running artifact answers · outputs
validated, not exit codes · background jobs resume from the store they write
· retries bounded and honest (count distinct units) · authorization is
resource-level on every mutating route · queries proven against the live
engine (reserved words fail silently) · served code declares its caching.
