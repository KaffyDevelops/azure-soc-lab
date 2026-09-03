# INC-001 Evidence Capture Guide

This folder should contain only sanitised evidence generated during the authorised INC-001 lab validation.

## Required Evidence

### E01: DET-001 query result

**Filename:** `E01-det001-query-result.png`

Capture the Microsoft Sentinel / Log Analytics results pane after running DET-001.

The screenshot should prove:

- the DET-001 query was executed
- the lab identity/source combination exceeded the threshold
- `FailedAttempts` is visible

Redact personal or tenant-sensitive values where necessary.

---

### E02: Sentinel alert or incident

**Filename:** `E02-sentinel-incident.png`

Capture the alert or incident generated from the controlled test activity.

The screenshot should show, where available:

- incident or alert title
- severity
- creation time
- affected entity/identity in sanitised form
- enough context to connect the incident to DET-001

Do not fabricate this stage. If DET-001 has not yet been configured as an analytics rule, create and validate the rule before marking INC-001 complete.

---

### E03: Investigation timeline

**Filename:** `E03-investigation-timeline.png`

Use the queries in `../investigation-queries.kql` to show the authentication timeline around the controlled event.

The screenshot should ideally demonstrate:

- timestamps
- failed sign-ins
- source IP consistency
- application/client context
- whether a successful sign-in followed the failures

## Redaction Rules

Before committing any screenshot publicly, verify that it contains no:

- passwords
- access tokens
- session tokens
- API keys
- unnecessary tenant identifiers
- private browser tabs or bookmarks
- unrelated personal information

A redacted screenshot must still preserve enough information to prove the technical step.

## Evidence Integrity

Do not edit a screenshot to alter security results, counts, timestamps or findings. Cropping and privacy redaction are acceptable. The purpose of this folder is to preserve a defensible evidence trail, not create a polished mock-up.
