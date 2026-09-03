# Detection Validation Plan

A KQL query executing successfully is not enough to call a detection complete.

This project uses the following validation standard.

## 1. Define the Behaviour

State clearly what behaviour should trigger the detection and why it matters.

Example:

> Repeated failed sign-in attempts against the same Microsoft Entra ID account from the same source IP.

## 2. Confirm the Data Source

Document:

- table name
- relevant fields
- event/result codes
- expected ingestion path

## 3. Build the Query

The query should be readable, commented and scoped to the security objective.

## 4. Define a Test Scenario

Create a controlled scenario that should produce the expected signal.

The scenario must be safe, authorised and appropriate for a lab environment.

## 5. Define Expected Output

Before running the test, state what successful detection should look like.

For example:

- user principal name returned
- source IP returned
- failure count above threshold
- relevant event time visible

## 6. Capture Evidence

Capture the query output and, where applicable, the generated alert or incident.

## 7. Perform Analyst Triage

Review surrounding telemetry and answer the triage questions documented with the detection.

## 8. Review False Positives

Document at least one plausible benign explanation and how an analyst could distinguish it from malicious behaviour.

## 9. Record Response Guidance

Document reasonable response options based on severity and supporting evidence.

## 10. Record Limitations

State what the detection does not cover and what would need to change for production use.

## Completion Criteria

A detection can move from **Planned** to **Implemented** when the repository contains:

- [ ] detection documentation
- [ ] KQL query
- [ ] defined data source
- [ ] test scenario
- [ ] expected output
- [ ] captured evidence
- [ ] triage guidance
- [ ] false-positive notes
- [ ] response guidance
- [ ] limitations

This checklist is intentionally strict because the goal of the repository is demonstrable capability rather than repository volume.
