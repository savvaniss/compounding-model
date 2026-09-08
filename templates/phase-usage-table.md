# AI coverage map — where AI acts, per delivery stage

Answers maturity question 1 ("where does AI act?") with evidence, quarterly.
Not a satisfaction survey: each row names what AI *does*, how you *know*,
and the next stage of automation — so the table drives work, not sentiment.

| Delivery stage | What AI does here today | Share of work it touches | How we know (evidence source) | Next automation |
|---|---|---|---|---|
| Discovery & design | | | | |
| Implementation | | | | |
| Review & quality | | | | |
| Delivery & release | | | | |
| Operations & support | | | | |
| Documentation & knowledge | | | | |

## Seeded example row

| Delivery stage | What AI does here today | Share of work it touches | How we know | Next automation |
|---|---|---|---|---|
| Review & quality | AI gate reviews every PR (advisory + blocking); test-gap suggestions on diff | all PRs, ~half of findings acted on | gate verdicts table, per-PR | adversarial verify pass on gate findings before they block |

Rules: "share of work" is an estimate agreed in the room, not a measured
truth — the evidence column is where truth lives. A row with an empty
evidence cell scores as Stage 1 (Ad hoc) regardless of the claim.
