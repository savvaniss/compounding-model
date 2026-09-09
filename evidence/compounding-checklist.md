# Evidence — Stage 3 (Compounding)

**Compounding means AI is part of the operating model, and every use makes
the next one better**: agents act with per-person identity, cost is
governed, output is judged, the factory ships itself through its own gates
and lessons feed back into the work. It is the third and final stage; see
[README.md](README.md) for the ladder.

Everything in [Systematic](systematic-checklist.md), plus the hard
artifacts below. Expect to walk a validator through the system live — the
evidence is the running system, not a deck about it.

| question | evidence artifact |
|---|---|
| Where does AI act? | the AI gate blocking a real PR (finding visible); a delivery-orchestration run reporting on its ticket; an agent-queued production change with the human approval attached |
| Who does it act as? | a tool-boundary refusal of an unattributed write; per-person action rows in the activity store |
| What does it cost? | cost ledger per model/person/feature priced from the effective-dated rate table; minutes or diff of a review that changed something (model mix, sampling rate, quota) |
| How good is its output? | judge verdicts on sampled traffic incl. an honest "not gradeable"; an eval-harness ranking (golden tasks × candidates, quality + cost) attached to a model-swap PR |
| Does capability compound? | the factory repo tree with history; **one PR where the gate blocked a change to the factory itself**; a lesson entry and the incident that spawned it |
| Who carries it? | per-person daily usage from telemetry; champions covering every core role |

Validity is time-boxed and re-proven per cycle — the practical argument for
evidence as a by-product (ledgers, telemetry, registries), never an export
project.
