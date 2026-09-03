# Incident Investigation Records

This folder is reserved for documented lab incident investigations.

Each incident should be stored in its own directory, for example:

```text
incidents/
└── INC-001-repeated-sign-in-failures/
    ├── README.md
    └── evidence/
```

## Incident Record Template

Use the following structure for each investigation.

### Incident ID

`INC-XXX`

### Classification

Simulated lab incident / detection validation.

### Summary

What happened and how was it detected?

### Detection Source

Which query, alert or rule surfaced the activity?

### Timeline

| Time | Event | Evidence |
|---|---|---|
| | | |

### Affected Assets or Identities

List only sanitised information suitable for public publication.

### Investigation Steps

1. Confirm alert details.
2. Review surrounding telemetry.
3. Identify related identities, IPs or activities.
4. Assess scope and severity.
5. Record findings.

### Findings

What did the evidence support?

### False-Positive Assessment

What benign explanations were considered?

### Response Decision

What action would be reasonable and why?

### Lessons Learned

What should be tuned, monitored or improved?

### Disclosure

> This is a simulated portfolio lab incident and is not presented as production employment experience.
