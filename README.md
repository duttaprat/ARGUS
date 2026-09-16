# ARGUS

**Evidence-constrained agentic reasoning for noncoding regulatory variant interpretation.**

ARGUS combines predicted transcription factor (TF) binding probabilities with bounded evidence investigation, deterministic verification, and reporting from fixed evidence facts.

This private repository currently contains repository documentation and templates. The implementation stages below reflect the project's current design as described by its maintainer; Python source, dependency specifications, runnable entry points, and datasets are not yet present in this checkout. Installation commands and execution examples will be added when they can be checked against the source.

## Current components

| Component | Role |
| --- | --- |
| DVR prediction | Produces TF binding probabilities. |
| Deterministic investigation | Uses ADASTRA, JASPAR, and ENCODE cCRE evidence. |
| Deterministic verifier | Applies verification, abstention, and stopping rules. |
| Optional Anthropic planner | Chooses only allowed evidence actions. |
| Reporter | Generates prose from fixed evidence facts. |
| AlphaGenome Atlas | Evaluated as a standalone computational cross-reference; not yet wired into the investigation loop. |

The optional planner selects evidence actions within the allowed action space. Verification and stopping remain deterministic. The reporter expresses the fixed evidence facts in prose. A computational cross-reference should be identified as such rather than presented as independent experimental validation.

## Repository guide

- [Architecture](docs/architecture.md): component boundaries and evidence flow.
- [Data setup](docs/data_setup.md): local resource organization and provenance.
- [Examples](examples/README.md): requirements for future small, reproducible examples.
- [Results](results/README.md): expectations for retained summaries and local run artifacts.
- [Environment template](.env.example): proposed local settings, pending alignment with source.
- [Citation metadata](CITATION.cff): provisional project citation.

## Getting started

1. Read the architecture and data setup documents.
2. Keep credentials, weights, knowledgebase databases, and bulk outputs outside version control. The repository provides ignore rules for common artifact types and local resource directories.
3. If useful for local planning, copy `.env.example` to `.env` and fill in local values. No environment loader or configuration contract is supplied yet.
4. Add installation and run instructions only after the relevant source and dependency files are available.

Do not commit API keys, model weights, knowledgebase databases, large result files, or user-specific server paths. Ignore rules do not remove files already tracked by Git.

## Development and reporting

Use the GitHub issue templates for bugs, feature requests, and evidence/data questions. Use the pull request template to record the scope of a change, its effect on evidence interpretation, and the checks performed.

No benchmark scores, supported genome assemblies, action names, thresholds, or dataset versions are asserted by this documentation. Those details must be documented from the implementation and actual run configuration.

## Citation and licensing

See [CITATION.cff](CITATION.cff). Contributor attribution is provisional; author names, repository URL, release version, and any publication identifier should be supplied by the maintainer before a formal release.

No license has been selected in this repository. Resource-specific access and redistribution terms must be checked separately.
