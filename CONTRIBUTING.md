# Contributing

Contributions that improve the technical quality, detection logic, documentation or validation approach are welcome.

## Good Contribution Areas

- KQL improvements
- detection tuning ideas
- MITRE ATT&CK mapping corrections
- false-positive analysis
- analyst triage guidance
- documentation improvements
- safe lab validation ideas

## Contribution Expectations

Please:

1. Open an issue describing the proposed change where practical.
2. Keep detections evidence-led and clearly distinguish implemented work from planned work.
3. Do not include production secrets, credentials, personal data or sensitive tenant information.
4. Explain why a query or threshold change improves the detection.
5. Include references where a technical claim or mapping benefits from external support.

## Detection Contributions

A new detection should ideally include:

- detection ID and title
- security objective
- data source
- KQL query
- MITRE ATT&CK mapping where appropriate
- test approach
- false-positive considerations
- triage guidance
- response guidance
- limitations

## Ethics

Only use test or simulated activity in environments where you have explicit authorisation.

This repository is for defensive cloud security learning and detection engineering.
