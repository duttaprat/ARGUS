# ARGUS architecture

ARGUS has two connected research stages: DVR prediction and bounded evidence investigation. Source code is not yet included in this repository; exact schemas, commands, thresholds, and runtime configuration will be documented with the code release.

## Primary architecture figure

![ARGUS evaluated loop: precomputed DVR hypotheses, planner policy, evidence acquisition, deterministic verification and stopping, fixed-fact narration, and a separate Atlas comparison](figures/ARGUS_architecture.png)

*This figure depicts the evaluated loop using precomputed DVR predictions. It does not depict a completed natural-language-to-DVR-to-investigation integration. AlphaGenome Atlas remains a separate computational comparison outside the loop.*

## Stage 1: DVR prediction

The existing DVR pipeline/notebook takes a variant query and produces TF binding probabilities for the reference and alternate alleles. ARGUS uses a DVR prediction as an investigation hypothesis.

The notebook/DVR prediction stage and the investigation runner are not yet fully integrated end-to-end from one natural-language prompt. Some investigation runs use supplied/precomputed probabilities. Retain the origin of those probabilities when describing a run; do not imply that an investigation-only run executed the prediction pipeline.

## Stage 2: bounded evidence investigation

The loop maintains a hypothesis and collected observations. A planner policy selects an allowed evidence action or abstains. After an evidence action, the deterministic verifier interprets the observation and assigns evidence states. Subsequent action selection depends on the updated state, within the investigation's bounds and stopping rules.

### Planner policies

Both implemented planners operate under the same scientific constraints:

- **Deterministic rule-based planner:** selects evidence actions using its rule-based policy.
- **Optional Anthropic LLM planner:** can propose only allowed evidence actions or abstain. Deterministic guardrails validate proposals. The planner cannot change the verifier or scientific verdict.

The figure labels this node **State + planner policy**: **Rule-based policy or optional LLM proposal, validated by deterministic guardrails.**

Action selection and scientific classification have separate responsibilities. An LLM proposal is not evidence. Exact action identifiers, budgets, validation schemas, and error-handling behavior are not specified here without source verification.

### Evidence sources and admissibility

| Source | Observation type | What it can establish in the loop |
| --- | --- | --- |
| ADASTRA | Direct experimental allele-specific TF binding evidence | May resolve a TF-binding hypothesis when the evidence is admissible under the deterministic verifier. |
| JASPAR | Computational allele-specific motif evidence | Provides indirect motif evidence relative to the DVR hypothesis; cannot independently establish TF binding. |
| ENCODE cCRE | Regulatory-element context | Describes regulatory context; cannot independently establish TF binding or resolve its allelic direction. |

The presence of a resource result is not sufficient for a strong terminal decision. Evidence type and admissibility matter. Missing evidence is not proof of absent binding, and contextual overlap is not experimental confirmation of a TF-specific claim.

### Deterministic verification and stopping

The verifier interprets observations and assigns evidence states. Strong terminal decisions such as `supported`, `contradicted`, and `rescued` require admissible direct experimental evidence. Neither planner nor the reporter may override this requirement.

Evidence that is unavailable, underpowered, contextual only, or mixed may leave the hypothesis unresolved. Stopping rules explicitly allow abstention in these circumstances. Indirect evidence must not be promoted into a strong direct-evidence verdict merely because the investigation stops.

Keep the observation's relationship to the DVR prediction separate from the final investigation status. In the illustrated FOXA1 case, direct experimental evidence contradicts the non-differential DVR prediction while the investigation status is `rescued`. Rescue does not mean confirmation of the original prediction.

### Reading the KLF6 demonstration

The figure shows unavailable ADASTRA evidence for the queried KLF6/variant entry, motif evidence opposing DVR, and cCRE regulatory context. **Motif evidence opposes DVR; regulatory context is non-resolving.** The result is abstention: motif direction and locus context cannot independently establish experimental KLF6 allele-specific binding. The cCRE observation does not counterbalance the motif result as evidence of TF-binding direction.

The figure's evidence-call counts and logged-event counts describe different quantities. They are details of the illustrated run, not benchmark metrics or general execution guarantees.

## Reporter

The reporter uses Anthropic to generate prose from fixed structured facts. It cannot override deterministic classifications, alter the scientific verdict, or claim experimental confirmation without direct evidence.

Anthropic therefore has two distinct roles when the optional planner is enabled: proposing allowed actions during investigation, and narrating fixed facts afterward. The reporter's factual constraints do not establish a measured rate of prose accuracy; the current evaluation is a small demonstration.

## AlphaGenome Atlas boundary

AlphaGenome Atlas was tested as a standalone computational cross-reference. It is not integrated into the loop, does not supply loop evidence, and does not determine loop verdicts. Its outputs remain computational comparisons, not direct experimental confirmation.

## Evaluation and integration limits

The current evaluation demonstrates bounded investigation behavior on a small set of examples, not predictive accuracy. The evaluated loop shown here starts from precomputed DVR predictions. A complete workflow from one natural-language prompt through fresh DVR prediction and investigation has not yet been integrated end-to-end.

## Reproducibility information to retain

For future run documentation, record the code revision, input identity, genome assembly and coordinate convention, origin of DVR probabilities, model and resource versions, planner policy, evidence actions, observation provenance, verifier outcomes, stopping/abstention reason, and facts supplied to the reporter. Record Anthropic model/settings separately for planning and reporting where used, without credentials.

These are documentation expectations, not a claim that a particular manifest schema is implemented. Deterministic verification does not by itself guarantee identical optional planner or prose outputs across runs.
