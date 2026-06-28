# IAM Design

## Objective

This lab was designed to demonstrate role-based access control, least privilege, and identity monitoring using AWS IAM and CloudTrail.

The goal was to separate users by job function and avoid giving broad access to accounts that did not need it.

---

## IAM Groups

| Group | Policy | Purpose |
|---|---|---|
| `AdminBreakGlass` | `AdministratorAccess` | Emergency administrative access |
| `SecurityAnalysts` | `SecurityAudit` | Security review and investigation |
| `ReadOnlyAuditors` | `ViewOnlyAccess` | GRC-style read-only review |
| `Developers` | `DeveloperLimitedReadOnly` | Limited developer access with explicit denies |

---

## IAM Users

| User | Group | Purpose |
|---|---|---|
| `admin-royce` | `AdminBreakGlass` | Administrative configuration |
| `soc-analyst-test` | `SecurityAnalysts` | SOC/security analyst testing |
| `grc-auditor-test` | `ReadOnlyAuditors` | Auditor testing |
| `dev-user-test` | `Developers` | Limited developer testing |

---

## Design Notes

Permissions were assigned through IAM groups instead of directly attaching policies to users.

This makes the access model easier to manage, review, and explain. In a real environment, this also makes it easier to onboard users, remove access, and audit permissions by job function.

---

## Security Design

The lab separates duties between administrators, security analysts, auditors, and developers.

The developer role was intentionally restricted from IAM, CloudTrail, GuardDuty, account, and organization-level actions to protect identity and logging controls.

The auditor role was designed for view-only access.

The SOC analyst role was designed for security visibility without administrative modification rights.

The admin role was kept separate as a break-glass style administrative account.

---

## Why This Matters

Identity is one of the most important parts of cloud security. If users have too much access, one compromised account can become a much bigger incident.

This lab shows how basic role separation and least privilege can reduce that risk while still allowing users to do their jobs.