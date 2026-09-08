# The Compounding Model — AI assessment prompt

Paste everything below this line into an AI agent that has access to your
organization's repositories, pipelines, and (ideally) telemetry — or tell
your agent: *"Fetch
https://raw.githubusercontent.com/savvaniss/compounding-model/main/assessment/ai-assessment-prompt.md
and run it against our organization."*

---

You are assessing this organization's AI delivery maturity against **The
Compounding Model** (https://github.com/savvaniss/compounding-model): six
questions, three stages — Stage 1 (Ad hoc: AI is a personal tool),
Stage 2 (Systematic: AI is a team practice), Stage 3 (Compounding: AI is
part of the operating model).

## Rules of the assessment

1. **Grade only on evidence you can cite**: a file, a branch-policy or
   pipeline setting, a run, a log line, a database/dashboard query result.
   Name each artifact in your verdict.
2. **A claim without an artifact is not evidence.** If people say it but
   you cannot find it, grade the question as if it were absent.
3. **"Can't tell" scores as the lower stage.** You are a skeptical
   validator, not a cheerleader.
4. **The overall stage is the MINIMUM across the six questions** — a
   chain, not an average.
5. Read-only: gather evidence without changing anything.

## The six questions, and what to hunt for

**Q1 — Where does AI act?**
Evidence of AI beyond code completion: AI-reviewed pull requests, AI-run
test generation, AI-drafted docs, agent-driven operational actions.
Stage 3 needs end-to-end reach including operations — e.g. agent-queued
production changes carrying a human approval.

**Q2 — Who does the AI act as?**
Inspect how agents authenticate: shared API keys or bot accounts → Stage 1.
Named accounts per tool → Stage 2. Stage 3 needs per-person identity on
every agent action (short-lived personal tokens, attributed commits/actions,
a tool boundary that *refuses* anonymous writes — find the refusal path).

**Q3 — What does it cost, and is it worth it?**
Look for a cost ledger: per-call attribution (who, which model, tokens), a
rate/price table, dashboards. Stage 3 needs unit economics AND proof that a
recurring review changed something (a model swap, a quota move, a sampling
change — find the diff or the minutes).

**Q4 — How good is its output?**
Look for evaluation machinery: LLM-judge verdicts on sampled real traffic,
an eval harness with golden tasks, a model-swap PR with eval results
attached. Honest failure handling ("not gradeable" instead of invented
scores) is a Stage 3 marker. Human spot-review alone → Stage 2.

**Q5 — Does the capability compound?**
Look for AI assets as code: a shared repo of prompts/rules/agent configs
with owners and history → Stage 2. Stage 3 needs a factory: assets shipped
through their own review gates, an asset registry maintained in the creating
commits, a lessons-learned library with evidence — and signs the lessons are
actually served back during work.

**Q6 — Who carries it?**
Champions named per team with time allocated (find the roster and recent
activity) → Stage 2. Stage 3 needs fluency across roles and **daily usage
proven from telemetry** (tool usage tables, per-person action rows) — not
surveys.

## Output format

1. A markdown table: `question | stage verdict | evidence cited | the gap`.
2. **Overall stage = the minimum**, stated plainly, with the weakest links
   named.
3. The top three next builds, mapped to the blueprint: utilities in
   `architecture.md`, decisions in `decisions/`, the order in `roadmap.md`
   at https://github.com/savvaniss/compounding-model.
4. A one-paragraph honest summary a sponsor can read — no grade inflation.
