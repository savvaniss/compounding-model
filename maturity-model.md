# The Compounding Model — six questions, three stages

Rather than grading capability rows against a vendor's rubric, this model
asks the **six questions a skeptical sponsor asks** about AI in a delivery
team, and defines three honest answers to each. A team's stage is the
*lowest* answer it can prove — maturity is a chain, not an average.

**Stages:** **Stage 1 — Ad hoc** (AI is a personal tool) → **Stage 2 —
Systematic** (AI is a team practice) → **Stage 3 — Compounding** (AI is
part of the operating model, and every use makes the next one better).

| # | Question | Stage 1 — Ad hoc | Stage 2 — Systematic | Stage 3 — Compounding |
|---|---|---|---|---|
| 1 | **Where does AI act?** | isolated coding tasks | most delivery stages (design, code, test, docs) | end-to-end workflows incl. operations — agents queue production changes, humans land them |
| 2 | **Who does it act as?** | shared keys, anonymous output | named accounts per tool | per-person agent identity; unattributed writes refused at the tool boundary; every action auditable to a human |
| 3 | **What does it cost — is it worth it?** | unknown | spend visible per tool | every call attributed and priced from an effective-dated rate table; unit economics reviewed on cadence, and the review changes decisions |
| 4 | **How good is its output?** | vibes | human spot-review of AI work | judged: sampled LLM-as-judge verdicts, an eval harness gating model swaps, honest "can't grade" over invented scores |
| 5 | **Does capability compound?** | nothing persists between people | prompts, rules, runbooks shared in source control with owners | a factory: assets are code, shipped through their own gates; incidents become lessons the assistant serves back during work |
| 6 | **Who carries it?** | a few enthusiasts | named champions per team, with time | fluency in every core role; daily usage proven from telemetry, not surveys |

## Proving a stage

Evidence-per-question, artifact or it doesn't count: for each question, name
the artifact that proves your answer (a ledger row, a blocked PR, a repo
tree with history, a telemetry query) and the **mechanism that produces it
continuously**. The [evidence checklists](evidence/) enumerate them per
stage. Build the artifact-producing mechanism, not the artifact — evidence
should be a by-product of the system, never a quarterly scramble.

## Reading the chain

The order of the questions is deliberate: identity (2) enables cost
attribution (3) and usage measurement (6); evaluation (4) makes model
decisions defensible; the factory (5) is what turns the first four from
projects into properties. Teams that jump to question 5 without questions
2–4 build an impressive repo nobody can govern.
