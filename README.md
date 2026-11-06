# SCIM: An Azure Identity Learning Journey

Welcome to this beginner-friendly collection of notes on **Microsoft Azure, Microsoft Entra ID, authentication, identity security, access management, and hybrid identity troubleshooting**.

The lessons begin with cloud fundamentals and gradually move into multifactor authentication, Conditional Access, device registration, passwordless sign-in, and Kerberos. The later notes expand into Global Secure Access, AD FS federation and claims, Web Application Proxy, certificates, delegation, DNS, and browser authentication troubleshooting. Weekly assessments and practical administration guides reinforce the daily lessons.

**Last updated:** 2 November 2026  
**Current coverage:** 26 daily lesson files, from 14 September to 29 October 2026, plus three weekly assessments and three supplementary guides. The latest supplementary material is dated 30 October 2026.

> **How to use this guide:** Follow the lessons in day order if you are new to Azure identity. If you already know the fundamentals, use the topic links below to jump directly to the lesson you need.

## Contents

- [The Journey at a Glance](#the-journey-at-a-glance)
- [Daily Learning Log](#daily-learning-log)
- [Weekly Assessments](#weekly-assessments)
- [Practical Administration Guides](#practical-administration-guides)
- [Diagrams, Screenshots, and Presentation](#diagrams-screenshots-and-presentation)
- [Suggested Learning Path](#suggested-learning-path)
- [A Note About the Day Numbers](#a-note-about-the-day-numbers)

## The Journey at a Glance

| Stage | Focus | Lessons |
|---|---|---|
| 1. Build the foundation | Cloud computing, Azure, identity, governance, roles, and licensing | Days 1–4 |
| 2. Control access | IAM, PIM, MFA, Conditional Access, and identity risk | Days 5–9 |
| 3. Connect users and devices | Device identity, hybrid authentication, BitLocker, and LAPS | Days 11–14 |
| 4. Strengthen authentication | Passwordless methods, attack awareness, and Kerberos | Days 16–17 |
| 5. Secure private and internet access | Global Secure Access, Private Access, traffic profiles, and diagnostics | Days 18–20 |
| 6. Understand federation and policy | AD FS farms, protocols, claims, hybrid join, lockout, auditing, risk, and authentication strengths | Days 21–24 and 26 |
| 7. Troubleshoot hybrid authentication | AD FS certificates, WAP trust, SPNs, delegation, Kerberos, DNS, and browser WIA | Days 31–34 |

Lesson ranges describe the learning stages; only the available day files are listed below.

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

This lesson explores Kerberos, the ticket-based authentication protocol widely used in Active Directory environments. It explains the Key Distribution Center, Ticket Granting Tickets, service tickets, single sign-on, troubleshooting, and attacks such as pass-the-ticket and golden-ticket attacks.

**Key themes:** Kerberos, Key Distribution Center, Authentication Service, Ticket Granting Service, tickets, SSO, Active Directory, troubleshooting, attacks, and defenses.

---

### Day 18 — Global Secure Access and Microsoft Entra Private Access Architecture

**Date:** 7 October 2026  
**Lesson:** [Open Day 18](./DAY18_07102026.md)

Day 18 introduces Microsoft Global Secure Access (GSA), Entra Internet Access, and Entra Private Access through beginner-friendly and expanded architecture walkthroughs. It follows users, devices, identity checks, and application traffic across trust boundaries, explains private network connectors, and connects the design to Zero Trust and security monitoring.

**Key themes:** Security Service Edge (SSE), Zero Trust Network Access (ZTNA), VPN comparisons, connectors, Conditional Access, Intune, Defender, Sentinel, tokens, packet journeys, and troubleshooting.

---

### Day 19 — Global Secure Access Setup and Identity Notes

**Date:** 8 October 2026  
**Lesson:** [Open Day 19](./DAY19_08102026.md)

This short working-note lesson records GSA setup considerations, the Microsoft 365, internet, and private-access traffic profiles, and the relationship between clients, devices, policies, and connectors. It also collects related terminology on tenant types, source of authority, device enrollment, and device-join checks.

**Key themes:** GSA profiles, setup prerequisites, named locations, licensing and roles, private network connectors, device enrollment, and `dsregcmd`.

---

### Day 20 — GSA Diagnostics, Source IP Anchoring, and Device Identifiers

**Date:** 9 October 2026  
**Lesson:** [Open Day 20](./DAY20_09102026.md)

Day 20 turns to support workflows: distinguish the endpoint client from the connector server, reproduce the issue, and collect connector, client, and browser evidence together. It also explains Source IP Anchoring, compares device IDs with directory object IDs, and examines IP mismatches in Conditional Access and sign-in logs.

**Key themes:** client health checks, advanced diagnostics, connector data collection, browser net-export, source IP anchoring versus restoration, IP allowlisting, device ID versus object ID, and sign-in investigation.

---

### Day 21 — AD FS Architecture, Authentication, and Federation Protocols

**Date:** 12 October 2026  
**Lesson:** [Open Day 21](./DAY21_12102026.md)

This lesson introduces Active Directory Federation Services (AD FS), from a basic deployment to highly available farms using Windows Internal Database (WID) or SQL Server. It covers network dependencies and authentication methods, then walks through WS-Federation and SAML requests, assertions, and browser-carried sign-in flows.

**Key themes:** federation servers, Web Application Proxy (WAP), high availability, intranet versus extranet authentication, WS-Fed, SAML, SP-initiated and IdP-initiated sign-in, `wauth`, certificates, and rollover.

---

### Day 22 — AD FS Farms, Authentication Flows, Claims, and Windows Sign-In Tools

**Date:** 13 October 2026  
**Lesson:** [Open Day 22](./DAY22_13102026.md)

Day 22 consolidates farm architecture, authentication paths, certificate roles, and the AD FS claims pipeline. It explains claims provider and relying party trusts, then adds related Windows notes on automatic device join and the `netplwiz` sign-in utility.

**Key themes:** farm configuration, authentication flows, certificates, acceptance and issuance rules, claims processing, Workplace Join, and Windows sign-in administration.

---

### Day 23 — Device Identity, Hybrid Join, AD FS Lockout, Auditing, and Claims

**Date:** 14 October 2026  
**Lesson:** [Open Day 23](./DAY23_14102026.md)

This extended lesson connects device identity states and managed hybrid join to authentication and Primary Refresh Tokens (PRTs). It then develops AD FS extranet lockout protection, auditing and debug tracing, Activity ID correlation, claim-rule language, and claims processing through detailed examples and diagrams.

**Key themes:** registered, joined, and hybrid-joined devices, hybrid-join verification, PRTs, Extranet Smart Lockout, familiar locations, audit events, correlation IDs, claims-engine execution, and `add` versus `issue`.

---

### Day 24 — Identity Protection and Conditional Access Authentication Strengths

**Date:** 15 October 2026  
**Lesson:** [Open Day 24](./DAY24_15102026.md)

Day 24 revisits Identity Protection with a deeper runtime view of risk detection, investigation, remediation, and Conditional Access decisions. It explains built-in authentication strengths and practical policy scenarios, including how federated authentication fits into the evaluation.

**Key themes:** user risk versus sign-in risk, risk detections, investigation, MFA strength, passwordless MFA, phishing-resistant MFA, policy evaluation, and federation.

---

### Day 26 — AD FS Claims, Federation Protocols, and End-to-End Troubleshooting

**Date:** 19 October 2026  
**Lesson:** [Open Day 26](./DAY26_19102026.md)

This lesson builds from claims foundations to practical Claims X-Ray testing and Microsoft Entra ID relying-party claim rules. It then traces token issuance through server logs and browser captures, keeping live sign-in traffic separate from trust management, directory dependencies, replication, and diagnostics.

**Key themes:** claims engine, trust rule sets, Claims X-Ray, RPT Rule Generator, user and device claims, Admin/debug logs, Activity IDs, Diagnostics Analyzer, DevTools, Fiddler, WS-Fed, and SAML.

---

### Day 31 — Troubleshooting and Replacing AD FS Certificates

**Date:** 26 October 2026  
**Lesson:** [Open Day 31](./DAY31_26102026.md)

Day 31 explains certificate roles and how to isolate SSL/TLS, service communication, and token-signing failures. It covers certificate replacement on AD FS and WAP, private-key access, bindings, rollover, metadata updates, and relying-party certificate mismatches.

**Key themes:** trust chains, expiry, revocation, hostname matching, private-key permissions, SSL bindings, farm-version differences, token signatures, thumbprints, and federation metadata.

---

### Day 32 — Web Application Proxy Troubleshooting

**Date:** 27 October 2026  
**Lesson:** [Open Day 32](./DAY32_27102026.md)

This lesson focuses on WAP's reverse-proxy role and its trust relationship with AD FS. It outlines collecting AD FS Admin/debug and WAP Admin/session logs, using diagnostic tools, and checking the dependencies that prevent proxy configuration or external authentication from working.

**Key themes:** proxy trust, event correlation, Diagnostics Analyzer, time skew, connectivity, certificates, private-key permissions, TLS, SPNs, proxy trust endpoints, and trust reset.

---

### Day 33 — Kerberos Principal Names, SPNs, Delegation, and Troubleshooting

**Date:** 28 October 2026  
**Lesson:** [Open Day 33](./DAY33_28102026.md)

Day 33 develops Kerberos service identity through User Principal Names (UPNs), Service Principal Names (SPNs), and service mappings. It explains delegation and the double-hop problem, then provides workflows for collecting evidence and isolating ticket, SPN, service-account, and delegation failures.

**Key themes:** UPNs, SPN ownership and duplicates, `sPNMappings`, delegation models, constrained delegation, diagnostic captures, Kerberos errors, `setspn`, and ticket inspection.

---

### Day 34 — Kerberos, AD FS DNS, and Browser Authentication Troubleshooting

**Date:** 29 October 2026  
**Lesson:** [Open Day 34](./DAY34_29102026.md)

The latest daily lesson combines Kerberos fundamentals and troubleshooting with AD FS DNS routing and browser-based Windows Integrated Authentication (WIA). It follows tickets, names, network paths, browser settings, and federation requests to isolate the failing layer rather than treating every sign-in failure as the same problem.

**Key themes:** KDC discovery, ticket flow, SPNs, delegation, time synchronization, split DNS, AD FS availability, browser zones, User-Agent handling, Negotiate, and federation request analysis.

## Weekly Assessments

Each assessment contains **20 questions** with answers and explanations. Try the questions first, then use the answer sheets and review material to reinforce the troubleshooting approach.

| Assessment | Focus |
|---|---|
| [TLA Week 1](<./TLA - Assessment - Week1.md>) | AD FS errors, Extranet Smart Lockout, WID and SQL considerations, certificate rollover, auditing, SPNs, federation, and WAP architecture |
| [TLA Week 2](<./TLA - Assessment - Week2.md>) | Trusts and claim rules, SAML signing and encryption, NameID, ImmutableID, OAuth application groups, OIDC discovery, protocol endpoints, and metadata |
| [TLA Week 3](<./TLA - Assessment - Week3.md>) | AD FS HTTPS bindings, WAP trust and publishing, browser WIA, Kerberos and constrained delegation, SPNs, DNS, and certificate validation |

## Practical Administration Guides

| Guide | What it covers |
|---|---|
| [Windows Service Accounts Without Active Directory](./SERVICE_ACCOUNTS.md) | Local, built-in, and virtual service identities; how managed accounts differ; choosing an identity; local group membership; verification; IIS examples; and least-privilege troubleshooting |
| [Miscellaneous Windows Administration Notes](./miscellaneous.md) | Service-account and local-group commands, inspecting security tokens, opening the Certification Authority console, and checking the public IP address with PowerShell |
| [Jump Servers, VPNs, and Azure Bastion](./JumpServer.md) | Controlled administrative access, VPN versus jump-server roles, enterprise access paths, Windows and Linux setup examples, Azure Bastion, firewall restrictions, and hardening |

> **Study-note caution:** Some files retain working notes, historical product names, or source-specific examples. Check the lesson's qualifications and current Microsoft guidance for your product version, licensing, and environment before running administrative commands or applying configuration changes.

## Diagrams, Screenshots, and Presentation

The [assets folder](./assets/) contains architecture illustrations, authentication flows, decision diagrams, screenshots, and a Kerberos presentation. Many are embedded directly in the lessons.

- **Foundations and access control:** visual flows for Azure resource hierarchy, authentication versus authorization, role boundaries, PIM, MFA, Conditional Access, and Identity Protection.
- **Device and hybrid identity:** diagrams for device-access decisions, hybrid authentication, LAPS, hybrid join, and PRT acquisition.
- **Global Secure Access:** beginner-friendly and enterprise Private Access architecture images accompanying Day 18.
- **AD FS federation and claims:** diagrams for browser federation, SAML flows, claims processing, extranet lockout, correlation troubleshooting, and runtime versus management paths.
- **Advanced troubleshooting:** diagrams for certificate paths, Kerberos delegation and diagnostics, AD FS DNS/browser flows, and Week 2–3 assessment concepts.
- **Presentation:** [Kerberos Authentication slide deck](./assets/Kerberos_Authentication_presentation.pptx).
- **Diagnostic screenshots:** [Enabling AD FS debug logging](./Enable_AD_FS_DEBUG.png), [filtering AD FS traces](./assets/FIlter_ADFS_Traces_Event_Viewer.png), and correlation-ID examples in the [assets folder](./assets/).

## Suggested Learning Path

1. **Learn the platform:** Begin with Days 1–4 to understand Azure, identity, governance, and permissions.
2. **Learn access control:** Continue with Days 5–9 for privileged access, MFA, Conditional Access, simulation, and risk.
3. **Learn hybrid and device identity:** Use Days 11–14 to understand device trust, hybrid sign-in, encryption, and local administrator security.
4. **Learn strong authentication:** Continue with Days 16–17 for passwordless methods, attack awareness, and Kerberos foundations.
5. **Learn secure access architecture:** Use Days 18–20 for GSA, Private Access, traffic profiles, and client/connector troubleshooting.
6. **Build federation knowledge:** Study Days 21–23 for AD FS architecture, protocols, claims, device identity, lockout, and auditing.
7. **Connect policy to investigation:** Read Day 24 for risk and authentication strengths, then Day 26 for claims testing and end-to-end federation troubleshooting.
8. **Practice specialist troubleshooting:** Work through Days 31–34 for certificates, WAP, SPNs, delegation, Kerberos, DNS, and browser WIA.
9. **Check your understanding:** Use the weekly assessments and their answer explanations to test concepts and diagnostic decisions.
10. **Apply the administration references:** Consult the service-account, miscellaneous Windows, and jump-server guides as needed, and revisit the diagrams and presentation for visual review.

## A Note About the Day Numbers

This folder currently contains notes for **Days 1–9, 11–12, 14, 16–24, 26, and 31–34**. Days **10, 13, 15, 25, and 27–30** are not included in the current collection mostly because they were included inside of preceding or future sessions. The numbering follows the existing files rather than implying that missing lessons are available.

## Start Exploring

New to the subject? Start with [Day 1: Cloud Computing and Azure Fundamentals](./DAY1_14092026.md).

Already comfortable with Azure? Jump to [Day 6: Microsoft Entra ID and Multi-Factor Authentication](./DAY6_21092026.md) and continue through the identity-security lessons.

Working on hybrid authentication support? Start with [Day 21: AD FS Architecture](./DAY21_12102026.md), then use [Day 26: End-to-End Troubleshooting](./DAY26_19102026.md) and [Days 31–34](#suggested-learning-path) to follow the failing layer.
