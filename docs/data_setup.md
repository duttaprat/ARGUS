# Local data setup

ARGUS resources are managed locally and are not included in this repository. Source and dependency files are not yet present, so this guide describes organization and provenance rather than executable installation instructions.

## Resource inventory

| Resource | Current role | Record for reproducibility |
| --- | --- | --- |
| DVR weights and supporting inputs | TF binding prediction | Model identifier, weight checksum, input requirements, preprocessing |
| ADASTRA | Investigation evidence | Release or snapshot, assembly, relevant context labels |
| JASPAR | Investigation evidence | Release, collection, identifiers used |
| ENCODE cCRE | Investigation evidence | Release, assembly, annotation context |
| AlphaGenome Atlas | Standalone computational cross-reference | Version or snapshot, query inputs, output interpretation |

Obtain resources through their providers or approved project storage and check the applicable access and redistribution terms. No specific release, download URL, assembly, or storage format is prescribed here because these have not yet been verified against the implementation.

## Suggested layout

```text
data/
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

These directories are suggestions, not hard-coded runtime requirements. The data and weights directories and generated results are ignored by Git. Keep machine-specific paths in local configuration only. The checked-in documentation and examples should use relative paths or neutral placeholders.

## Setup sequence

1. Identify the exact resource versions and model inputs required by the implementation once it is available.
2. Place resources in local ignored storage. Record checksums and provider release identifiers in a local manifest.
3. Confirm genome assembly, contig naming, coordinate convention, allele representation, and relevant biological contexts across inputs and evidence sources. Record any transformations explicitly.
4. If using `.env.example`, copy it to `.env` and adapt it locally. Its variable names are proposed; the repository currently supplies no loader or validated configuration contract.
5. Configure an Anthropic credential locally only if the optional planner is used. Leave the example credential empty in Git.
6. Add a small smoke-test procedure after runnable entry points and dependency specifications are committed. No working execution command is claimed here.

Keep AlphaGenome Atlas setup separate from investigation setup. Having local Atlas resources does not enable loop integration.

## Provenance record

A local resource manifest should capture the resource name, provider, release/snapshot, retrieval date, file checksum, assembly where applicable, coordinate convention, preprocessing, and usage restrictions. Share sanitized metadata when useful; exclude credentials, signed URLs, private server paths, and database contents.

## Before committing artifacts

Check `git status --short` and review the staged diff. `.gitignore` covers common local resources, model formats, and generated directories, but it cannot classify arbitrary files by size or content and does not affect already tracked files. Keep bulk files in ignored storage even if their extension is not listed.
