# ARGUS examples

The [architecture figure](../docs/figures/ARGUS_architecture.png) illustrates the evaluated investigation loop using precomputed DVR predictions. It does not demonstrate a completed workflow from one natural-language prompt through fresh DVR inference and investigation.

## Illustrated cases

| Case | Evidence interpretation | Investigation outcome |
| --- | --- | --- |
| FOXA1 | Admissible direct experimental evidence opposes the non-differential DVR prediction | `rescued`; the evidence verdict contradicts DVR rather than confirming its original prediction |
| KLF6 | Motif evidence opposes DVR; regulatory context is non-resolving | `abstained`; indirect evidence does not establish experimental TF binding |

These are demonstration cases, not a predictive-accuracy benchmark. AlphaGenome Atlas remains a separate computational comparison and is not an evidence action in these loop examples.

## Runnable examples

Executable examples will be added with the code release.

Future examples should be small, synthetic or approved for redistribution, and should document:

- Purpose and component demonstrated.
- Code revision, model/resource versions, genome assembly, and coordinate convention.
- Whether DVR probabilities are supplied/precomputed or produced by a separately executed prediction stage.
- Verified entry point, local configuration, and planner policy.
- Expected evidence observations, deterministic states, and stopping or abstention behavior.
- Whether Anthropic is used for planning, prose reporting, or both.

Keep evidence observations, scientific verdicts, and generated prose distinguishable. JASPAR/cCRE alone cannot establish TF binding; `supported`, `contradicted`, and `rescued` require admissible direct experimental evidence.

Use relative paths and placeholders. Keep credentials, weights, databases, bulk inputs, large results, and user-specific server paths out of examples. `examples/local/` is ignored for local trials. Label synthetic fixtures as synthetic and do not report their outputs as measured ARGUS results.
