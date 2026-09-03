# Evidence Standard

The purpose of this repository is to demonstrate practical security engineering, not just list tools used.

Every implemented detection or incident scenario should therefore include evidence that helps another reviewer understand what happened and how the conclusion was reached.

## Minimum Evidence for an Implemented Detection

A detection should include:

1. **Security objective**
2. **Data source**
3. **Query or detection logic**
4. **MITRE ATT&CK mapping**, where appropriate
5. **Expected result**
6. **Validation or simulation approach**
7. **Captured output or screenshot**
8. **False-positive considerations**
9. **Analyst triage guidance**
10. **Response recommendations**
11. **Limitations and tuning notes**

## Screenshot Rules

A screenshot should prove something meaningful.

### Useful evidence

- a query result that shows the triggering activity
- an alert or incident created from that activity
- investigation context showing the affected identity, source or timeline
- a control configuration directly relevant to the detection

### Weak evidence

- a product homepage with no relation to the scenario
- a generic Azure Portal screen
- screenshots without captions or context

## Public Redaction Checklist

Before committing evidence publicly, check for:

- tenant IDs
- subscription IDs
- personal email addresses
- private IP information that should not be disclosed
- access tokens
- API keys
- secrets
- account identifiers
- sensitive organisation names

Use synthetic or redacted values where disclosure is unnecessary.

## Evidence Naming Convention

Recommended format:

`DET-001-01-query-result.png`

`DET-001-02-alert.png`

`DET-001-03-investigation.png`

`INC-001-01-timeline.png`

This makes it easier to trace each artefact to the detection or incident it supports.

## Integrity Principle

Do not label planned work as completed. Do not describe simulated incidents as production incidents. Do not present portfolio labs as employment experience.

The value of the repository comes from transparent, reviewable evidence.
