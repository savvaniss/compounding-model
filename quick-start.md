# Quick start — hand this to your agent

**You (the human):** tell your AI agent —
*"Fetch https://raw.githubusercontent.com/savvaniss/compounding-model/main/quick-start.md
and execute it."* It will do the work and stop only where a decision is
genuinely yours. The 🧑 marks are those stops.

---

**Agent:** you are standing up the rails of the Compounding Model for this
project. Execute the ten actions in order. Verify each before moving on;
report every artifact you create with its link. Stop at 🧑 points and ask —
those decisions belong to the humans.

1. **Read the blueprint first**: fetch `README.md`, `maturity-model.md`
   and `CLAUDE.md` (your contract) from
   https://github.com/savvaniss/compounding-model — internalize the model
   and the categories it guides and enforces, then give the team a one-page
   summary. Execute nothing before this.
2. **Set the repo-local git identity** before any commit, and verify with
   `git log --format='%an %ae'` before the first push — the global config
   may carry an identity that must not appear in this history.
3. **Present the 15 [decisions](decisions/)** to the team as a one-page
   summary (one line per ADR: the default and what adopting it means).
   🧑 The team accepts or disagrees per ADR; draft a superseding ADR for
   every disagreement *today* — silent divergence is how blueprints die.
4. **Protect the branches**: PR-only on every long-lived branch, for
   humans and agents alike, via the platform's policy API. Verify by
   attempting (and being refused) a direct push.
5. **Give secrets one home**: enable push-protection secret scanning;
   inventory every `.env`, pipeline variable and wiki page holding a
   credential; move them to the vault (the only home) and delete the
   copies. 🧑 Rotation approval for anything that was exposed.
6. **Create the team's factory repo** (🧑 confirm org, name, visibility —
   `--template savvaniss/compounding-model` seeds it) with `REGISTRY.md`
   at its root; from this moment register every asset you create in the
   creating commit — including the assets from steps 7–10.
7. **Post the [working agreement](enforcement/working-agreement.md)** for
   signature. 🧑 Humans sign; you operate under it from now on.
8. **Stand up the gate skeleton** in advisory mode: ticket-in-branch check
   plus secret scan on every PR, reporting as a commit status. Verify with
   a deliberately failing test PR, then delete it. 🧑 Agree the date it
   turns blocking (this week).
9. **Draft the [NFR catalogue](solution/nfr-catalogue.md) targets** from
   what you can observe (current latencies, traffic, data classes) —
   never leave a row "TBD". 🧑 The team confirms or amends the numbers.
10. **Run the [AI assessment](assessment/ai-assessment-prompt.md)** against
    the organization's real repos and pipelines; present the verdict table,
    and file every weak answer as a ticket mapped to the
    [roadmap](roadmap.md).

Day one ends here. **Your next assignment is building the factory
itself**: provision its spoke from
[architecture/factory-infrastructure.md](architecture/factory-infrastructure.md)
as IaC through the pipeline, then build the thirteen utilities of
[architecture.md](architecture.md) in numbered order — the enforcement
spine (gate, tool server, orchestrator, guard library) first, each
landing as a gated PR with its registry row. The
[four-week roadmap](roadmap.md) sequences it; execute it the same way you
executed today — through the gate you just built.
