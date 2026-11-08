# Week 4 Assessment: AD FS Questions 1–20

This beginner-friendly assessment guide contains all 20 questions, the correct answer, explanations, reasons the other options are incorrect, exam tips, commands, URLs, caveats, and source observations.

## How to use this assessment

1. Read the prompt and choose an answer before opening the explanation.
2. Compare your reasoning with the **Correct answer** and **Explanation** sections.
3. Use **Why the other answers are incorrect** to learn the boundaries between AD FS, Web Application Proxy (WAP), Microsoft Entra, DNS, certificates, and network controls.
4. Treat commands and version-specific behavior as study material: verify them in documentation for the Windows Server and AD FS version you operate before making production changes.

## Learning outcomes

By the end, you should be able to:

- distinguish authentication policy decisions from proxying, DNS, certificates, and network controls;
- troubleshoot MFA, federation metadata, certificates, logs, correlation IDs, and load-balanced AD FS farms;
- separate browser-carried federation traffic from server-to-server traffic; and
- separate runtime request paths from configuration, management, WID replication, and monitoring paths.

## Architecture guide

![AD FS browser, server-to-server, configuration, replication, and monitoring paths](assets/week4-assessment-adfs-traffic-and-operations.svg)

> [!IMPORTANT]
> The diagram deliberately separates four concerns. Browser-carried redirects and tokens are not the same as AD FS-to-AD DS or AD FS-to-Entra service calls. Likewise, administration, WID replication, health probes, telemetry, and log review are operational paths—not end-user federation runtime traffic.

## Question 1

**Prompt:** After enabling MFA for extranet requests, internal users still see the MFA prompt. What AD FS component decides when MFA is triggered?

### Correct answer

> **Answer:** ✅ The AD FS authentication policy and its extranet or group conditions

### Explanation

In Active Directory Federation Services (AD FS), the decision to require Multi-Factor Authentication (MFA) is controlled by the Authentication Policies configured on the AD FS farm.

These policies can specify conditions such as:

- Whether the user is connecting from the Extranet (external network) or Intranet (internal network).
- Membership in specific Active Directory groups.
- Accessing particular Relying Party Trusts (applications).
- Device authentication and access control policies.

If internal users are unexpectedly receiving MFA prompts after MFA was enabled for extranet access, the administrator should review the configured AD FS Authentication Policy and any associated Access Control Policies because these determine when MFA is enforced.

### Why other answers are incorrect

#### Incorrect choice: The cache on each of the Web Application Proxy servers

Web Application Proxy (WAP) servers publish AD FS externally and cache some data.

WAP does not decide whether MFA is required.

The MFA requirement is evaluated by the AD FS service itself based on policy.

#### Incorrect choice: The token-signing certificate that the farm presents to apps

Token-signing certificates are used to digitally sign security tokens issued by AD FS.

They ensure trust between AD FS and applications.

They have no role in determining MFA requirements.

#### Incorrect choice: The DNS A record of the federation service that internal clients use

DNS records simply direct clients to the appropriate AD FS endpoint.

DNS has no authentication logic and cannot determine whether MFA should be triggered.

### Exam tip

**When you see questions asking "What decides when MFA is required?" in AD FS, think:**

AD FS Authentication Policies → Access Control Policies → Extranet/Intranet Conditions → Group Membership

Those components make the MFA decision, not DNS, certificates, or WAP caches.

### Final answer

> **Answer:** ✅ The AD FS authentication policy and its extranet or group conditions.

---

## Question 2

**Prompt:** The MFA adapter works internally but not via WAP. What should be checked?

### Correct answer

> **Answer:** ✅ That the policy covers extranet and WAP passes the needed headers

### Explanation

When an MFA adapter functions correctly for internal AD FS authentication but fails when users access AD FS through the Web Application Proxy (WAP), the issue is typically related to how extranet authentication policies are configured or how WAP communicates the client context to AD FS.

WAP sits between external clients and the AD FS servers and forwards authentication requests. AD FS relies on information provided by WAP to determine whether the request is coming from the extranet and whether MFA should be enforced.

Therefore, administrators should verify:

- The AD FS authentication policy includes Extranet users.
- MFA is enabled for the appropriate relying parties and access control policies.
- WAP is properly configured and successfully forwarding the required request information to AD FS.
- AD FS recognizes the request as coming from an external location.

### Why other answers are incorrect

#### Incorrect choice: The B2C custom policy that is attached to the application in use

Azure AD B2C custom policies are unrelated to traditional AD FS and WAP MFA processing.

AD FS MFA adapters do not depend on Azure AD B2C custom policies.

#### Incorrect choice: The managed domain subnet and the NSG rules that are applied to it

Network Security Groups (NSGs) are Azure networking controls.

While connectivity problems can affect authentication, NSG rules do not determine whether an AD FS MFA adapter works through WAP.

#### Incorrect choice: The cross-tenant access settings configured for the partner tenant

Cross-tenant access settings apply to Microsoft Entra ID (Azure AD) B2B scenarios.

They have no direct relationship to AD FS MFA adapters operating through WAP.

### Exam tip

**For AD FS questions involving:**

- External users
- Web Application Proxy (WAP)
- MFA works internally but not externally

**Think about:**

- Extranet authentication policies
- WAP-to-AD FS communication
- Access Control Policies
- MFA adapter configuration on AD FS servers

These are the most common causes of external MFA failures.

### Final answer

> **Answer:** ✅ That the policy covers extranet and WAP passes the needed headers.

---

## Question 3

**Prompt:** The load balancer hits both farm nodes, but one node is missing the latest relying party trust. What is wrong?

### Correct answer

> **Answer:** ✅ The secondary node isn't syncing from the primary WID node

### Explanation

In an AD FS farm using Windows Internal Database (WID), only the primary federation server is writable. All other federation servers are secondary nodes and receive their configuration through replication from the primary server.

When a new Relying Party Trust (RPT) is created or modified, the change is stored on the primary node and automatically replicated to the secondary nodes.

If:

- The load balancer can reach both AD FS servers,
- One server has the new relying party trust,
- Another server does not,

then the most likely cause is that the secondary node has stopped receiving configuration updates from the primary WID node.

Common causes include:

- AD FS service issues
- Replication failures
- Network connectivity problems between federation servers
- A server that has fallen out of synchronization

### Why other answers are incorrect

#### Incorrect choice: The Web Application Proxy servers are all offline

WAP servers only proxy external requests to AD FS.

They do not store or replicate relying party trusts.

Even if all WAP servers were offline, the federation servers would still contain the same configuration.

#### Incorrect choice: The DNS records for the farm are pointing to the wrong IP address

DNS affects how clients reach the AD FS farm.

DNS does not control AD FS configuration replication.

Incorrect DNS might cause connectivity issues, but it would not explain why one node lacks a recently created relying party trust.

#### Incorrect choice: The federation metadata has expired and needs renewal

Federation metadata helps applications learn about certificates, endpoints, and trust relationships.

Expired metadata can cause federation trust problems with applications.

It does not prevent replication of relying party trusts between AD FS farm nodes.

### Exam tip

**For AD FS farms using WID:**

- **Primary Server** = Read/Write
- **Secondary Servers** = Read-Only replicas

**Replication direction:** Relying Party Trusts, Claim Rules, Certificates, and Policies replicate from primary to secondary servers.

**When one AD FS node shows different configuration than another, immediately think:**

"WID replication/synchronization problem."

Useful command for troubleshooting:

```powershell
Get-AdfsSyncProperties
```

This shows whether the server is Primary or Secondary and its synchronization status.

### Final answer

> **Answer:** ✅ The secondary node isn't syncing from the primary WID node.

---

## Question 4

**Prompt:** The team needs to collect AD FS debug traces for a failure. Which is the correct approach?

### Correct answer

> **Answer:** ✅ Enable AD FS tracing in Event Viewer, reproduce it and review

### Explanation

When troubleshooting complex AD FS authentication, claims, federation, MFA, or token issuance failures, Microsoft recommends enabling AD FS Debug Tracing and then reproducing the issue.

AD FS provides detailed tracing logs that capture:

- Authentication flow processing
- Claims rule execution
- Token issuance steps
- MFA processing
- Relying party trust interactions
- Error details not visible in standard Admin logs

The typical process is:

1. Enable AD FS Debug logging in Event Viewer.
2. Reproduce the issue.
3. Collect and review the generated debug events.
4. Correlate events using the Activity ID when available.

Relevant logs are found under:

```text
Applications and Services Logs
└─ AD FS
├─ Admin
├─ Tracing/Debug
└─ Security
```

This is the standard Microsoft troubleshooting approach for deep AD FS investigations.

### Why other answers are incorrect

#### Incorrect choice: Collect the NSG flow logs for the subnet hosting the AD FS servers

NSG Flow Logs are an Azure networking feature.

They show whether traffic is allowed or denied.

They do not provide AD FS authentication, claims, or token processing details.

Useful for network troubleshooting, not AD FS debugging.

#### Incorrect choice: Use the Activity blade in the Azure portal for the tenant

Azure Activity Logs track Azure resource management operations.

They do not capture AD FS on-premises tracing information.

AD FS debug analysis requires AD FS logs, not Azure portal activity logs.

#### Incorrect choice: Enable NTLM auditing on the AD DS domain controllers first

NTLM auditing helps identify NTLM authentication usage.

It may assist with domain authentication investigations.

It does not produce AD FS debug traces and is not the first step for collecting AD FS diagnostic information.

### Exam tip

For AD FS troubleshooting questions:

| Problem | First place to look |
| --- | --- |
| Authentication failure | AD FS Admin Log |
| Claim issuance issue | AD FS Debug Trace |
| Token issuance problem | AD FS Debug Trace |
| MFA issue | AD FS Admin & Debug Logs |
| Correlation of events | Activity ID |

A common interview question is:

Which log is enabled by default after AD FS installation?

✅ AD FS Admin Log

And when deeper troubleshooting is required:

✅ Enable AD FS Debug/Tracing logs and reproduce the issue.

### Final answer

> **Answer:** ✅ Enable AD FS tracing in Event Viewer, reproduce it and review.

---

## Question 5

**Prompt:** A user is prompted twice for MFA after federation. Entra Conditional Access also requires MFA. What helps?

> [!NOTE]
> The assessment preserves `federatedIdpMfaBehavior = acceptIfMfaDoneByFederatedIdp` exactly as supplied. It expresses the intended setting/value; treat it as a configuration distinction rather than a complete executable command, and verify the current Microsoft Graph or domain-federation procedure before applying it.

### Correct answer

> **Answer:** ✅ Set federatedIdpMfaBehavior to acceptIfMfaDoneByFederatedIdp

### Explanation

This is a classic AD FS + Microsoft Entra ID (Azure AD) federation scenario.

The user receives:

- An MFA challenge from AD FS during federated authentication.
- A second MFA challenge from Microsoft Entra Conditional Access.

This occurs because Microsoft Entra ID does not automatically trust that MFA was already completed at the federated identity provider unless configured to do so.

The setting:

```powershell
federatedIdpMfaBehavior = acceptIfMfaDoneByFederatedIdp
```

tells Microsoft Entra ID to:

- Accept MFA that was successfully performed by the federated identity provider (AD FS).

As a result:

- User completes MFA once at AD FS.
- Entra ID recognizes the MFA claim.
- Conditional Access MFA requirements are satisfied.
- The second MFA prompt is avoided.

This improves the user experience while maintaining security.

### Why other answers are incorrect

#### Incorrect choice: Remove the user from the domain and add them back later

This has no relationship to federated MFA processing.

Removing and rejoining a user would not resolve double-MFA prompts.

#### Incorrect choice: Disable AD FS MFA so that no second factor is required at all

This weakens security.

The problem is not that MFA exists twice, but that Entra ID is not recognizing the MFA completed at AD FS.

Disabling MFA is not the recommended solution.

#### Incorrect choice: Change the DNS TTL on the federation service name record

DNS TTL controls how long DNS responses are cached.

It has no impact on MFA claims, federation trust, or Conditional Access behavior.

### Exam tip

**When you see:**

- AD FS Federation
- Microsoft Entra Conditional Access
- Users are getting prompted for MFA twice

**Immediately think:**

- MFA Claim Trust Between AD FS and Entra ID

The key setting is:

```powershell
acceptIfMfaDoneByFederatedIdp
```

which allows Entra ID to honor MFA already performed by the federated identity provider.

### Final answer

> **Answer:** ✅ Set federatedIdpMfaBehavior to acceptIfMfaDoneByFederatedIdp.

---

## Question 6

**Prompt:** An organization wants Entra MFA for AD FS sign-ins on Windows Server 2019. Which approach is used?

> [!CAUTION]
> `Set-AdfsAzureMfaTenant` is preserved as the source command reference, not presented as a complete production runbook. Tenant registration prerequisites and surrounding commands can vary; confirm current Microsoft guidance for the installed AD FS version.

### Correct answer

> **Answer:** ✅ Configure the Entra MFA adapter in AD FS (Set-AdfsAzureMfaTenant)

### Explanation

Starting with AD FS on Windows Server 2016 and later, Microsoft supports using Microsoft Entra MFA (formerly Azure MFA) as an authentication method directly within AD FS.

To integrate Entra MFA with AD FS on Windows Server 2019, administrators configure the built-in Azure MFA Adapter for the AD FS farm. One of the key configuration steps involves associating the AD FS farm with the Microsoft Entra tenant using:

```powershell
Set-AdfsAzureMfaTenant
```

Once configured:

- AD FS can use Entra MFA as a secondary authentication method.
- MFA can be triggered through AD FS Access Control Policies.
- Users receive Microsoft Authenticator, SMS, voice call, or other configured Entra MFA challenges.
- MFA can be enforced for intranet, extranet, groups, or specific applications.

This is Microsoft's supported method for integrating Entra MFA with AD FS.

### Why other answers are incorrect

#### Incorrect choice: Install the NPS extension on every Web Application Proxy server in the farm

The NPS Extension for Azure MFA is used with RADIUS-based applications, VPNs, RD Gateway, and NPS servers.

It is not used to integrate MFA directly with AD FS.

Web Application Proxy servers do not require the NPS extension for AD FS MFA.

#### Incorrect choice: Use LDAPS on the domain controllers as the second factor

LDAPS provides secure communication with Active Directory.

It is not a Multi-Factor Authentication method.

LDAPS cannot act as a second factor.

#### Incorrect choice: Deploy a custom claims provider trust for the second factor

Claims Provider Trusts are used to accept identities from external identity providers.

They are not the standard approach for enabling Entra MFA in AD FS.

Microsoft provides a native MFA adapter specifically for this purpose.

### Exam tip

Remember the common AD FS MFA integrations:

| Scenario | Solution |
| --- | --- |
| VPN / RD Gateway / RADIUS device | NPS Extension for Entra MFA |
| AD FS on Windows Server 2016/2019/2022 | Azure MFA Adapter |
| Conditional Access in Entra ID | Entra MFA |
| Third-party MFA product | MFA Adapter Framework |

A useful rule:

If the question specifically mentions AD FS on Windows Server 2019, think Azure/Entra MFA Adapter, not NPS.

### Final answer

> **Answer:** ✅ Configure the Entra MFA adapter in AD FS (Set-AdfsAzureMfaTenant).

---

## Question 7

**Prompt:** Partners cannot retrieve federation metadata externally but internal access works. What is the likely issue?

### Correct answer

> **Answer:** ✅ The metadata isn't published via WAP, or a firewall blocks it

### Explanation

In an AD FS deployment, federation metadata is typically available through the following URL:

```text
https://fs.contoso.com/FederationMetadata/2007-06/FederationMetadata.xml
```

Partners and federated applications use this metadata to obtain information such as:

- Federation service endpoints
- Token-signing certificates
- Token-decrypting certificates
- Supported protocols and claims

If:

- Internal users can successfully access the metadata URL,
- External partners cannot access it,

then the AD FS service itself is likely functioning correctly. The problem is usually in the path between the internet and the AD FS servers, most commonly:

- The Web Application Proxy (WAP) is not properly publishing the federation service.
- A firewall, reverse proxy, or network security device is blocking access.
- HTTPS (TCP 443) is not allowed externally.
- The federation metadata endpoint is not reachable through the published service.

Since internal access works, the issue is almost certainly with the external publishing layer rather than AD FS itself.

### Why other answers are incorrect

#### Incorrect choice: The SQL farm database is full and no longer accepts new writes

A full SQL database can cause AD FS operational issues.

However, it would likely affect both internal and external users.

It would not specifically prevent only external metadata retrieval.

#### Incorrect choice: Extranet smart lockout is running in log-only audit mode

Extranet Smart Lockout protects against password spray and brute-force attacks.

It affects authentication behavior.

It does not control access to the federation metadata endpoint.

#### Incorrect choice: The IdP-initiated sign-on page was switched on last week

The IdP-initiated sign-on page is a separate feature.

Federation metadata retrieval does not depend on that page being enabled or disabled.

Partners obtain metadata directly from the metadata URL.

### Exam tip

**When you see:**

- Internal works
- External fails
- Web Application Proxy (WAP) involved

**Think:**

- Publishing, firewall, DNS, SSL certificates, or external connectivity

**When the issue specifically mentions FederationMetadata.xml, the first things to check are:**

- WAP publishing configuration
- HTTPS connectivity (443)
- Firewall rules
- External DNS resolution
- SSL certificate binding

### Final answer

> **Answer:** ✅ The metadata isn't published via WAP, or a firewall blocks it.

---

## Question 8

**Prompt:** Users hit /adfs/ls/idpinitiatedsignon and see an error that no handler exists. Why?

> [!IMPORTANT]
> **Version-sensitive distinction:** the source specifically describes AD FS 2016 and later, where the IdP-initiated sign-on page is disabled by default. Do not generalize that default to every older AD FS release.

### Correct answer

> **Answer:** ✅ IdP-initiated sign-on is disabled by default since AD FS 2016

### Explanation

Beginning with AD FS 2016, Microsoft disabled the IdP-Initiated Sign-On Page by default for security reasons.

When users browse to:

```text
https://<federation-service>/adfs/ls/idpinitiatedsignon
```

they may receive an error similar to:

```text
The page cannot be displayed because an internal server error has occurred.
```

There is no registered protocol handler on path /adfs/ls/idpinitiatedsignon.

This behavior is expected when the IdP-Initiated Sign-On page has not been explicitly enabled.

To check the current setting:

```powershell
Get-AdfsProperties | Select EnableIdpInitiatedSignonPage
```

To enable it:

```powershell
Set-AdfsProperties -EnableIdPInitiatedSignonPage $true
```

After enabling, users can again access the sign-on page through the URL.

### Why other answers are incorrect

#### Incorrect choice: The token-signing certificate on the farm has already expired

An expired token-signing certificate can cause federation and token issuance failures.

It does not produce the specific "no handler exists" error for the IdP-initiated sign-on endpoint.

The endpoint would still exist.

#### Incorrect choice: The farm uses the Windows Internal Database for its config

Using WID versus SQL has no effect on whether the IdP-Initiated Sign-On page exists.

Both deployment models support the page.

#### Incorrect choice: DNS isn't configured for the federation service name yet

A DNS problem would typically result in:

- Name resolution failures
- Connection failures
- Certificate errors

It would not generate the specific AD FS message indicating that no handler exists for the endpoint.

### Exam tip

A very common AD FS interview and certification question is:

What changed regarding the IdP-Initiated Sign-On page in AD FS 2016 and later?

✅ It is disabled by default.

**Useful commands:**

```powershell
Get-AdfsProperties | Select EnableIdpInitiatedSignonPage
Set-AdfsProperties -EnableIdPInitiatedSignonPage $true
```

If you see "no handler exists", immediately think:

"The IdP-Initiated Sign-On page is disabled."

### Final answer

> **Answer:** ✅ IdP-initiated sign-on is disabled by default since AD FS 2016.

---

## Question 9

**Prompt:** Which log should be reviewed first for MFA adapter errors in AD FS?

### Correct answer

> **Answer:** ✅ The AD FS Admin log, including the MFA adapter events

### Explanation

When troubleshooting MFA adapter issues in AD FS, the first and most important place to look is the AD FS Admin log.

Location in Event Viewer:

```text
Applications and Services Logs
└─ AD FS
└─ Admin
```

The AD FS Admin log records key authentication events, including:

- MFA adapter initialization failures
- MFA challenge failures
- Adapter configuration errors
- Authentication workflow failures
- Claims issuance problems
- Communication issues with Azure/Entra MFA adapters or third-party MFA providers

In many cases, the Admin log contains enough information to identify the root cause without immediately enabling Debug Tracing.

For deeper analysis, you may later enable:

```text
AD FS Debug/Tracing Logs
```

but the Admin log is always the first place to start.

### Why other answers are incorrect

#### Incorrect choice: The DNS server log for the federation service zone

DNS logs help troubleshoot name resolution issues.

They do not provide information about MFA adapter processing or authentication failures.

An MFA adapter can fail even when DNS is working perfectly.

#### Incorrect choice: The Windows Update log on each of the AD FS servers in the farm

Windows Update logs track update installation activities.

They are not useful for diagnosing MFA adapter authentication issues.

Only relevant if a recent update is suspected to have introduced a problem.

#### Incorrect choice: The IIS access log on the Web Application Proxy

IIS logs can help troubleshoot HTTP/S connectivity and publishing issues.

They show requests reaching WAP.

They do not provide detailed MFA adapter processing information inside AD FS.

### Exam tip

A common AD FS troubleshooting sequence is:

| Problem | First log to check |
| --- | --- |
| MFA failures | AD FS Admin Log |
| Claims rule issues | AD FS Admin Log |
| Token issuance failures | AD FS Admin Log |
| Deep authentication analysis | AD FS Debug Tracing |
| Network connectivity issues | IIS/WAP or Firewall Logs |

Remember:

AD FS Admin Log is enabled by default and should always be your first stop when troubleshooting AD FS authentication and MFA issues.

### Final answer

> **Answer:** ✅ The AD FS Admin log, including the MFA adapter events.

---

## Question 10

**Prompt:** Metadata monitoring shows the partner didn't pick up a new signing cert. What is the correct communication?

### Correct answer

> **Answer:** ✅ Give them the metadata URL, or send the new cert before promotion

### Explanation

In AD FS certificate rollover scenarios, partners must update their trust configuration with the new token-signing certificate before the certificate becomes active (promoted).

AD FS typically publishes both:

- The current signing certificate
- The secondary (next) signing certificate

through its federation metadata endpoint:

```text
https://<FederationServiceName>/FederationMetadata/2007-06/FederationMetadata.xml
```

Partners should either:

- Consume federation metadata automatically, allowing certificate updates to occur automatically, or
- Import the new signing certificate manually before it is promoted.

If monitoring indicates the partner has not picked up the new certificate, the correct action is to proactively communicate:

- The federation metadata URL
- The upcoming certificate rollover date
- The new token-signing certificate file if manual updates are required

This prevents authentication failures when the new certificate becomes active.

### Why other answers are incorrect

#### Incorrect choice: Reinstall AD FS and generate a fresh signing certificate

Reinstalling AD FS is unnecessary and would create additional disruption.

The issue is that the partner has not updated their trust, not that AD FS is malfunctioning.

#### Incorrect choice: Hide the new certificate from them until the old one has expired

This is the opposite of best practice.

Partners need advance notice so they can update trusts before the rollover occurs.

Waiting until expiration may cause authentication outages.

#### Incorrect choice: Change the entity ID so that the partner has to re-import it all

The federation service identifier (entity ID) is a fundamental trust setting.

Changing it would break existing federation trusts and create a much larger issue.

It is not required for certificate rollover.

### Exam tip

For AD FS certificate rollover questions, remember:

| Best practice | Reason |
| --- | --- |
| Publish federation metadata | Allows automatic trust updates |
| Notify partners before promotion | Prevents outages |
| Share the new signing certificate in advance | Supports manual trust environments |
| Monitor metadata consumption | Confirms partners have updated |

When you hear:

"Partner didn't pick up the new signing certificate"

**think:**

"Provide metadata URL and communicate the new certificate before promotion."

### Final answer

> **Answer:** ✅ Give them the metadata URL, or send the new cert before promotion.

---

## Question 11

**Prompt:** The AD FS Diagnostic Analyzer reports multiple issues. What is it designed to do?

### Correct answer

> **Answer:** ✅ Check certificates, endpoints, SPNs and flag the problems

### Explanation

The AD FS Diagnostic Analyzer is a troubleshooting and health-check tool designed to examine an AD FS deployment and identify common configuration problems.

It analyzes components such as:

#### Certificates

- Service communications certificate
- Token-signing certificates
- Token-decrypting certificates

#### Endpoints

- Federation Metadata endpoint
- WS-Federation endpoints
- OAuth/OpenID Connect endpoints
- Device registration endpoints

#### Service Principal Names (SPNs)

- Verifies correct registration
- Detects duplicate or missing SPNs
- Helps identify authentication issues

#### AD FS farm configuration

- Trusts and configuration consistency
- Proxy configuration
- Authentication settings

#### Connectivity and health

- DNS resolution
- Endpoint availability
- Certificate validity and expiration

The tool is intended to identify and report issues, helping administrators quickly pinpoint likely causes of federation and authentication problems.

### Why other answers are incorrect

#### Incorrect choice: Perform the MFA step for users who cannot sign in

The Diagnostic Analyzer is a troubleshooting tool.

It does not participate in authentication or perform MFA challenges.

MFA is handled by AD FS and the configured MFA provider.

#### Incorrect choice: Provision users from the HR system into Active Directory

User provisioning is handled by solutions such as:

- Microsoft Identity Manager (MIM)
- Microsoft Entra Connect
- Microsoft Entra Provisioning

The Diagnostic Analyzer does not create or synchronize user accounts.

#### Incorrect choice: Replace failed farm nodes automatically with fresh new ones

The tool only analyzes and reports issues.

It does not perform automatic server replacement, rebuild AD FS farms, or deploy infrastructure.

### Exam tip

**When you see AD FS Diagnostic Analyzer, think:**

- ✅ Health checks
- ✅ Certificate validation
- ✅ Endpoint verification
- ✅ SPN checks
- ✅ Configuration analysis
- ❌ Not authentication
- ❌ Not user provisioning
- ❌ Not automatic remediation

The keyword is:

"Analyzer" = Detects and reports problems, it does not fix them automatically.

### Final answer

> **Answer:** ✅ Check certificates, endpoints, SPNs and flag the problems.

---

## Question 12

**Prompt:** The audit shows many 1203 events from one IP. Which action is appropriate?

> [!CAUTION]
> A burst of Event ID 1203 records from one IP is an investigation signal, not proof by itself. Confirm the event meaning for the deployed AD FS version and correlate timestamps, account patterns, proxy addresses, and other security telemetry before concluding that it is a password spray.

### Correct answer

> **Answer:** ✅ Treat it as a password spray: enforce lockout and block the IP

### Explanation

In AD FS, a large number of authentication-related events originating from a single IP address can be a strong indicator of a password spray attack.

A password spray attack occurs when an attacker:

- Tries a small set of common passwords
- Against many user accounts
- From the same source IP address
- To avoid triggering traditional account lockout thresholds

When AD FS auditing reveals numerous Event ID 1203 entries from a single external IP, administrators should investigate the activity as potentially malicious and take protective actions such as:

- Enabling or reviewing Extranet Smart Lockout (ESL)
- Blocking the offending IP address on the firewall, WAF, or reverse proxy
- Reviewing affected accounts
- Monitoring for additional attack indicators
- Increasing security controls such as MFA enforcement

This is the appropriate security response rather than ignoring the activity.

### Why other answers are incorrect

#### Incorrect choice: Disable auditing to stop the security event log filling up

Auditing provides the evidence needed to detect attacks.

Disabling it would remove visibility into the threat.

Security monitoring should be enhanced, not disabled.

#### Incorrect choice: Delete the relying party trust that the source IP is attacking

Password spray attacks target user authentication, not necessarily a specific relying party trust.

Deleting a relying party trust could cause service outages without addressing the attacker.

#### Incorrect choice: Ignore it, because event 1203 is only an informational message

Even informational events can reveal attack patterns.

A high volume of authentication-related events from one IP should never be dismissed without investigation.

Security teams should correlate the events and determine whether malicious activity is occurring.

### Exam tip

**When you see:**

- Many authentication events
- One external IP
- Repeated failed sign-in activity

**Think:**

- Password Spray Attack

**Recommended mitigations:**

- ✅ Enable Extranet Smart Lockout (ESL)
- ✅ Block malicious source IPs
- ✅ Review AD FS Security and Admin logs
- ✅ Require MFA
- ✅ Monitor affected accounts

A common AD FS security troubleshooting mindset is:

Repeated failures from one IP = investigate as password spray until proven otherwise.

### Final answer

> **Answer:** ✅ Treat it as a password spray: enforce lockout and block the IP.

---

## Question 13

**Prompt:** Which command enables the IdP-initiated sign-on page?

### Correct answer

> **Answer:** ✅ Set-AdfsProperties -EnableIdPInitiatedSignonPage $true

### Explanation

Starting with AD FS 2016, the IdP-Initiated Sign-On page is disabled by default for security reasons.

If users browse to:

```text
https://<FederationService>/adfs/ls/idpinitiatedsignon
```

they may receive errors such as:

```text
No protocol handler was found
```

or

```text
No handler exists for this endpoint
```

To enable the page, Microsoft provides the following PowerShell command:

```powershell
Set-AdfsProperties -EnableIdPInitiatedSignonPage $true
```

You can verify the current setting using:

```powershell
Get-AdfsProperties | Select EnableIdPInitiatedSignonPage
```

Expected output:

```text
EnableIdPInitiatedSignonPage : True
```

Once enabled, the IdP-initiated sign-on page becomes accessible again.

### Why other answers are incorrect

#### Incorrect choice: Enable-AdfsIdpInitiatedSignon -Enabled $true -Force

There is no standard AD FS PowerShell cmdlet called:

```powershell
Enable-AdfsIdpInitiatedSignon
```

This is not a valid AD FS command.

#### Incorrect choice: Add-AdfsRelyingPartyTrust -IdpInitiatedSignon $true

Relying Party Trusts define trust relationships with applications.

They do not control whether the IdP-initiated sign-on page is enabled.

The parameter shown is not used for enabling the global IdP sign-on page.

#### Incorrect choice: Set-AdfsEndpoint -AddressPath /adfs/ls -Enabled $true

AD FS endpoints can be enabled or disabled.

However, the IdP-initiated sign-on page is controlled through AD FS properties, not by enabling the /adfs/ls endpoint.

This command would not solve the specific issue.

### Exam tip

A common AD FS interview question is:

Why does /adfs/ls/idpinitiatedsignon fail after upgrading to AD FS 2016 or later?

**Answer:**

✅ Because the IdP-Initiated Sign-On Page is disabled by default.

Commands to remember:

```powershell
Get-AdfsProperties | Select EnableIdPInitiatedSignonPage
Set-AdfsProperties -EnableIdPInitiatedSignonPage $true
```

These are among the most frequently tested AD FS administration commands.

### Final answer

> **Answer:** ✅ Set-AdfsProperties -EnableIdPInitiatedSignonPage $true

---

## Question 14

**Prompt:** A team load-balances AD FS nodes and wants to know whether the load balancer must keep users on the same node (session persistence). What is the correct guidance?

> [!NOTE]
> The source wording about SNI is preserved. Whether SNI is mandatory depends on the AD FS version, certificate-authentication design, and load-balancer mode; validate the appliance and Microsoft requirements for the actual deployment.

### Correct answer

> **Answer:** ✅ Not required, but the LB must support SNI and use /adfs/probe

### Explanation

In modern AD FS deployments, session persistence (sticky sessions) is generally not required.

AD FS is designed so that authentication requests can be processed by any federation server in the farm. Because configuration is shared across farm members, a user can be directed to different AD FS servers during the authentication process without issues.

What is important is that the load balancer is configured correctly:

- Support Server Name Indication (SNI) if required by the deployment.
- Use the AD FS health check endpoint:

```text
/adfs/probe
```

Example health probe URL for a specific farm node:

```text
http://<node>/adfs/probe
```

This endpoint is specifically designed for load balancer health checks and allows unhealthy AD FS servers to be removed automatically from the rotation. The source guidance uses **HTTP**, not HTTPS, for the node probe to avoid SNI-related probe failures. Configure the load balancer to probe each backend node rather than its own virtual IP.

### Why other answers are incorrect

#### Incorrect choice: It is always required for SAML sign-in to work correctly

Modern AD FS farms do not generally require session affinity for SAML authentication.

AD FS supports load-balanced operation without sticky sessions.

Microsoft guidance does not require persistence simply because SAML is being used.

#### Incorrect choice: It is required only for the LDAPS traffic between the farm nodes

LDAPS traffic is unrelated to load balancer session persistence.

Federation servers communicate with Active Directory independently of LB stickiness requirements.

#### Incorrect choice: It is required only for the Web Application Proxy servers

Session persistence is not specifically required for WAP servers either.

Proper health probes and load-balancer configuration are more important than sticky sessions.

### Exam tip

For AD FS load-balancing questions, remember:

| Component | Recommendation |
| --- | --- |
| Session Persistence | Usually Not Required |
| Health Check Endpoint | `/adfs/probe` |
| Load Balancer Health Monitoring | Required |
| SSL/TLS Support | Required |
| SNI Support | Recommended/Required depending on deployment |

A common certification question is:

Which endpoint should a load balancer use to determine AD FS server health?

✅ /adfs/probe

If the question asks whether sticky sessions are mandatory:

✅ No, they are generally not required.

### Final answer

> **Answer:** ✅ Not required, but the LB must support SNI and use /adfs/probe.

---

## Question 15

**Prompt:** A load balancer terminates TLS and re-encrypts to AD FS, breaking certificate authentication. What does Learn recommend?

### Correct answer

> **Answer:** ✅ Use SSL pass-through with SNI support and forward TCP 443

### Explanation

When certificate authentication is used with AD FS, the client certificate must be presented directly to the AD FS server.

If a load balancer performs:

- TLS termination (decrypts the client connection), and then
- TLS re-encryption to the AD FS servers,

the original client certificate may not reach AD FS correctly. As a result:

- Certificate authentication fails.
- Device authentication may fail.
- Smart card or certificate-based logons may not work.

Microsoft guidance for these scenarios is to use:

✅ SSL pass-through

This allows the TLS session to remain intact from the client all the way to the AD FS server.

**The load balancer should:**

- Forward TCP 443 directly.
- Support Server Name Indication (SNI) where required.
- Avoid interfering with client certificate exchange.

This preserves the certificate authentication flow expected by AD FS.

### Why other answers are incorrect

#### Incorrect choice: Use port 80 only between the load balancer and the farm nodes

Port 80 is unencrypted HTTP.

AD FS requires HTTPS for authentication.

Certificate authentication cannot function over HTTP.

#### Incorrect choice: Terminate TLS on the load balancer and disable client certs

This removes certificate authentication entirely.

If the goal is to support certificate-based authentication, disabling client certificates is not a solution.

#### Incorrect choice: Use UDP instead of TCP between the load balancer and the nodes

AD FS authentication uses HTTPS over TCP 443.

UDP is not used for AD FS web authentication traffic.

Changing to UDP would not address certificate authentication issues.

### Exam tip

For AD FS load-balancer questions, remember:

| Authentication method | Recommended LB mode |
| --- | --- |
| Forms Authentication | Follow the supported design for the deployed version; do not assume TLS termination is supported |
| Windows Authentication | Follow the supported design for the deployed version; do not assume TLS termination is supported |
| Client Certificate Authentication | ✅ SSL Pass-Through |
| Smart Card Authentication | ✅ SSL Pass-Through |

> [!IMPORTANT]
> The table preserves the question's focus on certificate authentication, but it should not be read as general approval for TLS bridging. Current deployment guidance and the exact AD FS/WAP scenario remain authoritative; SSL pass-through avoids terminating the original client TLS session at the load balancer.

**A common troubleshooting clue is:**

"Client certificate authentication works directly against AD FS but fails through the load balancer."

**Think:**

✅ Load balancer is terminating TLS and stripping the client certificate.

**The fix is:**

✅ SSL pass-through with SNI support and TCP 443 forwarding.

### Final answer

> **Answer:** ✅ Use SSL pass-through with SNI support and forward TCP 443.

---

## Question 16

**Prompt:** IdP-initiated sign-on shows no apps although relying party trusts exist. What is the typical cause?

### Correct answer

> **Answer:** ✅ The RP trusts lack the IdP-initiated option or required endpoints

### Explanation

The IdP-Initiated Sign-On page in AD FS displays only applications (Relying Party Trusts) that are configured to support IdP-initiated authentication.

Simply having a Relying Party Trust (RP Trust) configured in AD FS is not enough for it to appear on the sign-on page.

For an application to be listed:

- The RP Trust must support IdP-initiated sign-on.
- The required SAML endpoints (such as Assertion Consumer Service URLs) must be configured correctly.
- The trust configuration must allow AD FS to generate a valid sign-on link.

If RP trusts exist but no applications appear on the IdP-initiated sign-on page, the most common cause is that the trusts were not configured with the necessary IdP-initiated settings or endpoints.

### Why other answers are incorrect

#### Incorrect choice: Extranet smart lockout is enabled on the AD FS farm now

Extranet Smart Lockout protects against password spray attacks.

It affects authentication attempts.

It does not control whether applications appear on the IdP-initiated sign-on page.

#### Incorrect choice: DNS uses a CNAME for the federation service name record

Using a CNAME is a DNS design choice.

It does not determine whether RP trusts are displayed.

If DNS were incorrect, users would typically experience connectivity or certificate errors instead.

#### Incorrect choice: The domain controller that the farm uses is currently offline

A domain controller outage can cause authentication failures.

It does not directly affect whether configured applications are shown on the IdP sign-on page.

### Exam tip

**When troubleshooting the AD FS IdP-Initiated Sign-On page:**

| Symptom | Likely cause |
| --- | --- |
| Page not accessible | IdP page disabled |
| "No handler exists" error | IdP page disabled by default |
| Page opens but shows no applications | RP trusts missing IdP-initiated support or endpoints |
| App launches fail | Incorrect RP trust configuration |

A useful troubleshooting approach is:

1. Verify the IdP sign-on page is enabled.
2. Review the RP Trust configuration.
3. Confirm SAML endpoints are configured correctly.
4. Validate the trust supports IdP-initiated authentication.

### Final answer

> **Answer:** ✅ The RP trusts lack the IdP-initiated option or required endpoints

---

## Question 17

**Prompt:** An engineer wants a quick health check of AD FS from the perspective of Entra. Which tool does this?

### Correct answer

> **Answer:** ✅ Microsoft Entra Connect Health for AD FS agents

### Explanation

Microsoft Entra Connect Health for AD FS is Microsoft's cloud-based monitoring and health-check solution for AD FS environments.

It provides visibility into the health and performance of:

- AD FS federation servers
- Web Application Proxy (WAP) servers
- Authentication activity
- Token issuance activity
- Certificate status
- Trust relationships
- Sign-in trends and failures

By installing the Entra Connect Health Agent on AD FS servers and WAP servers, administrators can view health information directly from the Microsoft Entra portal.

**Common insights include:**

- AD FS service outages
- Authentication failures
- Certificate expiration warnings
- Performance bottlenecks
- Extranet lockout events
- Server availability issues

This makes it the preferred tool when you want a health assessment of AD FS from Microsoft's cloud and Entra perspective.

### Why other answers are incorrect

#### Incorrect choice: Windows Admin Center on a single server only

Windows Admin Center manages Windows servers.

It does not provide federation-wide AD FS health analytics from the Entra perspective.

Monitoring is limited to the server(s) being managed.

#### Incorrect choice: Microsoft Defender for Endpoint device reports

Defender for Endpoint focuses on endpoint security, threat detection, vulnerability management, and device health.

It does not provide specialized AD FS federation health monitoring.

#### Incorrect choice: Microsoft Entra Domain Services health monitoring

Microsoft Entra Domain Services (formerly Azure AD DS) monitors managed domain services.

It is unrelated to AD FS server health and federation monitoring.

### Exam tip

**When you see:**

- AD FS monitoring
- Health dashboard
- Microsoft cloud perspective
- Federation server health

**Think:**

✅ Microsoft Entra Connect Health

Common workloads monitored by Entra Connect Health:

| Workload | Supported |
| --- | --- |
| AD FS | ✅ |
| Web Application Proxy | ✅ |
| Microsoft Entra Connect Sync | ✅ |
| Active Directory Domain Services | ✅ |

A good rule to remember:

Entra Connect Health = Microsoft's primary monitoring solution for AD FS environments.

### Final answer

> **Answer:** ✅ Microsoft Entra Connect Health for AD FS agents

---

## Question 18

**Prompt:** An engineer wants to correlate events for a single request across nodes. What is used?

### Correct answer

> **Answer:** ✅ The activity ID or correlation ID in the AD FS events

### Explanation

When troubleshooting AD FS authentication issues, a single user request may pass through multiple components, including:

- Web Application Proxy (WAP)
- AD FS federation servers
- Load balancers
- Multiple farm nodes

To trace that specific authentication transaction across logs and servers, AD FS assigns an Activity ID (Correlation ID) to the request.

The Activity ID allows administrators to:

- Track a sign-in request end-to-end
- Correlate related events across multiple AD FS servers
- Match Admin and Debug log entries
- Troubleshoot authentication, claims, and token issuance failures more efficiently

A typical AD FS event may contain:

```text
Activity ID:
12345678-abcd-1234-abcd-1234567890ab
```

Administrators can search Event Viewer logs using this identifier to reconstruct the authentication flow.

### Why other answers are incorrect

#### Incorrect choice: The SPN registered for the AD FS service account

Service Principal Names (SPNs) are used for Kerberos authentication.

They help clients locate services.

They do not correlate individual authentication requests across servers.

#### Incorrect choice: The IP address of the Web Application Proxy server

A WAP IP address identifies the proxy handling traffic.

Many different requests can originate through the same WAP.

It cannot uniquely track a single authentication transaction.

#### Incorrect choice: The thumbprint of the token-signing certificate on each node

Certificate thumbprints identify certificates.

They are useful for trust and certificate troubleshooting.

They do not correlate events belonging to a specific user request.

### Exam tip

For AD FS troubleshooting, remember:

| Purpose | Tool/identifier |
| --- | --- |
| Correlate a single authentication request | ✅ Activity ID / Correlation ID |
| Verify Kerberos configuration | SPN |
| Verify federation trust | Token-signing certificate |
| Trace external traffic source | WAP IP Address |

**A commonly tested troubleshooting workflow is:**

1. Obtain the Activity ID from the error page or event.
2. Search the AD FS Admin or Debug logs.
3. Follow all events containing the same Activity ID.
4. Identify where the authentication flow failed.

Useful PowerShell example:

```powershell
Get-WinEvent -LogName "AD FS/Admin" |
Where-Object {$_.Message -match "<ActivityID>"}
```

### Final answer

> **Answer:** ✅ The activity ID or correlation ID in the AD FS events

---

## Question 19

**Prompt:** A load balancer health probe keeps failing and nodes are removed. Which AD FS endpoint is used for probes?

> [!CAUTION]
> **Source and version differences:** this assessment preserves `http://<node>/adfs/probe` as the expected answer. Other materials or appliances may show different probe schemes or host formats. Do not silently substitute one for another; verify the required scheme, per-node targeting, certificate/SNI behavior, and probe settings for the deployed AD FS version and load balancer.

### Correct answer

> **Answer:** ✅ http://<node>/adfs/probe

### Explanation

Microsoft recommends using the dedicated AD FS probe endpoint for load balancer health monitoring:

```text
http://<node>/adfs/probe
```

This endpoint was specifically created for load balancers to determine whether an AD FS server is healthy and able to service requests.

When the load balancer periodically queries:

```text
/adfs/probe
```

the AD FS server returns a successful response if the AD FS service is functioning correctly. If the probe fails repeatedly, the load balancer can automatically remove that node from the pool.

This helps maintain high availability by ensuring users are only directed to healthy federation servers.

### Why other answers are incorrect

#### Incorrect choice: ldap://<node>:389

LDAP port 389 is used for Active Directory communication.

It does not validate the health of the AD FS service itself.

A domain controller may be reachable while AD FS is unhealthy.

#### Incorrect choice: https://<node>/FederationMetadata

Federation metadata is used by relying parties to discover federation settings and certificates.

It is not Microsoft's recommended health-check endpoint.

Using it for probes creates unnecessary processing overhead.

#### Incorrect choice: https://<node>/adfs/ls

/adfs/ls is the AD FS sign-in endpoint.

It is designed for authentication requests, not health monitoring.

Using authentication endpoints for health probes is inefficient and not recommended.

### Exam tip

For AD FS load-balancing questions, remember this key fact:

| Purpose | Endpoint |
| --- | --- |
| Health Probe | ✅ `/adfs/probe` |
| User Authentication | `/adfs/ls` |
| Federation Metadata | `/FederationMetadata/...` |
| Device Registration | `/adfs` device endpoints |

A very common AD FS infrastructure question is:

Which endpoint should a load balancer monitor?

Always think:

✅ /adfs/probe

### Final answer

> **Answer:** ✅ http://<node>/adfs/probe

---

## Question 20

**Prompt:** MFA via the AD FS adapter fails with an error about the tenant certificate. What is the usual remedy?

### Correct answer

> **Answer:** ✅ Register a new tenant certificate, re-run Set-AdfsAzureMfaTenant

### Explanation

When using the Microsoft Entra MFA Adapter (formerly Azure MFA Adapter) with AD FS, communication between AD FS and Microsoft Entra ID relies on a valid tenant certificate.

If the certificate:

- Has expired
- Is missing
- Is corrupted
- Has not been registered correctly

you may see MFA failures and errors referencing the tenant certificate.

**The typical remediation process is to:**

1. Register or renew the tenant certificate.
2. Re-establish the trust between AD FS and Microsoft Entra ID.

Re-run the configuration command:

```powershell
Set-AdfsAzureMfaTenant
```

This updates the tenant configuration and allows the AD FS Azure MFA Adapter to authenticate properly with Entra ID services again.

This is one of the most common fixes for Azure/Entra MFA Adapter certificate-related issues.

### Why other answers are incorrect

#### Incorrect choice: Rename the farm and then reissue the SSL certificate for it

Changing the AD FS farm name is a major infrastructure change.

The issue is related to the MFA tenant certificate, not the federation service name or SSL certificate.

This would not normally resolve MFA adapter certificate errors.

#### Incorrect choice: Disable WIA for the extranet endpoints on all farm nodes

Windows Integrated Authentication (WIA) controls how users authenticate.

It does not affect the registration or validity of the Entra MFA tenant certificate.

MFA adapter certificate issues will remain unresolved.

#### Incorrect choice: Reset the password of each affected user and clear the AD FS cache

User passwords are unrelated to tenant certificate authentication.

The issue exists between AD FS and Entra MFA services, not between users and Active Directory.

Resetting passwords would not fix the certificate trust problem.

### Exam tip

For AD FS + Entra MFA questions:

| Symptom | Likely fix |
| --- | --- |
| MFA adapter registration failure | Re-run `Set-AdfsAzureMfaTenant` |
| Tenant certificate expired | Register/renew certificate |
| MFA works internally but not externally | Check WAP and Extranet policies |
| Double MFA prompts | `acceptIfMfaDoneByFederatedIdp` |
| MFA adapter errors in logs | Review AD FS Admin Log |

A common troubleshooting pattern is:

Certificate error + Azure/Entra MFA Adapter = Re-register the tenant certificate and rerun Set-AdfsAzureMfaTenant.

### Final answer

> **Answer:** ✅ Register a new tenant certificate, re-run Set-AdfsAzureMfaTenant
