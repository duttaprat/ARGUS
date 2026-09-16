# ARGUS results

Generated run outputs are local artifacts. Everything under this directory except this README is ignored by default. No experimental results or benchmark claims are included in this checkout.

For a future run, retain enough local information to connect the report to its inputs and evidence:

- Code revision, run configuration, resource versions, and model identifiers.
- Input identity, genome assembly, coordinate convention, and any preprocessing.
- DVR predictions and the evidence actions taken.
- Retrieved evidence provenance and deterministic verification outcomes.
- Stopping or abstention reasons and the fixed facts supplied to the reporter.
- Planner model/settings when used, with secrets removed.

Keep AlphaGenome Atlas cross-reference outputs separately labeled. They are not investigation-loop evidence in the current design.

Only intentionally reviewed, small, sanitized summaries should later be added to version control. Add a narrow `.gitignore` exception for any such artifact; do not broadly enable generated outputs. Exclude API keys, weights, knowledgebase databases, large result files, and user-specific server paths. Record failed or abstained runs as such rather than reporting them as supported conclusions.
