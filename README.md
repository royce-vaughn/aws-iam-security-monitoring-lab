# AWS IAM Security Monitoring Project

## Overview

This project is an AWS IAM and CloudTrail lab I built to practice identity security, least privilege, and access investigation.

I wanted to create different AWS users, give them different levels of access, test what each user could and could not do, then use CloudTrail to see what got logged.

This connects directly to SOC, GRC, and cloud security because a lot of security issues come back to identity. Who has access? What can they do? What happens when someone tries to do something they should not be able to do?

That is what this lab was built to test.

## Tools I Used

| Tool or Service          | How I Used It                            |
| ------------------------ | ---------------------------------------- |
| AWS IAM                  | Created users, groups, and permissions   |
| AWS CloudTrail           | Reviewed user activity and denied events |
| IAM User Groups          | Separated access by job role             |
| AWS Managed Policies     | Assigned common access levels            |
| Custom IAM Policy        | Created limited developer access         |
| CloudTrail Event History | Investigated AccessDenied events         |

## What I Built

I created a small AWS identity setup with different groups for different types of users.

| Group              | Purpose                                  |
| ------------------ | ---------------------------------------- |
| `AdminBreakGlass`  | Admin access for setup and emergency use |
| `SecurityAnalysts` | Security review and investigation access |
| `ReadOnlyAuditors` | Auditor style view only access           |
| `Developers`       | Limited developer access                 |

I also created test users for each role.

| User               | Group              |
| ------------------ | ------------------ |
| `admin-royce`      | `AdminBreakGlass`  |
| `soc-analyst-test` | `SecurityAnalysts` |
| `grc-auditor-test` | `ReadOnlyAuditors` |
| `dev-user-test`    | `Developers`       |

Instead of giving permissions directly to each user, I assigned permissions through groups. This made the setup cleaner and closer to how access is usually managed in a real environment.

## Security Controls I Set Up

| Control                 | What I Did                                          |
| ----------------------- | --------------------------------------------------- |
| Root account protection | Enabled MFA on the root account                     |
| Admin protection        | Enabled MFA on the admin IAM user                   |
| Role based access       | Created separate IAM groups for each job role       |
| Least privilege         | Limited users to only the access they needed        |
| Developer restrictions  | Built a custom policy for the developer group       |
| Logging                 | Enabled CloudTrail to record account activity       |
| Investigation           | Reviewed denied actions in CloudTrail Event History |

The main security idea behind the lab was this:

> Give users only the access they need. Nothing extra.

## Custom Developer Policy

The `Developers` group was given a custom policy called `DeveloperLimitedReadOnly`.

I wanted the developer user to have basic visibility into a few services, but I did not want that user touching sensitive security areas.

The developer user was blocked from IAM admin actions, CloudTrail changes, GuardDuty access, account changes, and organization changes.

This helped prove that least privilege was working. It also showed why logging and identity controls need to be protected from users who do not need access to them.

## Test Scenarios

I tested the setup by signing in as different users and trying actions based on their role.

| Test                                                | User               | Result       |
| --------------------------------------------------- | ------------------ | ------------ |
| Developer tried to access IAM information           | `dev-user-test`    | AccessDenied |
| Developer tried to access CloudTrail resources      | `dev-user-test`    | AccessDenied |
| Auditor viewed the IAM dashboard                    | `grc-auditor-test` | Allowed      |
| Auditor tried to create an IAM group                | `grc-auditor-test` | AccessDenied |
| SOC analyst viewed security related IAM information | `soc-analyst-test` | Allowed      |
| SOC analyst tried to create an IAM group            | `soc-analyst-test` | AccessDenied |

## CloudTrail Investigation

After testing the denied actions, I went into CloudTrail Event History to review what happened.

CloudTrail showed the user, the action they attempted, the AWS service involved, the time of the event, and the error code.

One example was `dev-user-test` trying to access IAM account summary information. The request was denied with `AccessDenied`.

That confirmed two things.

First, the permissions were working.

Second, CloudTrail captured the activity so it could be reviewed later.

That is the part I wanted to practice from a SOC point of view. It is not just about blocking access. It is also about being able to prove what happened.

## Evidence Collected

I collected screenshots and CloudTrail event records for the main parts of the lab.

| Evidence                           | Purpose                                       |
| ---------------------------------- | --------------------------------------------- |
| IAM users                          | Shows the test users created for the lab      |
| IAM groups                         | Shows the role based access structure         |
| CloudTrail trail status            | Shows that logging was enabled                |
| Developer denied IAM access        | Shows least privilege working                 |
| Developer denied CloudTrail access | Shows logging resources were protected        |
| Auditor view only access           | Shows the auditor could review but not change |
| Auditor denied group creation      | Shows the auditor could not modify IAM        |
| SOC analyst view access            | Shows the analyst had security visibility     |
| SOC analyst denied group creation  | Shows the analyst did not have admin rights   |

Before publishing, I redacted sensitive information like account IDs, ARNs, source IP addresses, access key IDs, request IDs, event IDs, and sign in URLs.

## What I Learned

This lab helped me understand how important identity is in cloud security.

IAM groups made the access easier to manage. Least privilege helped control what each user could do. CloudTrail gave me a way to investigate activity after the fact.

The biggest takeaway for me was that denied events still matter. Even when an action fails, it can still tell a security team a lot about what a user attempted to do.

This lab also helped me see how SOC and GRC overlap. SOC cares about investigating activity. GRC cares about access control, evidence, and proving that controls are working. This project touched both sides.

## Career Relevance

This project is relevant to the kind of roles I am working toward.

| Role                     | Why This Project Helps                           |
| ------------------------ | ------------------------------------------------ |
| SOC Analyst              | Shows log review and investigation practice      |
| Security Analyst         | Shows identity and access control knowledge      |
| GRC Analyst              | Shows control mapping and access review evidence |
| Cloud Security Analyst   | Shows AWS IAM and CloudTrail experience          |
| Junior Security Engineer | Shows hands on security configuration work       |

## Project Outcome

This lab gave me hands on practice with AWS IAM, CloudTrail, least privilege, and access investigation.

I built the users and groups, tested the permissions, reviewed the CloudTrail logs, and documented the results.

Overall, this project helped me practice how identity security works in AWS and how denied access events can be used as investigation evidence.
