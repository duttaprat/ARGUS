# Local data setup

ARGUS resources are managed locally and are not included in this repository. The implemented workflow has a DVR prediction stage and a bounded investigation stage, but they are not yet fully integrated end-to-end from one natural-language prompt. Source code is not yet included in this repository, so this guide describes resource organization rather than executable installation commands.

## Prediction input and evidence resources

| Resource | Current role | Record for reproducibility |
| --- | --- | --- |
| DVR weights and supporting inputs | Existing pipeline/notebook produces reference- and alternate-allele TF binding probabilities | Model identifier, weight checksum, input requirements, preprocessing |
| Supplied/precomputed DVR probabilities | Starting hypotheses for some investigation runs, including the illustrated evaluation | Variant/TF identity, ref/alt labels, probability values, source run or artifact, model version when known |
| ADASTRA | Direct experimental allele-specific TF binding evidence | Release/snapshot, assembly, experimental context and observation provenance |
| JASPAR | Computational allele-specific motif evidence | Release, collection, motif identifiers and scoring context |
| ENCODE cCRE | Regulatory-element context | Release, assembly, annotation context |
| AlphaGenome Atlas | Separate computational cross-reference outside the loop | Version/snapshot, query inputs and output interpretation |

JASPAR and cCRE are indirect evidence and cannot independently establish TF binding. Strong terminal decisions require admissible direct experimental evidence. Resource availability alone does not establish a scientific verdict.

Obtain resources through their providers or approved project storage and check access and redistribution terms. No specific download, release, format, or runtime path is prescribed here without checking the implementation.

## Suggested local layout

```text
data/
  dvr_predictions/
  adastra/
  jaspar/
  encode_ccre/
  alphagenome_atlas/
weights/
  dvr/
results/
  README.md
  <local-run-id>/
```

These are suggested locations, not runtime requirements. Data, weights, and generated results are ignored by Git. Keep machine-specific paths in local configuration. Use relative paths or neutral placeholders in shared documentation.

## Setup sequence

1. Establish whether the investigation receives supplied/precomputed DVR probabilities or probabilities from a separately executed DVR pipeline/notebook. Preserve that distinction in the run record.
2. Identify the exact resource versions and input formats required by the implementation. Place bulk resources and weights in ignored storage.
3. Confirm genome assembly, contig naming, coordinate convention, reference/alternate allele representation, TF identity, and relevant biological context. Record transformations explicitly.
4. If useful, copy `.env.example` to `.env` and adapt it locally. Its variable names are proposed; no loader or verified configuration contract is supplied here.
5. Configure Anthropic credentials locally for the optional LLM planner and/or the Anthropic prose reporter. Choosing the rule-based planner does not remove the reporter's Anthropic dependency if prose reporting is used.
6. Add verified installation and run commands after source and dependencies are available. No runnable end-to-end natural-language command is claimed here.

Keep AlphaGenome Atlas setup separate. Local Atlas resources do not enable loop integration.

## Provenance

A local resource manifest should record resource name, provider, release/snapshot, retrieval date, checksum, assembly where applicable, coordinate convention, preprocessing, and usage restrictions. For precomputed predictions, retain the producing run or artifact rather than attributing fresh inference to the investigation runner.

Share sanitized metadata when useful. Exclude credentials, signed URLs, private server paths, database contents, model weights, and bulk outputs.

## Before committing

Check `git status --short` and review the staged diff. Ignore rules cover common artifact formats and local directories, but cannot classify arbitrary files by size/content and do not affect already tracked files. Keep bulk artifacts in ignored storage even when their extension is not listed.
