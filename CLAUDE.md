# Operating manual for the AI agent on this project

You are an AI coding agent starting on a new project that follows this
repository's baseline. This file is your contract. Every rule
below was learned in production operation — each exists because its
absence cost real time.

## Identity & attribution
- You act **as a person**: every write you make carries the identity of the
  human who directed it. Mint/receive a personal token at session start.
  Refuse to write anonymously.
- **Connect to the team's tool server (MCP) at session start** and route
  every team question (runbooks, telemetry, environment status, lessons)
  and every team action (deliver, deploy, PR, ticket comment) through its
  tools — never around it with raw credentials. A missing tool is a
  factory gap to raise, not a reason to improvise past the boundary.
- Whatever you trigger (pipeline, PR), hand over **the link and run id**.
  Work the human cannot open is progress they cannot see.

## Secrets in your hands
- **Never print a secret value** — not in chat, logs, tickets, commit
  messages or tool output. Fetch secrets into permission-restricted files;
  reference them by name.
- A secret that appears anywhere readable is **rotated the same day**, not
  risk-assessed. Say so immediately; exposure you caused and hid is worse
  than the exposure.
- Secrets have exactly one home: **the vault**. Pipeline variables, secure
  files and runtime env vars are transports materialized from it at run
  time — never a second source of truth. If a task seems to need a secret
  pasted somewhere, the task is wrong, not the rule.

## Delivery loop — never deviate
1. Work on a branch named `type/TICKET-slug` from the environment branch.
2. Open a PR; the AI gate reviews it (blocking). Fix findings or have a human
   override **on the record**; never bypass.
3. Ship through the **one orchestrated delivery path** — never hand-run
   individual build/deploy pipelines around it. A hand-run build once
   skipped a component and left production pulling a tag that never
   existed; the orchestrator is where the guards live.
4. After a merge, **deploy**. After a deploy, **verify on the running
   system** — a green pipeline is not proof; the rollout converging and the
   endpoint answering is. Then report with links.
5. Stop only where a decision is genuinely a human's: approval gates,
   destructive actions, real ambiguity.

## The rules that came from incidents
- **Fetch before branching.** A stale environment ref once produced a branch
  that re-added a feature just removed.
- **Merged = ancestor of main** (`git merge-base --is-ancestor`), never "PR
  shows completed". A commit pushed after its PR completed is silently
  stranded.
- **Promotions are real merges, never squashes** — a squashed promotion
  drops ancestry and every later promotion conflicts from a stale base.
- **A deploy's tag must exist in the registry before you queue it.** A deploy
  of an unbuilt tag leaves old pods serving while the pipeline reports green.
- **Never patch resources a deployment tool owns** (e.g. `kubectl patch` on
  Helm-managed objects) — field ownership breaks the next upgrade.
- **Failure ≠ empty.** A `catch` that returns nothing turns a broken query
  into "no data". Carry could-not-read and nothing-there as different values.
- **Verify the path that deploys**, not the file you happen to have open.
- **Check the shape of every return value** before comparing it. Twice in one
  day a guard shipped that compared strings against objects.
- **Re-check, don't poll.** Read what the tooling already told you; a watcher
  prints on every iteration and names every exit.
- **Two failed fixes = stop shipping guesses.** Reproduce in the lowest
  environment that shows the bug and debug **in place** there; builds cost
  real minutes and every blind redeploy burns trust with them.
- **When you change a data shape, find every reader first.** Grep all parse
  sites on the path and test with a real payload against a real store — a
  fix that satisfies the one consumer you looked at fails in the one you
  didn't.
- **"Works in the lower environment" says nothing about config.** Same code,
  different values files: before promoting, diff the environment configs for
  the resources your change touches (endpoints, models, quotas, keys). Most
  "prod-only" bugs are values-file bugs.
- **Never delete or hand-edit an applied migration.** Histories are
  reconciled forward (a new migration, idempotent `IF EXISTS` drops), never
  rewritten; an inconsistent history blocks every future deploy.
- **A duplicated truth is a pending incident.** Two copies of an enum,
  mapping or schema drift apart silently; a change isn't done until every
  copy is updated — and the real fix is deleting the copy.
- **Verify from the requester's vantage point**: a fresh session, an
  unauthenticated request, an uncached load. "It works" from your warm,
  authenticated context proves little — and put the answer where the human
  will actually read it, not buried in tool output.
- Every asset you create goes in `REGISTRY.md` **in the same commit**. Every
  incident becomes a lessons-learned entry carrying its evidence.

## Where things are
- Standards: `baseline/` · Enforcement mechanics: `enforcement/`
- Evidence you must keep green: `evidence/` · Templates: `templates/`
