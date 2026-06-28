# GRC Control Mapping

This file maps the lab activity to common GRC and security control ideas.

| Control Area | What Was Implemented |
|---|---|
| Least Privilege | Developers received limited read-only permissions with explicit denies |
| Access Control | Users were assigned permissions through IAM groups |
| Separation of Duties | Admin, analyst, auditor, and developer roles were separated |
| Audit Logging | CloudTrail was enabled to record management events |
| Monitoring | Denied access attempts were reviewed in CloudTrail Event History |
| Identity Governance | Auditor role was limited to view-only access |
| Administrative Control | Admin access was isolated to the `AdminBreakGlass` group |
| Incident Investigation | AccessDenied events were reviewed and documented |
| Evidence Collection | Screenshots and redacted CloudTrail event records were collected |
| Change Control | Non-admin users were blocked from creating IAM groups |

---

## GRC Summary

This lab supports basic governance and audit concepts by showing that users were separated by role, permissions were limited based on job function, and denied actions were logged for review.

The main control theme is:

> Give users only the access they need, monitor what they do, and keep evidence that proves the controls are working.

---

## SOC Relevance

From a SOC perspective, the lab shows how denied activity can still be useful during an investigation.

Even though the blocked actions did not succeed, CloudTrail still recorded important details like:

- Which user made the request
- What action they attempted
- What service was involved
- Whether the action was denied
- When the event happened

That is useful for identifying account misuse, suspicious access attempts, or privilege escalation attempts.

---

## GRC Relevance

From a GRC perspective, this lab shows:

- Access was role-based
- Permissions were limited
- Administrative access was separated
- Auditor access was read-only
- Evidence was collected to prove controls were working

This kind of evidence can support access reviews, control testing, and audit readiness.