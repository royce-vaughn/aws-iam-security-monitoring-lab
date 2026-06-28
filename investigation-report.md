# Identity & Access Security Monitoring Investigation Report

## Objective

The objective of this lab was to build an AWS identity and access monitoring environment that demonstrates least privilege, role-based access control, CloudTrail audit logging, and investigation of denied access events.

This project was built to connect cloud identity security with real SOC and GRC work. Instead of only creating users and groups, I tested the permissions and reviewed the results in CloudTrail.

---

## Environment

- AWS IAM
- AWS CloudTrail
- IAM Users
- IAM User Groups
- AWS Managed Policies
- Custom IAM Policy
- CloudTrail Event History

---

## Identity Structure

Four IAM groups were created:

| Group | Purpose |
|---|---|
| `AdminBreakGlass` | Emergency administrative access |
| `SecurityAnalysts` | Security review and investigation |
| `ReadOnlyAuditors` | GRC-style read-only review |
| `Developers` | Limited developer access |

Four IAM users were created:

| User | Group |
|---|---|
| `admin-royce` | `AdminBreakGlass` |
| `soc-analyst-test` | `SecurityAnalysts` |
| `grc-auditor-test` | `ReadOnlyAuditors` |
| `dev-user-test` | `Developers` |

---

## Security Controls Implemented

- MFA enabled for the root account
- MFA enabled for the admin IAM user
- IAM group-based access control
- Least-privilege permissions
- Custom developer policy
- Multi-region CloudTrail trail
- Read and write management event logging
- AccessDenied investigation through CloudTrail Event History

---

## Test 1: Developer IAM Access Denied

The `dev-user-test` account attempted to access IAM account summary information using the `GetAccountSummary` API call.

### Result

The action was denied with an `AccessDenied` error.

### Analysis

This confirmed that the developer user could not access restricted IAM information. The custom developer policy helped enforce least privilege by blocking identity-related visibility and administrative actions.

### Evidence

- `screenshots/dev-user-iam-accessdenied.png`
- `evidence/dev-user-iam-accessdenied.json`

---

## Test 2: Developer CloudTrail Access Denied

The `dev-user-test` account attempted to list CloudTrail event data stores using the `ListEventDataStores` API call.

### Result

The action was denied with an `AccessDenied` error.

### Analysis

This confirmed that the developer role could not access security logging resources. This is important because logging systems should be protected from users who do not need access to them.

### Evidence

- `screenshots/dev-user-cloudtrail-accessdenied.png`
- `evidence/dev-user-cloudtrail-accessdenied.json`

---

## Test 3: GRC Auditor View-Only Access

The `grc-auditor-test` account was able to view IAM dashboard information but was unable to create IAM groups.

### Result

View access was allowed. Modification access was denied.

### Analysis

This validated the auditor role as a read-only user suitable for governance, risk, and compliance review. The auditor could review configuration but could not make changes.

### Evidence

- `screenshots/grc-auditor-view-only-iam-dashboard.png`
- `screenshots/grc-auditor-create-group-denied.png`
- `screenshots/cloudtrail-grc-auditor-creategroup-accessdenied.png`
- `evidence/grc-auditor-creategroup-accessdenied.json`

---

## Test 4: SOC Analyst SecurityAudit Access

The `soc-analyst-test` account was able to review security-related IAM information but was unable to create IAM groups.

### Result

Security review access was allowed. Administrative modification was denied.

### Analysis

This validated that the SOC analyst role could review security posture without having administrative permissions. This supports separation of duties and reduces risk.

### Evidence

- `screenshots/soc-analyst-securityaudit-view.png`
- `screenshots/soc-analyst-create-group-denied.png`
- `screenshots/cloudtrail-soc-analyst-creategroup-accessdenied.png`
- `evidence/soc-analyst-creategroup-accessdenied.json`

---

## Investigation Summary

The denied access events confirmed that the IAM design was working as intended.

The developer user could not access IAM or CloudTrail security logging resources. The auditor user could view information but could not make IAM changes. The SOC analyst user had security visibility but did not have administrative permissions.

CloudTrail recorded the denied attempts, which allowed the events to be reviewed and documented.

---

## Conclusion

This lab demonstrated identity security monitoring through AWS IAM and CloudTrail.

The environment enforced least privilege, separated responsibilities by job function, and captured denied access attempts in CloudTrail for investigation.

This project supports SOC Analyst, Security Analyst, GRC Analyst, and Cloud Security career paths by showing hands-on experience with identity access controls, audit logging, and access-denied investigation.