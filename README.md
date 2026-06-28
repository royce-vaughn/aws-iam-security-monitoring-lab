# AWS IAM Security Monitoring Lab

## Overview

This project is an AWS identity and access security lab focused on IAM, least privilege, CloudTrail logging, and investigating denied access events.

I built this lab to practice how security teams manage user permissions, separate job roles, monitor account activity, and investigate unauthorized access attempts.

The main goal was simple:

> Build an AWS environment where different users have different levels of access, then use CloudTrail to confirm that restricted actions are denied and logged properly.

This project connects directly to SOC, GRC, and cloud security work because identity is one of the biggest parts of modern security.

---

## Tools and Services Used

- AWS IAM
- AWS CloudTrail
- IAM Users
- IAM User Groups
- AWS Managed Policies
- Custom IAM Policy
- CloudTrail Event History

---

## What I Built

I created a small AWS identity environment with separate roles for different job functions.

| Group | Purpose |
|---|---|
| `AdminBreakGlass` | Emergency/admin access |
| `SecurityAnalysts` | Security review and investigation |
| `ReadOnlyAuditors` | GRC-style read-only access |
| `Developers` | Limited developer access |

I also created test users for each role:

| User | Group |
|---|---|
| `admin-royce` | `AdminBreakGlass` |
| `soc-analyst-test` | `SecurityAnalysts` |
| `grc-auditor-test` | `ReadOnlyAuditors` |
| `dev-user-test` | `Developers` |

Permissions were assigned through groups instead of directly attaching policies to individual users. This made the setup cleaner and closer to how access is usually managed in real environments.

---

## Security Controls Implemented

For this lab, I implemented:

- MFA for the root account
- MFA for the admin IAM user
- Role-based access using IAM groups
- Least-privilege permissions
- A custom limited developer policy
- Multi-region CloudTrail logging
- CloudTrail Event History review
- AccessDenied investigation evidence

The main security idea behind the lab was:

> Users should only have the access they need to do their job — nothing more.

---

## Custom Developer Policy

The `Developers` group was given a custom policy called:

`DeveloperLimitedReadOnly`

This policy allowed basic read-only visibility into common services, but denied access to sensitive identity, account, logging, and security services.

The developer user was intentionally blocked from actions like:

- IAM administration
- CloudTrail changes
- GuardDuty access
- Account-level changes
- Organization-level changes

This helped validate least privilege and protected important security logging controls.

---

## Test Scenarios

I tested the lab by signing in as different users and attempting actions based on their role.

| Scenario | User | Result |
|---|---|---|
| Developer attempted IAM access | `dev-user-test` | AccessDenied |
| Developer attempted CloudTrail access | `dev-user-test` | AccessDenied |
| Auditor viewed IAM dashboard | `grc-auditor-test` | Allowed |
| Auditor attempted to create an IAM group | `grc-auditor-test` | AccessDenied |
| SOC analyst viewed security-related IAM information | `soc-analyst-test` | Allowed |
| SOC analyst attempted to create an IAM group | `soc-analyst-test` | AccessDenied |

---

## CloudTrail Investigation

After testing restricted actions, I reviewed the events in CloudTrail Event History.

CloudTrail captured details like:

- User name
- Event name
- Event source
- Timestamp
- Error code
- Source IP address
- Request details

One example was the `dev-user-test` account attempting to access IAM account summary information. The event was denied with `AccessDenied`, which confirmed that least-privilege controls were working and that CloudTrail captured the denied activity for investigation.

---

## Evidence Collected

Evidence was collected through screenshots and CloudTrail event records.

Examples of evidence include:

- IAM users
- IAM groups
- CloudTrail trail status
- Developer denied IAM access
- Developer denied CloudTrail access
- Auditor view-only access
- Auditor denied IAM modification
- SOC analyst view access
- SOC analyst denied IAM modification

Sensitive information was redacted before publishing, including account IDs, ARNs, source IP addresses, access key IDs, request IDs, event IDs, and sign-in URLs.

---

## What I Learned

This lab helped me understand how identity security connects directly to SOC, GRC, and cloud security work.

Key takeaways:

- IAM groups make access management cleaner and easier to audit.
- Least privilege helps reduce risk if a user account is misused.
- CloudTrail is important for investigating user activity.
- Denied events are still valuable security evidence.
- GRC and SOC work both depend heavily on identity, access control, and logging.
- Security is not just about blocking attacks — it is also about proving what happened.

---

## Career Relevance

This project is relevant to roles like:

- SOC Analyst
- Security Analyst
- GRC Analyst
- Cloud Security Analyst
- Junior Security Engineer

It shows hands-on experience with identity access controls, audit logging, least privilege, and security investigation workflows.

---

## Project Outcome

This lab successfully demonstrated how to design a basic AWS IAM environment, separate access by job function, enforce least privilege, and investigate denied access attempts using CloudTrail.

The project gave me practical experience with identity security, cloud audit logging, and access investigation — all skills that are valuable in real-world security roles.