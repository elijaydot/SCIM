# AD FS, Extranet Smart Lockout, Federation & Auditing - 20 Question Study Guide

---

# Question 1

## Question
A user sees a generic AD FS error page. Where should you look first for the actual exception details?

### Correct Answer
✅ AD FS Admin event log (for example Event ID 364)

### Why this is correct
The AD FS Admin log contains detailed error information, including stack traces, certificate errors, LDAP failures, claims issuance issues, and other authentication failures. Event ID 364 is one of the most common events used for AD FS troubleshooting.

### Why the other options are incorrect

❌ Entra Connect Health synchronization error reports

- Focus on synchronization and health monitoring.
- Do not provide detailed authentication exception information.

❌ Managed Domain health blade in Azure portal

- Provides service health information.
- Does not contain the underlying AD FS exception details.

❌ IIS logs on Web Application Proxy servers

- Useful for connectivity troubleshooting.
- Usually do not provide the actual AD FS exception.

---

# Question 2

## Question
Extranet Smart Lockout counters are inconsistent across farm nodes. What is the likely cause?

### Correct Answer
✅ Secondary nodes must reach the primary node on port 80

### Why this is correct
AD FS Extranet Smart Lockout synchronizes lockout information from the User Activity primary node to other farm nodes. Secondary nodes communicate with the primary node over port 80 to exchange lockout and familiar-location data.

### Why the other options are incorrect

❌ Port 636 must be open

- Port 636 is LDAPS.
- Not used for ESL synchronization.

❌ Microsoft Entra Connect synchronizes the counters

- Entra Connect performs directory synchronization.
- Does not manage ESL counters.

❌ WAP servers synchronize the counters

- WAP servers proxy requests only.
- They do not maintain or synchronize ESL state.

---

# Question 3

## Question
An AD FS farm using WID has more than 100 relying party trusts. What is recommended?

### Correct Answer
✅ Move to a SQL Server farm; WID is meant for about 100 trusts

### Why this is correct
WID is suitable for smaller deployments. As relying party trusts increase, SQL Server provides better scalability and manageability.

### Why the other options are incorrect

❌ Add more Web Application Proxy servers

- WAP capacity is unrelated to AD FS configuration database scalability.

❌ No change needed because WID has no limit

- There are practical scaling recommendations for WID deployments.

❌ Convert relying party trusts to claims provider trusts

- Different trust types serve different purposes.
- Does not solve scalability concerns.

---

# Question 4

## Question
A SaaS application reports "token not yet valid." The application's clock is 3 minutes ahead of AD FS. What setting should be adjusted?

### Correct Answer
✅ NotBeforeSkew on the relying party trust

### Why this is correct
NotBeforeSkew compensates for clock differences between AD FS and the application consuming the token.

### Why the other options are incorrect

❌ AutoCertificateRollover

- Controls certificate rotation.
- Does not affect token times.

❌ TokenLifetime

- Controls token expiry.
- Not token start validity.

❌ EncryptClaims

- Controls token encryption.
- Unrelated to clock differences.

---

# Question 5

## Question
What happens when AutoCertificateRollover is enabled?

### Correct Answer
✅ A new certificate is generated early, and relying parties must refresh metadata to pick it up

### Why this is correct
AD FS generates a replacement token-signing certificate before expiration and publishes it in federation metadata.

### Why the other options are incorrect

❌ No relying party needs updating

- Relying parties still need metadata updates.

❌ Entra ID signs tokens automatically

- AD FS signs its own tokens.

❌ AD FS stops issuing tokens

- AutoCertificateRollover prevents this scenario.

---

# Question 6

## Question
Which cmdlet shows Extranet Smart Lockout activity, including familiar IPs?

### Correct Answer
✅ Get-ADFSAccountActivity

### Why this is correct
The cmdlet displays:

- Familiar locations
- Bad password counters
- Account activity information

### Why the other options are incorrect

❌ Get-AdfsEndpoint

- Shows endpoint configuration.

❌ Get-WebApplicationProxyApplication

- Displays WAP-published applications.

❌ Get-AdfsCertificate

- Displays AD FS certificates.

---

# Question 7

## Question
The PDC Emulator becomes unavailable and extranet authentication stops working due to lockout checking. What should be done?

### Correct Answer
✅ Disable ExtranetLockoutRequirePDC so another DC can be used

### Why this is correct
This removes dependency on the PDC Emulator and allows other domain controllers to process lockout checks.

### Why the other options are incorrect

❌ Convert SQL to WID

- Unrelated to DC availability.

❌ Increase lockout threshold

- Does not remove PDC dependency.

❌ Enable ExtranetLockoutRequirePDC on WAP

- Not a WAP setting.

---

# Question 8

## Question
A single user receives MSIS7068 Access Denied for one application. What is the likely cause?

### Correct Answer
✅ An authorization rule on that relying party trust denies the user

### Why this is correct
Authentication succeeds, but authorization fails for that specific application.

### Why the other options are incorrect

❌ Invalid token-signing certificate

- Would affect many users.

❌ Broken WAP trust

- Would affect numerous users.

❌ DNS issue

- Would affect broader connectivity.

---

# Question 9

## Question
AD FS service fails to start after a certificate renewal because the private key cannot be accessed. What is the solution?

### Correct Answer
✅ Import the certificate with its key and grant the AD FS account access

### Why this is correct
AD FS requires access to the certificate's private key to function properly.

### Why the other options are incorrect

❌ Rename federation service

- Unrelated to private key permissions.

❌ Rejoin server to domain

- Unnecessary.

❌ Import certificate without private key

- AD FS requires the private key.

---

# Question 10

## Question
Which AD FS Security Audit Event indicates credential validation failure?

### Correct Answer
✅ Event ID 1203

### Why this is correct

- Event 1202 = Credential validated successfully
- Event 1203 = Credential validation failed
- Event 1200 = Token issued
- Event 1201 = Token issuance failed

### Why the other options are incorrect

❌ Event 1200

- Token issued.

❌ Event 1201

- Token issuance failure.

❌ Event 1202

- Successful credential validation.

---

# Question 11

## Question
Kerberos authentication to AD FS stops working after the federation SPN is added to another account. What is wrong?

### Correct Answer
✅ A duplicate SPN; only the AD FS service account should hold it

### Why this is correct
Kerberos cannot determine the correct account when duplicate SPNs exist.

### Why the other options are incorrect

❌ Register the SPN on WAP

- Incorrect location.

❌ Delete the SPN everywhere

- The service account still needs it.

❌ AD FS does not use SPNs

- Kerberos relies on SPNs.

---

# Question 12

## Question
A tenant uses federation with AD FS. Entra Smart Lockout is enabled, yet attackers still lock AD accounts. Why?

### Correct Answer
✅ It does not cover federated sign-ins, which AD FS validates itself

### Why this is correct
In a federated environment, AD FS performs password validation, not Entra ID.

### Why the other options are incorrect

❌ Wait 24 hours

- Not required.

❌ AD FS disables Entra Smart Lockout

- Not true.

❌ P2 licensing issue

- Not the cause.

---

# Question 13

## Question
A SaaS vendor provides SAML metadata and wants users to authenticate using AD FS. Which object should be created?

### Correct Answer
✅ A relying party trust for the vendor's application

### Why this is correct
The SaaS application relies on tokens issued by AD FS.

### Why the other options are incorrect

❌ WAP application

- Not required.

❌ Forest trust

- Unrelated to SAML federation.

❌ Claims provider trust

- Used when AD FS receives claims from another IdP.

---

# Question 14

## Question
Before enforcing Smart Lockout, an organization wants AD FS to learn familiar locations. Which mode should be enabled?

### Correct Answer
✅ ADFSSmartLockoutLogOnly

### Why this is correct
Log-only mode learns familiar locations while continuing existing soft-lockout behavior.

### Why the other options are incorrect

❌ ADFSSmartLockoutEnforce

- Enforces immediately.

❌ ADPasswordCounter

- Not a lockout mode.

❌ Disable extranet lockout

- Removes protection.

---

# Question 15

## Question
The security team wants to know who authenticated and which tokens AD FS issued. What must be enabled?

### Correct Answer
✅ Enable AD FS auditing and the Audit Application Generated policy

### Why this is correct
This records authentication and token issuance events in the Security log.

### Why the other options are incorrect

❌ Entra Sign-in Logs

- Do not capture full AD FS audit detail.

❌ NSG Flow Logs

- Capture network traffic only.

❌ Application Insights in B2C

- Unrelated to AD FS auditing.

---

# Question 16

## Question
An attacker password-sprays from unknown IPs while a legitimate user signs in daily from the office. What happens when Extranet Smart Lockout is enforced?

### Correct Answer
✅ The attacker is blocked but the user on a familiar IP can still sign in

### Why this is correct
ESL differentiates familiar and unfamiliar locations.

### Why the other options are incorrect

❌ Legitimate user is blocked

- ESL avoids this.

❌ Both accounts are locked

- Opposite of ESL design.

❌ Lockout only applies internally

- ESL applies to extranet access.

---

# Question 17

## Question
Enabling Extranet Smart Lockout fails with a database permission error. What is the likely cause?

### Correct Answer
✅ The service account cannot create a table in the AD FS artifact database

### Why this is correct
ESL creates and uses AccountActivity tables and requires database permissions.

### Why the other options are incorrect

❌ Missing SSL certificate

- Not a database issue.

❌ Domain functional level too low

- Not related to database permissions.

❌ Expired WAP trust

- Not related to database access.

---

# Question 18

## Question
A familiar-location user is locked out and Helpdesk must immediately unlock the account. Which command should be used?

### Correct Answer

```powershell
Reset-ADFSAccountLockout -UserPrincipalName user@contoso.com -Location Familiar
```

### Why this is correct
The cmdlet directly clears the ESL lockout state.

### Why the other options are incorrect

❌ Disable Extranet Lockout

- Excessive and unnecessary.

❌ Restart AD FS services

- Does not clear lockout counters.

❌ Remove and recreate RP trust

- Unrelated to lockout status.

---

# Question 19

## Question
Users must authenticate through AD FS from the internet without exposing AD FS servers directly. What is the proper design?

### Correct Answer
✅ Place Web Application Proxy servers in the perimeter network and publish the federation service through them

### Why this is correct
This is the standard Microsoft AD FS perimeter design.

### Why the other options are incorrect

❌ Replace AD FS with Entra Domain Services

- Different technology.

❌ Install AD FS on a Domain Controller and open port 389

- Poor security design.

❌ Publish AD FS directly to the internet

- Not recommended.

---

# Question 20

## Question
A tenant uses Password Hash Sync. AD DS lockout threshold is 5 attempts for 60 seconds. How should Entra Smart Lockout be configured?

### Correct Answer
✅ Set the threshold lower and the lockout duration longer than AD DS

### Why this is correct
Entra should stop password-spray attacks before they can trigger AD DS lockouts.

### Why the other options are incorrect

❌ Match AD DS settings

- Does not provide earlier protection.

❌ Disable Smart Lockout

- Removes an important security layer.

❌ Higher threshold and shorter duration

- Makes AD DS lockouts more likely.

---

# Quick Answer Sheet

| Q# | Correct Answer |
|----|---------------|
| 1 | AD FS Admin Event Log (364) |
| 2 | Secondary nodes must reach primary node on Port 80 |
| 3 | Move to SQL Server farm |
| 4 | NotBeforeSkew |
| 5 | New certificate generated, RP refreshes metadata |
| 6 | Get-ADFSAccountActivity |
| 7 | Disable ExtranetLockoutRequirePDC |
| 8 | Authorization rule denies user |
| 9 | Import certificate with private key and grant access |
| 10 | Event 1203 |
| 11 | Duplicate SPN |
| 12 | Federated sign-ins validated by AD FS |
| 13 | Relying Party Trust |
| 14 | ADFSSmartLockoutLogOnly |
| 15 | Enable AD FS Auditing + Audit Application Generated |
| 16 | Attacker blocked, familiar user allowed |
| 17 | Service account lacks database permissions |
| 18 | Reset-ADFSAccountLockout |
| 19 | Use Web Application Proxy in DMZ |
| 20 | Lower threshold, longer duration than AD DS |