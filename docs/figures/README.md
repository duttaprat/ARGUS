# ARGUS architecture figure

[ARGUS_architecture.png](ARGUS_architecture.png) is the primary figure used by the repository README and architecture guide. It depicts the evaluated loop starting from precomputed DVR predictions, with AlphaGenome Atlas outside the loop as a separate computational comparison.

The maintainer-supplied figure was edited with the built-in image-generation tool. The edit preserves the scientific values and layout while making the planner-policy and KLF6 wording match the maintainer's corrections. It is documentation of the supplied demonstration, not a new experiment or a benchmark.

## Edit prompt

```text
Use case: text-localization.
Edit target: the supplied ARGUS architecture figure. Produce a faithful edited PNG for repository documentation; preserve the entire layout, colors, borders, arrows, scientific numbers, labels and all other wording. No redesign.
In the top central box retain the heading "1 State + planner policy". Replace the existing two lines "Rule-based or optional LLM proposal" and "Deterministic guardrails validate actions" with exactly this sentence, wrapped legibly within that box: "Rule-based policy or optional LLM proposal, validated by deterministic guardrails." Preserve the two preceding lines "Read hypothesis and evidence" and "Select next admissible test or abstain". Make only a modest font-size or line-wrap adjustment as necessary to fit without overlap.
In the lower KLF6 portion of panel D, use exactly "Motif evidence opposes DVR; regulatory context is non-resolving." Keep the following "3 evidence calls; 8 logged events".
The source already uses "State + planner policy" and mostly the revised KLF6 wording: preserve those corrections, ensure the requested sentence is exact including punctuation.
Keep precomputed DVR input labeling, separate Atlas comparison box with no connection into the loop, all evidence values, and all other text unchanged. Do not add discovery, clinical-use, or predictive-accuracy claims. Maintain sharp readable text and the original aspect ratio.
```
