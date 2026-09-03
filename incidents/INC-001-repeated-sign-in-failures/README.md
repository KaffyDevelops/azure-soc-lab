# INC-001: Repeated Microsoft Entra ID Sign-in Failures

> **Classification:** Simulated lab incident / detection validation  
> **Related detection:** [DET-001](../../detections/DET-001-brute-force.md)  
> **MITRE ATT&CK:** T1110 Brute Force  
> **Status:** Evidence capture in progress

## Purpose

This incident record validates DET-001 end to end by connecting a controlled authentication test to Microsoft Entra ID telemetry, a KQL detection result, a Microsoft Sentinel alert or incident, analyst investigation and a documented response decision.

The evidence chain is:

**controlled lab activity → `SigninLogs` telemetry → DET-001 KQL result → alert / incident → analyst investigation → findings → response decision**

## Safety and Scope

Use only a dedicated test identity in a Microsoft Entra ID tenant you own or are authorised to test.

Do **not** use:

- a production administrator account
- another person's account
- an account whose lockout behaviour you do not understand
- credentials belonging to an organisation you are not authorised to test

Before generating failures, confirm that six failed attempts will not violate the tenant's lockout policy. If necessary, use a disposable lab-only user configured specifically for this exercise.

## Test Identity

Record only sanitised information in this public repository.

- **Public alias:** `lab-user-01`
- **Actual UPN:** Do not publish if it contains personal or organisation-sensitive information.
- **Privilege level:** Standard user only
- **Purpose:** Detection validation

## Pre-Test Conditions

Before generating activity, confirm:

- [ ] Microsoft Sentinel is enabled for the workspace.
- [ ] Microsoft Entra ID sign-in logs are arriving in the `SigninLogs` table.
- [ ] The test account is a non-privileged lab account.
- [ ] The test account can safely tolerate at least six failed sign-in attempts under the current lockout policy.
- [ ] The analyst records the UTC test start time.
- [ ] No secrets, passwords or sensitive identifiers will appear in screenshots committed publicly.

## Expected Result Before Testing

The expected behaviour is:

1. At least six invalid-password sign-in events are generated for the same lab identity from the same source IP.
2. The events appear in `SigninLogs` with `ResultType == "50126"`.
3. DET-001 groups the events by IP address and user principal name.
4. Because the count exceeds five, the user/IP combination appears in the query result.
5. If an analytics rule is configured for DET-001, Sentinel creates an alert or incident.
6. The analyst reviews the surrounding authentication telemetry and determines that the activity is authorised lab testing rather than a genuine compromise.

## Controlled Test Procedure

### Step 1: Record the test window

Record the UTC start time before beginning.

```text
Test start (UTC): ____________________
Test end (UTC):   ____________________
```

### Step 2: Generate controlled failed sign-ins

Using the dedicated standard lab account, perform **six failed sign-in attempts** from the same device/network by deliberately entering an incorrect password.

Stop immediately if the tenant begins locking the account or otherwise behaves unexpectedly.

Do not exceed the number of failures necessary to validate the lab detection.

### Step 3: Allow telemetry to arrive

Wait for the Microsoft Entra ID sign-in events to appear in the Log Analytics workspace. Ingestion is not always instantaneous.

Use the investigation queries in [`investigation-queries.kql`](investigation-queries.kql) to confirm the events have arrived before evaluating DET-001.

### Step 4: Run DET-001

Run the implemented detection query:

```kusto
SigninLogs
| where ResultType == "50126"
| summarize FailedAttempts = count() by IPAddress, UserPrincipalName
| where FailedAttempts > 5
| project IPAddress, UserPrincipalName, FailedAttempts
| order by FailedAttempts desc
```

Expected result: at least one row for the controlled lab identity/source with `FailedAttempts >= 6`.

### Step 5: Capture Evidence E01

Capture a screenshot showing the DET-001 query result.

Save it as:

```text
evidence/E01-det001-query-result.png
```

Redact or crop sensitive tenant/user information where necessary while preserving the security-relevant evidence.

### Step 6: Validate the alert / incident

If DET-001 has been configured as a Sentinel analytics rule, confirm the generated alert or incident and open it for investigation.

Capture:

- incident or alert title
- severity
- creation time
- affected identity/entity where safe to show
- evidence that the incident corresponds to the controlled activity

Save the screenshot as:

```text
evidence/E02-sentinel-incident.png
```

If no analytics rule currently exists, record that fact rather than inventing an incident. The issue cannot be closed as complete until an alert/incident stage has been implemented or the project scope is formally changed.

### Step 7: Investigate the surrounding telemetry

Use [`investigation-queries.kql`](investigation-queries.kql) to review:

- all sign-ins for the test user in the test window
- source IP
- application/client context
- failure descriptions
- any successful authentication near the failures
- whether the same source affected other accounts

Capture the most useful investigation view as:

```text
evidence/E03-investigation-timeline.png
```

## Investigation Timeline

Complete this table from the actual telemetry. Use UTC.

| Time (UTC) | Event | Evidence | Analyst interpretation |
|---|---|---|---|
| TBD | Controlled test begins | Lab notes | Authorised simulation |
| TBD | First failed sign-in recorded | `SigninLogs` | Expected test activity |
| TBD | Sixth failed sign-in recorded | `SigninLogs` | DET-001 threshold exceeded |
| TBD | DET-001 returns matching row | E01 | Detection validated |
| TBD | Sentinel alert/incident created | E02 | Detection escalated for investigation |
| TBD | Surrounding telemetry reviewed | E03 | Scope and context assessed |
| TBD | Investigation closed | Incident record | Authorised lab activity confirmed |

## Investigation Questions

The analyst should answer these using evidence rather than assumption:

1. Is the affected identity the designated lab user?
2. Did the failures originate from the expected test source IP?
3. Did the activity occur inside the documented test window?
4. Were more accounts targeted by the same source IP?
5. Was there a successful sign-in shortly after the failures?
6. Were any privileged actions or additional security alerts associated with the identity?
7. Is there any evidence inconsistent with the authorised simulation?

## Findings

Complete after the test.

### Confirmed facts

- **Identity:** Pending evidence
- **Source:** Pending evidence
- **Failure count:** Pending evidence
- **Time window:** Pending evidence
- **Alert/incident:** Pending evidence
- **Related successful sign-in:** Pending evidence
- **Other affected identities:** Pending evidence

### Preliminary interpretation

The planned activity is an authorised lab simulation intended to validate DET-001. The final conclusion must still be based on the captured telemetry and incident evidence.

## False-Positive Assessment

DET-001 can match benign behaviour. Plausible explanations include:

- repeated user typing errors
- cached or stale credentials
- an application or device repeatedly using an old password
- authorised security testing

For INC-001, the intended benign explanation is **authorised lab testing**. This conclusion should only be recorded as confirmed after the timestamps, identity and source are matched to the documented test window.

## Response Decision

### Expected lab decision

If the evidence matches the authorised simulation exactly:

**Disposition:** Benign positive / authorised test activity.

**Reasoning:** The failed sign-ins were deliberately generated by the authorised tester using a dedicated non-privileged lab account during the documented validation window.

**Production-equivalent analyst actions:**

- verify the account owner and source
- check for successful authentication following the failures
- review MFA and privileged access
- inspect whether the source targeted additional identities
- reset credentials or revoke sessions if compromise cannot be ruled out
- block or investigate the source when organisational policy and evidence justify it

The lab incident should not recommend destructive containment when the evidence confirms authorised testing.

## Lessons and Tuning Notes

The current DET-001 logic is intentionally simple. This incident should capture observations that feed Issue #3, including:

- whether an explicit time window is needed
- whether the `> 5` threshold is meaningful for this environment
- whether a later successful sign-in should be correlated
- whether additional identity, device or location context improves triage
- how the detection differs from password spraying across multiple users

Do not change DET-001 as part of INC-001 unless the change is separately tested and documented.

## Public Evidence Review

Before committing screenshots:

- [ ] No password or secret is visible.
- [ ] No access token, session token or API key is visible.
- [ ] Tenant identifiers are removed where unnecessary.
- [ ] Personal email/UPN is redacted or replaced with a lab alias where appropriate.
- [ ] IP addresses are shown only if safe and useful to the technical evidence.
- [ ] Browser tabs/bookmarks do not expose private information.
- [ ] The screenshot still proves the technical step after redaction.

## Evidence Register

| Evidence ID | Description | File | Status |
|---|---|---|---|
| E01 | DET-001 KQL query result showing threshold exceeded | `evidence/E01-det001-query-result.png` | ⏳ Required |
| E02 | Microsoft Sentinel alert/incident created from the controlled activity | `evidence/E02-sentinel-incident.png` | ⏳ Required |
| E03 | Sign-in telemetry / investigation view supporting the timeline | `evidence/E03-investigation-timeline.png` | ⏳ Required |

## Completion Gate

INC-001 can be marked complete only when:

- [x] Safe test scenario is documented.
- [x] Expected result is written before testing.
- [ ] Query-result evidence is captured.
- [ ] Alert or incident evidence is captured.
- [ ] Investigation timeline is populated with real timestamps.
- [x] False-positive assessment framework is documented.
- [x] Response decision framework is documented.
- [x] Lessons/tuning questions are documented.
- [ ] Public evidence is reviewed for sensitive information.

## Disclosure

> This is a simulated portfolio lab incident completed in a controlled environment. It is not presented as production SOC employment experience.
