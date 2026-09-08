# Pipeline guards — refusals, not conventions

Implemented in the tool layer (MCP/CLI wrapper) that queues pipelines, so
every caller — human or agent — hits them.

1. **Tag-exists guard**: a deploy is refused if its image tag is not in the
   registry; the refusal names the newest tags that do exist. (Read the tag
   objects' *names* — and test the guard's own shape assumptions; an
   untested guard once blocked all production deploys with a type error.)
2. **Build-reason guard**: cluster-writing steps skip on CI-triggered runs.
3. **Approval-check verification**: production queuing verifies the
   environment's human-approval check exists *at call time*.
4. **Promotion guard**: main only from an environment branch; the promoted
   tip must be carried by an image an environment actually ran; promotions
   complete as real merges.
5. **Orchestrator preference**: humans and agents use the change-delivery
   orchestrator (build→deploy→migrate→verify→report); individual pipelines
   remain for rollback and rebuilds, and inherit all guards.
