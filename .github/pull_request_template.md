## Problem and resulting behavior

Describe what changes and why. Include a concrete before/after example when useful.

## Scope

Identify the affected stages and documentation. State whether this changes allowed evidence actions, planner policies, deterministic verification, stopping/abstention, or Anthropic prose reporting from fixed facts. Label AlphaGenome Atlas work as standalone comparison or proposed loop integration.

## Validation

List checks performed and outcomes. For documentation-only changes, record the review and do not imply code tests were run. Distinguish supplied/precomputed DVR probabilities from fresh prediction and investigation-only runs from end-to-end execution. Identify planner and reporter use of Anthropic separately.

## Evidence and evaluation review

- [ ] Strong terminal decisions (`supported`, `contradicted`, `rescued`) remain tied to admissible direct experimental evidence.
- [ ] JASPAR motif evidence and cCRE context are not presented as independently establishing TF binding.
- [ ] Unavailable, underpowered, contextual-only, or mixed evidence can remain unresolved and lead to abstention.
- [ ] Neither the optional planner nor the reporter overrides deterministic scientific classifications.
- [ ] The small demonstration is not presented as a predictive-accuracy benchmark, autonomous discovery, or clinical use.
- [ ] Implemented behavior, proposed changes, and unverified details are clearly distinguished.

## Repository hygiene

- [ ] No API keys, model weights, knowledgebase databases, large results, or user-specific server paths are included.
- [ ] New configuration is documented with secret-free placeholders.
