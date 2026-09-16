# ARGUS architecture

This document records the maintainer-described components and their intended boundaries. Source code is not yet present in this checkout, so module names, schemas, control-flow details, thresholds, and command-line interfaces remain to be documented from the implementation.

## Evidence flow

```text
Variant input
    |
    v
DVR prediction --> TF binding probabilities
    |
    v
Deterministic investigation <--> ADASTRA / JASPAR / ENCODE cCRE
    ^            |
    |            v
    +--- Deterministic verification and stopping/abstention
                 |
                 v
          Fixed evidence facts --> Reporter --> Prose report

Optional Anthropic planner --> chooses only allowed evidence actions
                               for the investigation

AlphaGenome Atlas --> standalone computational cross-reference
                     (not connected to the investigation loop)
```

The diagram is conceptual, not a verified execution trace.

## Component responsibilities

### DVR prediction

The prediction stage produces TF binding probabilities. Document the model identity, input requirements, output schema, and probability interpretation when the implementation is available. Do not infer calibration, effect direction, or clinical significance from the presence of a probability alone.

### Investigation

The deterministic investigation uses ADASTRA, JASPAR, and ENCODE cCRE. The implementation must supply the exact query actions, evidence representations, resource compatibility checks, and update rules; this document does not invent an action registry.

### Verification, abstention, and stopping

A deterministic verifier and deterministic abstention/stopping rules govern the investigation. The exact acceptance conditions, stopping criteria, budgets, and abstention reason codes remain to be documented from source. Keep a lack of retrieved evidence distinguishable from evidence that supports a negative finding.

### Optional planner

The Anthropic planner chooses only allowed evidence actions. Its role is action selection within the constrained workflow, while verification and stopping remain deterministic. Document the planner's input/output contract, validation, error handling, and fallback behavior when the source is added. Do not assume a fallback implementation from this design description.

### Reporter

The reporter generates prose from fixed evidence facts. Reporting should preserve provenance and uncertainty and should not introduce new biological claims or silently convert a computational prediction into observed evidence. The precise fact format and report checks remain to be documented.

### AlphaGenome Atlas

AlphaGenome Atlas has been evaluated as a standalone computational cross-reference. It is not yet wired into the investigation loop. Keep its findings and provenance separate from loop evidence and verifier decisions. Integration would require an explicit design and implementation change.

## Reproducibility information to record

For future runs, record the code revision, model and resource versions, genome assembly, coordinate convention, normalized input, selected actions, retrieved evidence provenance, verification outcomes, stopping/abstention reasons, and report inputs. If the optional planner is used, also record its model identifier and relevant settings without secrets.

These are documentation requirements for reproducibility, not a claim that a run-manifest schema or logging system already exists. Deterministic control rules do not by themselves guarantee identical outputs across changed data, models, or optional planner responses.
