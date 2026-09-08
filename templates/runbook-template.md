# <task name>

One topic per file. Keep it inside the assistant's read window (~6k chars);
split with an index if it grows. Category by filename prefix.

## When to use
## Preconditions
## Steps (exact commands, no placeholders left unexplained)
## Verify (how you know it worked)
## Rollback

---

## Seeded example

# Wake a slept non-prod environment

### When to use
A developer needs an environment that the cost schedule stopped overnight.

### Preconditions
Cloud CLI logged in with the environment's reader+operator group; you know
which environment (never guess — check the console's status view).

### Steps
1. Start the compute (cluster or app environment) via the wake pipeline —
   not by hand, so the action is attributed and logged.
2. Watch the pipeline's verify stage: it waits for node readiness and
   workload convergence, and fails loudly on timeout.

### Verify
The console shows the environment green; the app's health endpoint answers
200 **via an unauthenticated request from outside the cluster**.

### Rollback
Re-run the sleep pipeline; it is the same code path in reverse.
