# DevOps practices: branching, pipelines, guards

The practices below are engine-neutral invariants; the
[mapping table](#the-mapping) translates them to Azure DevOps, GitHub,
GitLab, AWS and GCP. The guards are implementable everywhere because they
live in the **tool layer that queues the pipelines**, not inside any
engine.

## Environments & branching
- Environment branches (`dev`, `stg`/`sbx`) deploy to their cluster on
  merge; **main is reachable only from an environment branch**; production
  deploys a tag already proven downstream, behind a human approval check on
  the deployment environment.
- Branch policies mirrored on every long-lived branch, enforced by the
  platform. Feature branches squash-merge; **environment-branch promotions
  merge as real merges (no squash)** or ancestry breaks and every later
  promotion conflicts.

## The pipeline catalogue (portable as a set)
build-base · build-dependent (layered images, one tag names the release) ·
deploy-per-env (chart/template + migrations + health verification) · change-delivery
orchestrator (build → deploy → migrate → verify → report on the ticket) ·
blocking AI PR gate · advisory test generation · docs guard (size + secret
scan) · registry retention · agent health · release notes.

## Guard rules — each one paid for in production
1. **A CI trigger must never deploy.** Triggered runs carry no parameters and
   fall back to defaults; guard environment-writing steps on the run's
   trigger/reason field.
2. **Defaults must be pullable**: verify a default tag exists in the registry
   for every image it drives.
3. **A deploy refuses a tag that is not in the registry** — otherwise the new
   pod sits in ImagePullBackOff, the old pods keep serving the old config,
   and the run still reports success.
4. **Rollout convergence is the success signal**, not pipeline exit code:
   after deploy, wait for rollout status and probe the endpoint.
5. Database migrations run **inside the deploy, in every environment** —
   hand-applied production schemas drift silently and cost you the columns
   you need most.
6. Production can be **queued by anyone (including agents), landed only by a
   named person** — verify the approval check exists at call time; if someone
   deletes it, refuse again automatically.
7. Rollback = redeploying the previous tag, which is why release tags are
   immutable and never overwritten.

The Helm/cluster mechanics above assume the ADR-005 managed-Kubernetes
trigger has fired; on the serverless-containers default the same catalogue
and guards apply, with revisions in place of charts.

## The mapping

| concept | Azure DevOps | GitHub | GitLab | AWS | GCP |
|---|---|---|---|---|---|
| branch protection | branch policies | protected branches + rulesets | protected branches + approvals | host of choice via CodeConnections | host of choice via Developer Connect |
| pipeline engine | Azure Pipelines | GitHub Actions | GitLab CI | CodePipeline + CodeBuild | Cloud Build + Cloud Deploy |
| blocking PR gate | build-validation policy + PR status | required status checks | merge checks (pipeline must succeed) | PR build + required check on the host | build trigger + required check on the host |
| human approval into prod | environment approval check | environment required reviewers | protected environment / manual job | manual approval action | Cloud Deploy approval |
| merge automation | auto-complete | auto-merge | merge-when-pipeline-succeeds | via host | via host |
| pipeline → cloud identity | workload identity federation (OIDC) | OIDC to cloud roles | OIDC to cloud roles | OIDC to IAM roles | Workload Identity Federation |
| pipeline secret transport (fed from the vault) | secure files + variable groups | encrypted secrets + environments | protected/masked CI variables | Secrets Manager in CodeBuild | Secret Manager in Cloud Build |
| artifact/container registry | Azure Artifacts + ACR | Packages + GHCR | GitLab registry | CodeArtifact + ECR | Artifact Registry |

The invariants never change with the engine: PR-only, blocking gate, build
once, guards at the queuing layer, a named human landing production.
