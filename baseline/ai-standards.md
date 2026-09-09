# AI infrastructure standards — the Stage 3 (Compounding) layer

AI is infrastructure with its own contracts, not a feature bolted on.

## Identity
Per-person signed short-lived tokens for every agent; unattributed writes
refused; every tool call telemetered per person into a queryable table. The
"who did what through the AI" view is the adoption evidence validators ask
for — build it as a by-product, not a report.

## Cost (governance dimension)
- Every LLM call persists: user, model **requested vs actually served**,
  input/cached/output/reasoning tokens, latency, retries, error — and the
  **ticket/feature id** where the call served one, which is what makes
  "what did this feature cost" a query (the memory join of
  knowledge-work-integration.md).
- Prices live in an **effective-dated rate table** (per-1M-token, the unit
  providers quote). Cost is computed at read time, never stored — correcting
  a rate re-prices history. Providers may bill under **dated deployment names** (Azure does);
  price rows must use the name as billed or spend silently reads zero.
- Spend rows survive document/entity deletion (snapshot columns + trigger).
- A recurring cost review that actually changes things: model mix, judge
  sampling rate, quota allocations.

## Quality — the evaluation loop
- LLM-as-judge scores generations (groundedness, coverage — pick the axes
  that match *your* workload) on a sample of real traffic.
- A **model evaluation harness** runs on demand: a golden set of tasks
  representative of your workload, executed by every candidate model, scored
  by the judge, with **cost shown next to quality per candidate** — quality
  per euro is the real decision. Re-run it before every model swap or
  provider migration; the harness is what turns "the new model feels fine"
  into evidence.
- The judge **never scores its own generator**, stamps its prompt version on
  every verdict, and when retrieval returns nothing answers **NOT
  GRADEABLE — a retrieval problem, not a model result** instead of inventing
  zeros. An honest judge is the difference between a metric and a decoration.

## Capacity
The AI that *builds* the product runs on quota isolated from the AI *in* the
product. Model TPM quotas are regional and finite: check the quota table
before creating deployments; a second account in the same region gets zero
capacity if the first consumed the allocation.

## Provider traps — a field guide
Some vendors' models behind a cloud AI gateway answer only their native
API route — the gateway's OpenAI-style route 404s (seen with
Anthropic-style models; test both routes before wiring). Gateways under throttle can return
transient 404s for deployments that exist — classify them retryable. Duplicate
model-name enums across packages will meet in one LangChain sequence and
refuse the whole chain — one enum, imported everywhere.
