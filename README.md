# SCIM: An Azure Identity Learning Journey

Welcome to this beginner-friendly collection of notes on **Microsoft Azure, Microsoft Entra ID, authentication, identity security, and access management**.

The lessons begin with cloud fundamentals and gradually move into practical identity topics such as multifactor authentication, Conditional Access, device registration, passwordless sign-in, and Kerberos. Each day builds vocabulary and context that make the later security concepts easier to understand.

> **How to use this guide:** Follow the lessons in day order if you are new to Azure identity. If you already know the fundamentals, use the topic links below to jump directly to the lesson you need.

## The Journey at a Glance

| Stage | Focus | Lessons |
|---|---|---|
| 1. Build the foundation | Cloud computing, Azure, identity, governance, roles, and licensing | Days 1–4 |
| 2. Control access | IAM, PIM, MFA, Conditional Access, and identity risk | Days 5–9 |
| 3. Connect users and devices | Device identity, hybrid authentication, BitLocker, and LAPS | Days 11–14 |
| 4. Strengthen authentication | Passwordless methods, attack awareness, and Kerberos | Days 16–17 |

## Daily Learning Log

### Day 1 — Cloud Computing and Azure Fundamentals

**Date:** 14 September 2026  
**Lesson:** [Open Day 1](./DAY1_14092026.md)

The journey starts with the language of the cloud. This lesson explains cloud computing, pay-as-you-go pricing, deployment models, Azure's global infrastructure, availability options, and the Azure resource hierarchy. It provides the foundation needed to understand where identity and security services operate.

**Key themes:** cloud services, cost models, public and hybrid cloud, regions, availability, resource groups, and Azure Resource Manager.

---

### Day 2 — Identity, Security, Governance, and Compliance

**Date:** 15 September 2026  
**Lesson:** [Open Day 2](./DAY2_15092026.md)

This lesson connects identity to the wider Azure security picture. It explains the difference between authentication and authorization, introduces Microsoft Entra ID and MFA, and shows how monitoring, governance, compliance, and licensing help protect an organization.

**Key themes:** authentication, authorization, least privilege, MFA, monitoring, Azure Policy, compliance, privacy, and Microsoft 365 licensing.

---

### Day 3 — Microsoft Entra Groups, Roles, and Domain Protection

**Date:** 16 September 2026  
**Lesson:** [Open Day 3](./DAY3_16092026.md)

Day 3 moves into identity administration. It covers hybrid-tenant checks, Microsoft Entra groups, membership types, administrative units, and the important difference between Microsoft Entra roles and Azure role-based access control (RBAC) roles. It also introduces protections for domains and email.

**Key themes:** hybrid identity, groups, dynamic membership, Entra roles, Azure RBAC, privileged access, and domain protection.

---

### Day 4 — Azure Administration and Identity Security

**Date:** 17 September 2026  
**Lesson:** [Open Day 4](./DAY4_17092026.md)

This lesson explores the relationship between administrator roles, subscriptions, licenses, and security features. It also explains Security Defaults, Conditional Access, migration planning, user attributes, and why having a powerful role does not automatically grant every specialized permission.

**Key themes:** administrative roles, Microsoft 365 subscriptions, Azure licensing, Security Defaults, Conditional Access, and identity configuration.

---

### Day 5 — IAM and Privileged Identity Management

**Date:** 18 September 2026  
**Lesson:** [Open Day 5](./DAY5_18092026.md)

Day 5 compares broad **Identity and Access Management (IAM)** with **Privileged Identity Management (PIM)**. It explains how PIM reduces permanent administrator access by allowing eligible users to activate privileged roles only when needed.

**Key themes:** IAM, PIM, just-in-time access, role activation, approval, MFA, auditing, and reduced standing privilege.

---

### Day 6 — Microsoft Entra ID and Multi-Factor Authentication

**Date:** 21 September 2026  
**Lesson:** [Open Day 6](./DAY6_21092026.md)

This lesson takes a deeper look at Microsoft Entra ID and the identity objects it manages. It then examines multifactor authentication methods, tokens, password protection, registration campaigns, authentication strengths, and legacy per-user MFA settings.

**Key themes:** tenants, users, groups, applications, devices, authentication tokens, MFA methods, password protection, and authentication strengths.

---

### Day 7 — Microsoft Entra Conditional Access

**Date:** 22 September 2026  
**Lesson:** [Open Day 7](./DAY7_22092026.md)

Day 7 introduces Conditional Access as Microsoft's identity-driven policy engine. It explains how sign-in signals are evaluated and how policies can allow access, block access, or require extra controls. The lesson also covers report-only testing, locations, device filters, session controls, and practical policy scenarios.

**Key themes:** sign-in signals, policy assignments, grant controls, report-only mode, device filters, workload identities, Terms of Use, and session controls.

---

### Day 8 — Using the Conditional Access What If Tool

**Date:** 23 September 2026  
**Lesson:** [Open Day 8](./DAY8_23092026.md)

This practical lesson explores the Conditional Access **What If** tool. The simulator predicts which policies would apply to a sign-in based on details such as the user, target resource, device, location, client application, authentication flow, and risk level.

**Key themes:** policy simulation, identities, target resources, sign-in conditions, risk, locations, device filters, and result interpretation.

---

### Day 9 — Microsoft Entra ID Protection

**Date:** 24 September 2026  
**Lesson:** [Open Day 9](./DAY9_24092026.md)

Day 9 explains how Microsoft Entra ID Protection detects, investigates, and helps remediate identity risk. It distinguishes risk detection from Conditional Access enforcement and introduces the reports used to investigate risky users and sign-ins.

**Key themes:** identity risk, risky users, risky sign-ins, risk detections, machine-learning signals, investigation, and remediation.

---

### Day 11 — Microsoft Entra Device Identity and Registration

**Date:** 28 September 2026  
**Lesson:** [Open Day 11](./DAY11_28092026.md)

This lesson adds devices to the identity story. It compares Microsoft Entra registered, Microsoft Entra joined, and hybrid Microsoft Entra joined devices. It also explains how device identity affects Conditional Access and how to use `dsregcmd` to inspect and troubleshoot registration.

**Key themes:** device identity, registration states, Primary Refresh Tokens, Conditional Access, `dsregcmd`, diagnostics, and recovery.

---

### Day 12 — Seamless Single Sign-On and Hybrid Authentication

**Date:** 29 September 2026  
**Lesson:** [Open Day 12](./DAY12_29092026.md)

Day 12 connects an on-premises Active Directory environment to Microsoft Entra ID. It introduces Seamless Single Sign-On and compares Password Hash Synchronization with Pass-Through Authentication so that beginners can understand where and how password validation occurs.

**Key themes:** hybrid identity, Seamless SSO, Microsoft Entra Connect, Password Hash Synchronization, Pass-Through Authentication, and sign-in flow.

---

### Day 14 — BitLocker and Windows LAPS

**Date:** 1 October 2026  
**Lesson:** [Open Day 14](./DAY14_01102026.md)

This lesson covers two complementary Windows security controls. BitLocker protects the data stored on a drive through encryption, while Windows LAPS protects local administrator accounts with unique, automatically rotated passwords.

**Key themes:** drive encryption, recovery keys, Trusted Platform Module (TPM), local administrator passwords, password rotation, Microsoft Entra ID, and Intune.

---

### Day 16 — Strong Authentication, Passwordless Sign-In, SSO, and Identity Attacks

**Date:** 5 October 2026  
**Lesson:** [Open Day 16](./DAY16_05102026.md)

Day 16 brings authentication methods and attacker techniques together. It introduces strong authentication factors, passwordless technologies, Seamless SSO, and a broad range of identity attacks. The goal is to understand both how modern sign-in works and how it may be targeted.

**Key themes:** FIDO2, passkeys, Windows Hello for Business, certificate-based authentication, Temporary Access Pass, SSO, phishing, MFA attacks, and privilege escalation.

---

### Day 17 — Kerberos Authentication

**Date:** 6 October 2026  
**Lesson:** [Open Day 17](./DAY17_06102026.md)

The final available lesson explores Kerberos, the ticket-based authentication protocol widely used in Active Directory environments. It explains the Key Distribution Center, Ticket Granting Tickets, service tickets, single sign-on, troubleshooting, and attacks such as pass-the-ticket and golden-ticket attacks.

**Key themes:** Kerberos, Key Distribution Center, Authentication Service, Ticket Granting Service, tickets, SSO, Active Directory, troubleshooting, attacks, and defenses.

## Suggested Learning Path

1. **Learn the platform:** Begin with Days 1–4 to understand Azure, identity, governance, and permissions.
2. **Learn access control:** Continue with Days 5–9 for privileged access, MFA, Conditional Access, simulation, and risk.
3. **Learn hybrid and device identity:** Use Days 11–14 to understand device trust, hybrid sign-in, encryption, and local administrator security.
4. **Learn advanced authentication:** Finish with Days 16–17 for passwordless methods, attack awareness, and Kerberos.
5. **Review the diagrams:** Each lesson includes visual flows where architecture or decision-making benefits from a picture.

## A Note About the Day Numbers

This folder currently contains notes for Days 1–9, 11–12, 14, and 16–17. Days 10, 13, and 15 are not included in the current collection, so the links above intentionally follow the available lesson files.

## Start Exploring

New to the subject? Start with [Day 1: Cloud Computing and Azure Fundamentals](./DAY1_14092026.md).

Already comfortable with Azure? Jump to [Day 6: Microsoft Entra ID and Multi-Factor Authentication](./DAY6_21092026.md) and continue through the identity-security lessons.

