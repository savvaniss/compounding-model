# The AI merge gate — specification

A blocking check on every PR — a build-validation policy, required status
check or merge check, depending on the platform (see baseline/devops.md's
mapping). Deterministic checks first, then an LLM review of the full diff.

## Checks
| check | overridable? |
|---|---|
| Traceable to a ticket (branch + title) | no — ten-second fix |
| Branch naming (`feat/ fix/ hotfix/ chore/ docs/ refactor/ test/ revert/` + environment branches promote as themselves) | no |
| No secrets in the diff (credential-shape scan, full file contents) | **never** — a judgement can be wrong, a committed secret cannot |
| AI review (blocking findings) | yes — by a named person, on the record |

## Report contract (legibility is what makes it obeyed)
Every finding: **file:line · why it matters · the fix**. Blocking findings
separated from advisory notes. Posted as a PR thread AND a commit status; the
status is what merge automation (auto-complete / auto-merge) reads. Every run telemetered (outcome, tokens,
files reviewed) — precedence blocked > overridden > degraded > passed.

## Override — a decision on the record
`/gate override: <reason ≥ one sentence>` in the PR thread by a listed team
member flips only the AI review. Hard-learned mechanics: the gate must skip
its **own** comments (mark them; an unanchored regex once matched the gate's
instruction text and would have self-approved every PR), anchor the command
to line start, reject the literal placeholder, and scope the override **to
the push it was written after** — new commits need a fresh one, or one
override silently blesses unreviewed code forever.

## Degradation
If the model is unavailable the gate says so and does **not** block — but
reports "degraded", never "passed". If it cannot resolve the repo/PR it
stops loudly; a swallowed 404 once blocked a PR with nothing said on it.
