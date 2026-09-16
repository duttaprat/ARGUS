---
name: Bug report
about: Report an implementation, evidence-handling, or documentation defect.
title: "[Bug] "
labels: []
assignees: []
---

## Problem

Describe the observed behavior and the expected behavior.

## Reproduction

Provide a minimal sanitized input and the exact command or steps. Use relative paths and omit secrets, database contents, model weights, and large outputs.

## Context

- Code revision:
- Component affected:
- Operating system and dependency versions, if relevant:
- Resource/model versions, genome assembly, and coordinate convention, if relevant:
- Optional planner enabled and model identifier, if relevant:

## Evidence and impact

Include a short sanitized error or trace excerpt. Explain whether the issue changes retrieved facts, verification, stopping/abstention, or report wording. Identify standalone AlphaGenome Atlas behavior separately.

## Checks

- [ ] I removed credentials and user-specific server paths.
- [ ] I distinguished observed output from expected or inferred behavior.
