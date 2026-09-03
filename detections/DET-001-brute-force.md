# DET-001: Repeated Failed Microsoft Entra ID Sign-ins

## Status

**Implemented**

## Security Objective

Identify repeated failed Microsoft Entra ID authentication attempts associated with the same source IP address and user principal name.

This pattern can indicate password guessing, brute-force behaviour, credential abuse, user error or a misconfigured automated process. The purpose of the detection is to surface activity that warrants analyst review.

## Data Source

- Microsoft Entra ID sign-in logs
- Log Analytics table: `SigninLogs`

## Detection Logic

```kusto
SigninLogs
| where ResultType == "50126"
| summarize FailedAttempts = count() by IPAddress, UserPrincipalName
| where FailedAttempts > 5
| project IPAddress, UserPrincipalName, FailedAttempts
| order by FailedAttempts desc
```

## Detection Explanation

The query:

1. Filters sign-in events to result code `50126`, representing invalid username or password authentication failures.
2. Groups events by source IP address and user principal name.
3. Counts the number of failed attempts for each combination.
4. Returns combinations with more than five failed attempts.
5. Sorts the result so the highest failure counts are reviewed first.

## MITRE ATT&CK Mapping

- **T1110: Brute Force**

The mapping is used to provide threat context. It does not prove that every matching event is malicious.

## Analyst Triage Questions

When this detection fires, investigate:

- Is the affected user account known and active?
- Is the source IP expected for the user or organisation?
- Did any successful sign-in occur shortly after the failures?
- Are other accounts receiving failures from the same source IP?
- Is the activity concentrated in a short time window?
- Is MFA configured for the account?
- Are there related alerts or privileged actions?
- Could the activity be caused by an outdated password in an application, device or service?

## Potential False Positives

Possible benign explanations include:

- a user repeatedly entering an incorrect password
- cached credentials on a device
- an application or service using an expired password
- expected testing activity in a controlled lab

## Response Guidance

Depending on the surrounding evidence and assessed severity, possible actions include:

- continue monitoring if the behaviour is confirmed benign
- contact the account owner for verification
- review recent successful sign-ins
- reset credentials where compromise is suspected
- revoke active sessions where appropriate
- enforce or verify MFA
- review the account's privileged access
- block or investigate the source IP if organisational controls and evidence support that action

## Validation Status

The KQL query is implemented in this repository. Supporting Sentinel screenshots are available under `/images`.

The next validation improvement is to capture a repeatable test scenario that links:

**generated activity → matching query result → alert/incident → investigation notes → response decision**.

## Limitations

- The `> 5` threshold is a lab value and is not presented as a universal production threshold.
- Grouping by both IP address and user can miss password spraying patterns where one source targets many accounts with fewer attempts per account.
- Additional time-bounding and enrichment would be appropriate in a production detection.
- A matching event is an investigation signal, not proof of malicious intent.

## Related Evidence

- [`../kql-queries/brute-force.kql`](../kql-queries/brute-force.kql)
- [`../images/sentinel-dashboard.png`](../images/sentinel-dashboard.png)
- [`../images/incident-alert.png`](../images/incident-alert.png)

## Future Tuning Ideas

- add a defined time window using `TimeGenerated`
- enrich source IPs with location or threat-intelligence context
- correlate failed attempts with subsequent successful authentication
- create a separate password-spray detection grouped primarily by source IP
- apply environment-specific thresholds and exclusions
