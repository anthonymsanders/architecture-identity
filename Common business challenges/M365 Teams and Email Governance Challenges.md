# M365 Teams and Email Governance Challenges

**Date:** 11 February 2026

---

## Overview

This document identifies common governance challenges for **Microsoft Teams** and **Exchange Online (Email)** across different user personas. Effective collaboration governance requires balancing productivity with security, compliance and data protection — and the right approach varies significantly depending on the user type.

All guidance assumes a **Microsoft 365** environment with **Microsoft Entra ID** as the identity provider.

---

## 1. User Personas

The following personas are used throughout this document. Organisations may have all or a subset of these depending on their sector and size.

| Persona | Description | Typical M365 Licence |
|---------|-------------|----------------------|
| **Staff (Permanent Employees)** | Full-time and part-time employees on payroll | M365 E3/E5 or Business Premium |
| **Students** | Learners in education institutions (schools, colleges, universities) | M365 A1/A3/A5 (Education) |
| **Contractors / Temporary Workers** | Fixed-term or agency workers engaged for specific projects | M365 E1/E3 or guest access |
| **External Guests (B2B)** | Partners, suppliers and collaborators from external organisations | Entra External ID (B2B guest) |
| **Executives / Senior Leadership** | C-suite and board members with heightened security requirements | M365 E5 with enhanced controls |
| **IT Administrators** | Technical staff managing the M365 tenant and services | M365 E5 + admin roles |
| **Alumni / Leavers** | Former staff or students who may retain limited access post-departure | Reduced licence or guest conversion |

---

## 2. Microsoft Teams Governance Challenges

### 2.1 Team Creation and Sprawl

Uncontrolled team creation leads to duplication, abandoned teams and confused ownership.

| Persona | Challenge | Impact |
|---------|-----------|--------|
| **Staff** | Any employee can create teams freely, leading to duplicate and abandoned teams | Content fragmentation; sensitive data scattered across ungoverned teams |
| **Students** | Students create teams for study groups, social purposes and projects with no lifecycle management | Massive team sprawl; abandoned teams persist after course completion |
| **Contractors** | Contractors create teams that outlive their engagement period | Orphaned teams with external members retain access to internal data |
| **External Guests** | Guests cannot create teams but are added to multiple teams without review | Guest access accumulates across teams with no expiry or recertification |
| **Executives** | Executive teams contain highly sensitive strategic content | Insufficient access controls; risk of accidental sharing or data leakage |
| **IT Administrators** | Admins must balance enablement with control across thousands of teams | Policy enforcement at scale without blocking legitimate collaboration |

**Common Governance Controls:**
- Restrict team creation to specific security groups via Entra ID group creation policies
- Enforce **naming conventions** using Microsoft 365 group naming policies (prefixes/suffixes by department, project or classification)
- Deploy **team templates** with pre-configured channels, tabs and settings for common use cases
- Implement **M365 group expiration policies** to automatically expire teams after 90, 180 or 365 days unless the owner renews
- Use **sensitivity labels** to classify teams and enforce protection settings (privacy, guest access, sharing) based on classification

---

### 2.2 Team Ownership and Accountability

Every team requires at least one active owner responsible for membership, content and lifecycle decisions.

| Persona | Challenge | Impact |
|---------|-----------|--------|
| **Staff** | Teams created for short-term projects are left ownerless when the creator moves roles or leaves | No one to approve membership requests, review content or respond to expiry notifications |
| **Students** | Student-created teams have no faculty oversight and no ownership transfer when students graduate | Ungoverned content persists; potential policy violations go undetected |
| **Contractors** | Contractors own teams during an engagement but lose access when their contract ends | Team becomes orphaned; remaining members cannot manage it |
| **IT Administrators** | Admins become default owners of abandoned teams, creating an unsustainable burden | Admin team bloat; admins own teams whose content they should not access |

**Common Governance Controls:**
- Require **minimum two owners** per team via Teams admin policies
- Deploy **ownerless group policy** in M365 Admin Centre to notify nominated users when a team loses all owners
- Use **Entra ID Lifecycle Workflows** to trigger ownership transfer when an owner leaves the organisation
- Run periodic **access reviews** targeting team owners to validate active ownership

---

### 2.3 Guest and External Access in Teams

Controlling how external users participate in Teams is critical for data protection.

| Persona | Challenge | Impact |
|---------|-----------|--------|
| **Staff** | Staff invite guests without considering data sensitivity in the team | Guests access files, chat history and shared resources beyond what was intended |
| **Students** | Students invite personal contacts or users from other institutions without oversight | Unvetted external access to educational content and potentially sensitive student data |
| **External Guests** | Guests accumulate access across multiple teams with no expiry or review | Persistent access long after the collaboration need has ended |
| **Executives** | Executive assistants add external advisors or board members as guests to sensitive teams | Confidential board and strategy content exposed to external identities |
| **Contractors** | Contractors are added as guests rather than member accounts, creating inconsistent identity types | Mixed identity types complicate governance; contractor activity harder to audit |

**Common Governance Controls:**
- Configure **Entra ID external collaboration settings** to restrict guest invitations to specific roles or groups
- Apply **Conditional Access policies** for guest users requiring MFA, compliant devices and accepted Terms of Use
- Set **guest access expiration** (e.g. 30, 60 or 90 days) via Entra ID access reviews with sponsor re-confirmation
- Use **sensitivity labels** to block guest access on teams classified as confidential or highly confidential
- Deploy **information barriers** to prevent guests from accessing teams or users in restricted segments

---

### 2.4 Channel Management and Private/Shared Channels

Private and shared channels introduce complexity around membership, data residency and compliance.

| Persona | Challenge | Impact |
|---------|-----------|--------|
| **Staff** | Private channels created within teams bypass the team's membership controls | Sensitive discussions occur outside the visibility of team owners |
| **Students** | Students create private channels for social conversations, circumventing institutional monitoring | Safeguarding and duty-of-care obligations are harder to enforce |
| **External Guests** | Shared channels allow cross-tenant collaboration but complicate data governance | Data resides in the host tenant; the guest's organisation loses visibility of their user's activity |
| **Contractors** | Contractors are added to shared channels across multiple teams, making access difficult to track | Audit trail fragmented; offboarding requires checking every shared channel individually |

**Common Governance Controls:**
- Control **who can create private channels** via Teams admin policies
- Manage **shared channel creation** and cross-tenant access via Entra ID cross-tenant access settings
- Use **Microsoft Purview eDiscovery** and **Communication Compliance** to cover private and shared channel content
- Audit private channel membership changes via the **M365 unified audit log**

---

### 2.5 Data Loss Prevention and Information Protection in Teams

Teams chat and file sharing are high-risk vectors for data leakage.

| Persona | Challenge | Impact |
|---------|-----------|--------|
| **Staff** | Employees share sensitive files (HR data, financials, customer PII) in Teams chat without classification | Data leakage through chat; files downloaded to unmanaged devices |
| **Students** | Students share personal information, assessment answers or copyrighted material in Teams | GDPR/FERPA violations; academic integrity breaches |
| **External Guests** | Guests download shared files from Teams to unmanaged personal devices | Organisation loses control of data once it leaves the tenant |
| **Executives** | Board papers and M&A discussions shared in Teams without enhanced protection | High-impact data exposure if an executive account is compromised |
| **IT Administrators** | Admins must configure DLP across chat, channels and file sharing without disrupting workflows | Over-restrictive policies generate alert fatigue; under-restrictive policies miss genuine leaks |

**Common Governance Controls:**
- Deploy **Microsoft Purview Data Loss Prevention (DLP)** policies for Teams chat and channel messages to detect and block sensitive data (PII, financial data, health records)
- Apply **sensitivity labels** to files shared in Teams, enforcing encryption and access restrictions
- Use **Conditional Access session controls** with **Microsoft Defender for Cloud Apps** to prevent file downloads on unmanaged devices
- Enable **Microsoft Purview Information Protection** auto-labelling to classify content shared in Teams automatically
- Configure **communication compliance policies** to detect inappropriate content, regulatory violations or data sharing in Teams messages

---

### 2.6 Teams Lifecycle and Retention

Teams content must be retained for compliance and disposed of when no longer needed.

| Persona | Challenge | Impact |
|---------|-----------|--------|
| **Staff** | Project teams are abandoned but content is retained indefinitely without policy | Storage bloat; stale content appears in search results; compliance risk from over-retention |
| **Students** | Course-based teams should be archived at end of term but often persist for years | Student data retained beyond legitimate educational purpose; GDPR/data minimisation violations |
| **Contractors** | Teams created for contractor engagements are not cleaned up when the contract ends | Former contractor content remains accessible to team members indefinitely |
| **Alumni / Leavers** | Content created by leavers remains in teams but cannot be managed by the departed user | Ownership gaps; content may contain personal data requiring deletion under GDPR subject access requests |

**Common Governance Controls:**
- Configure **M365 group expiration policies** with 90, 180 or 365-day cycles and owner renewal notifications
- Deploy **Microsoft Purview retention policies** for Teams channel messages, chat messages and files with defined retention and deletion periods
- Use **retention labels** for specific high-value or regulated content requiring longer retention
- Archive inactive teams via **Teams archive functionality** to preserve content in a read-only state
- Automate end-of-term archival for education using **Power Automate** or **Graph API** workflows triggered by academic calendar events

---

## 3. Exchange Online (Email) Governance Challenges

### 3.1 Mailbox Provisioning and Licensing

Email access must be provisioned appropriately for each persona with correct licence and mailbox type.

| Persona | Challenge | Impact |
|---------|-----------|--------|
| **Staff** | All employees receive a mailbox, but licensing varies and shared mailbox usage is inconsistent | Cost inefficiency; shared mailboxes used to avoid licensing when individual mailboxes are needed |
| **Students** | Large student populations require bulk provisioning often tied to enrolment systems | Delays in mailbox availability; students cannot access course materials or institutional communications |
| **Contractors** | Contractors may receive full mailboxes, shared mailbox access or no mailbox at all, inconsistently | Contractors sending email from personal accounts creates phishing risk and brand damage |
| **External Guests** | Guests do not receive an Exchange Online mailbox but may need to receive emails from internal distribution lists | Guests excluded from essential communications; workarounds create shadow distribution lists |
| **Alumni / Leavers** | Former employees and graduates expect to retain their email address but the organisation needs to reclaim the licence | Email forwarding to personal accounts creates data leakage; abrupt cutoff damages relationships |

**Common Governance Controls:**
- Automate mailbox provisioning via **Entra ID HR-driven provisioning** or **Lifecycle Workflows** triggered by HR system events
- Define **mailbox type standards** by persona (individual mailbox for staff/students, shared mailbox for teams, no mailbox for short-term guests)
- Implement **alumni mailbox conversion** policies: convert to shared mailbox, enable forwarding for a defined period, then disable
- Use **Exchange Online licence assignment** via Entra ID dynamic groups based on user attributes

---

### 3.2 Email External Forwarding and Data Exfiltration

Automatic forwarding of email to external addresses is a common data exfiltration vector.

| Persona | Challenge | Impact |
|---------|-----------|--------|
| **Staff** | Employees set up inbox rules to forward all email to personal Gmail or Outlook.com accounts | Sensitive business data continuously exfiltrated to unmanaged personal accounts |
| **Students** | Students forward institutional email to personal accounts to avoid checking a second inbox | Student data (grades, financial aid, health services) sent to unprotected personal email |
| **Contractors** | Contractors forward email to their employer's mail system for convenience | Client-confidential data forwarded to a third-party organisation's email infrastructure |
| **Executives** | Executives forward to personal devices or accounts when travelling | Board papers, M&A details and strategic plans leave the corporate boundary |
| **Leavers** | Auto-forwarding rules set before departure continue to operate after the account is disabled or converted | Ongoing data leakage from a mailbox the organisation believes is decommissioned |

**Common Governance Controls:**
- Block **automatic external forwarding** at the transport rule level in Exchange Online (outbound spam filter policy: automatic forwarding = off)
- Use **Microsoft Purview DLP policies** to detect and block emails containing sensitive data sent to external recipients
- Monitor forwarding rules via **Exchange Online mail flow rules** and alert on new auto-forward rules via **Microsoft Defender for Office 365**
- Audit existing inbox rules using **Exchange Online PowerShell** (`Get-InboxRule`) to identify active forwarding rules across all mailboxes
- Enforce **Conditional Access** to control email access on unmanaged devices via Outlook mobile app protection policies

---

### 3.3 Distribution Lists and Microsoft 365 Groups

Managing who can send to whom and controlling group email is a persistent challenge.

| Persona | Challenge | Impact |
|---------|-----------|--------|
| **Staff** | Anyone can create distribution lists; lists are never cleaned up and memberships become stale | All-staff emails sent to lists containing leavers; bounced emails; confusion over which list to use |
| **Students** | Cohort-based distribution lists are not updated when students change course, defer or withdraw | Students receive irrelevant communications; withdrawn students retain access to cohort groups |
| **Contractors** | Contractors are added to internal distribution lists and continue to receive emails after departure | Former contractors receive confidential internal communications |
| **External Guests** | Guests cannot be added to all distribution list types, leading to manual forwarding workarounds | Inconsistent communication; some external partners miss important updates |

**Common Governance Controls:**
- Migrate static distribution lists to **dynamic distribution groups** based on Entra ID attributes (department, role, student cohort, employment status)
- Restrict **who can create M365 groups** (which underpin Teams and group mailboxes) to approved users via Entra ID group creation policies
- Apply **send-on-behalf and send-as restrictions** to prevent impersonation via shared or group mailboxes
- Implement **group naming policies** and **group expiration policies** to maintain hygiene
- Use **Exchange Online address book policies** to segment the global address list by persona (e.g. staff see staff; students see students and relevant faculty)

---

### 3.4 Email Retention and Compliance

Email retention requirements vary by regulation, persona and content type.

| Persona | Challenge | Impact |
|---------|-----------|--------|
| **Staff** | Business email must be retained for regulatory periods (7 years for financial services, 3 years for GDPR defence) but users delete emails routinely | Compliance gaps; inability to produce email evidence for audits, litigation or subject access requests |
| **Students** | Student email may contain safeguarding-relevant content that must be retained, but GDPR data minimisation requires timely deletion | Tension between safeguarding retention and privacy rights; inconsistent retention across institutions |
| **Contractors** | Contractor mailboxes are deleted when the contract ends, losing potentially important project correspondence | Loss of business records; inability to reconstruct project decisions or communications |
| **Executives** | Executive email is frequently subject to litigation hold and regulatory preservation requirements | Executives expect privacy; legal hold requirements conflict with executive expectations of email management |
| **Leavers** | Leaver mailboxes must be retained for a defined period but licensing costs continue as long as the mailbox is active | Over-retention inflates licence costs; under-retention loses critical business records |

**Common Governance Controls:**
- Deploy **Microsoft Purview retention policies** for Exchange Online with persona-specific retention periods (e.g. 7 years for staff, 3 years for students, 1 year for contractors)
- Use **retention labels** for specific content types (legal correspondence, financial records, HR matters) with longer retention
- Convert leaver mailboxes to **inactive mailboxes** (removes licence requirement while preserving content under hold)
- Place **litigation holds** or **eDiscovery holds** on executive and key personnel mailboxes for legal preservation
- Use **Microsoft Purview eDiscovery (Premium)** for search, review and export of email content in response to legal or regulatory requests

---

### 3.5 Phishing, Impersonation and Email Security

Email remains the primary attack vector, and different personas face different threat profiles.

| Persona | Challenge | Impact |
|---------|-----------|--------|
| **Staff** | Employees receive phishing emails impersonating IT, HR or executives requesting credential entry or payments | Business email compromise (BEC); credential theft; fraudulent payments |
| **Students** | Students are targeted with financial aid scams, fake tuition refund emails and accommodation fraud | Financial loss; credential theft leading to identity abuse across institutional systems |
| **Contractors** | Attackers impersonate contractors to request payment changes or gain access to internal systems | Fraudulent invoice redirection; supply chain compromise |
| **External Guests** | Compromised guest accounts send phishing emails from within the tenant, bypassing external email filters | Internal trust exploited; phishing emails appear to come from a legitimate collaboration partner |
| **Executives** | CEO/CFO impersonation (whaling attacks) requesting urgent wire transfers or sensitive data | High-value financial fraud; reputational damage; data breach |
| **IT Administrators** | Admins targeted with credential phishing to gain tenant-level access | Full tenant compromise; attacker gains Global Admin or Exchange Admin access |

**Common Governance Controls:**
- Deploy **Microsoft Defender for Office 365 Plan 2** with anti-phishing policies, safe attachments and safe links
- Enable **impersonation protection** for executives and high-value users (mailbox intelligence and user impersonation detection)
- Configure **DMARC, DKIM and SPF** records to prevent domain spoofing
- Use **attack simulation training** in Defender for Office 365 to test and educate users on phishing recognition
- Deploy **Conditional Access** requiring phishing-resistant MFA (FIDO2, Windows Hello) for admin and executive accounts
- Enable **priority account protection** for executives and key personnel with enhanced detection and alerting

---

### 3.6 Shared Mailbox Governance

Shared mailboxes are widely used but often poorly governed.

| Persona | Challenge | Impact |
|---------|-----------|--------|
| **Staff** | Shared mailboxes (e.g. info@, hr@, finance@) have no clear owner and membership is not reviewed | Former employees retain access; no accountability for actions taken from the shared mailbox |
| **Students** | Student union or society mailboxes are handed over informally between student committees each year | Incoming committee members cannot access the mailbox; outgoing members retain access |
| **Contractors** | Contractors are granted shared mailbox access for project communications but access is not revoked at contract end | Former contractors continue to read and send email from organisational shared mailboxes |
| **IT Administrators** | Admins use shared mailboxes for system alerts and monitoring but mailboxes are not licensed or governed | Shared mailboxes exceed the 50 GB unlicensed limit; auto-replies and rules create mail loops |

**Common Governance Controls:**
- Assign a **designated owner** for every shared mailbox, documented in a mailbox register
- Review shared mailbox membership via **Entra ID Access Reviews** or manual quarterly reviews
- Remove shared mailbox access as part of the **JML leaver process** using Lifecycle Workflows or PowerShell automation
- Monitor shared mailbox size and activity via **Exchange Online admin reports**
- Apply **send-as auditing** to track which user sent from each shared mailbox for accountability

---

## 4. Cross-Cutting Governance Challenges

These challenges span both Teams and Email and affect all personas.

### 4.1 Information Barriers

| Persona | Challenge | Impact |
|---------|-----------|--------|
| **Staff** | Regulatory requirements (e.g. financial services Chinese walls) mandate separation between departments | Staff in investment banking must not communicate with retail banking staff via Teams or email |
| **Students** | Exam boards must be isolated from students during assessment periods | Students communicating with exam board members could compromise assessment integrity |
| **External Guests** | Guests from competing organisations should not see each other in the same tenant | Competitive intelligence leakage if guests from rivals can discover each other |

**Microsoft Solution:** Deploy **Microsoft Purview Information Barriers** to define segments (by department, role, organisation) and policies that block or allow communication between segments in Teams chat, calls, channels and Exchange Online.

---

### 4.2 Communication Compliance

| Persona | Challenge | Impact |
|---------|-----------|--------|
| **Staff** | Regulatory requirements mandate monitoring of business communications (e.g. FCA, SEC, MiFID II) | Failure to monitor can result in regulatory fines and enforcement action |
| **Students** | Safeguarding obligations require detection of bullying, self-harm indicators or inappropriate content | Institutions have a duty of care; failure to detect could have serious welfare consequences |
| **Executives** | Executive communications may be subject to insider trading monitoring or legal privilege review | High-risk communications require enhanced monitoring without impeding executive productivity |

**Microsoft Solution:** Deploy **Microsoft Purview Communication Compliance** with policies tailored to each persona:
- Financial regulation policies for staff in regulated roles
- Threat, harassment and self-harm detection for student populations
- Custom keyword and sensitive information type policies for executive communications

---

### 4.3 Conditional Access and Device Compliance

| Persona | Challenge | Impact |
|---------|-----------|--------|
| **Staff** | Staff access Teams and email from corporate-managed and personal devices | Data leakage through copy/paste, screenshots and downloads on unmanaged personal devices |
| **Students** | Students primarily use personal devices (BYOD) and cannot be required to enrol in device management | Cannot enforce device compliance; must rely on app-level protection |
| **Contractors** | Contractors use their own employer's devices which are not managed by the host organisation | Cannot enforce device compliance or endpoint protection on third-party devices |
| **External Guests** | Guests access Teams and email from entirely unmanaged external devices | No visibility into device security posture; highest risk of data exfiltration |

**Microsoft Solution:**
- Deploy **Conditional Access** policies per persona:
  - Staff: Require managed/compliant device or app protection policy
  - Students: Require app protection policy (Intune MAM without enrolment)
  - Contractors/Guests: Require MFA + app protection policy; block download on unmanaged devices via Defender for Cloud Apps session controls
  - Executives: Require compliant device + phishing-resistant MFA
- Use **Entra ID authentication strengths** to require different MFA methods by persona

---

### 4.4 Sensitivity Labels and Classification

| Persona | Challenge | Impact |
|---------|-----------|--------|
| **Staff** | Staff do not consistently label documents and emails, undermining the classification scheme | Sensitive data shared without encryption or access restrictions |
| **Students** | Students are unfamiliar with data classification concepts and do not apply labels | Assessment materials and personal data circulated without protection |
| **Contractors** | Contractors may apply labels from their own organisation or ignore the host organisation's labelling scheme | Inconsistent classification; contractor-generated content unprotected |
| **Executives** | Executives expect seamless workflows and resist manual labelling steps | Highly sensitive content left unclassified because labelling is seen as burdensome |

**Microsoft Solution:**
- Deploy **Microsoft Purview sensitivity labels** with mandatory labelling for emails and documents
- Enable **default labelling** to apply a baseline label (e.g. "Internal") automatically
- Use **auto-labelling policies** to detect and label content containing sensitive data types (PII, financial, health) without user intervention
- Apply **label-based encryption** and access restrictions to protect classified content regardless of where it is shared
- Use **label analytics** in the Purview compliance portal to monitor labelling adoption by persona and department

---

## 5. Education Sector — Additional Considerations

Education institutions face unique governance challenges due to the combination of staff and student personas, safeguarding obligations and academic requirements.

| Challenge | Description |
|-----------|-------------|
| **Student age and consent** | Students under 18 (or under 13 in some jurisdictions) require parental consent for data processing. M365 education tenants must configure **age-gating** and comply with COPPA, GDPR Article 8 and local equivalents |
| **Academic calendar lifecycle** | Teams and mailboxes should align with academic terms: created at enrolment, archived at end of term, deleted after a retention period. Manual processes cannot keep pace with cohort volumes |
| **Safeguarding and duty of care** | Institutions must monitor student communications for indicators of bullying, self-harm, radicalisation or abuse. **Communication Compliance** and **Microsoft Defender for Cloud Apps** anomaly detection support this |
| **Student-to-staff boundaries** | Inappropriate one-to-one communication between students and staff must be detectable. **Information barriers** and **communication compliance** address this |
| **Alumni email retention** | Graduates expect lifelong email access (common in higher education). Institutions must balance alumni engagement with licence cost and data retention obligations |
| **Classroom Teams governance** | Teams created via **School Data Sync (SDS)** or **Teams for Education** class teams have specific policies: student roles cannot delete channels, remove members or change team settings. These policies must be maintained as students and staff change |
| **Research data governance** | Research teams collaborate externally with sensitive data subject to funder requirements, ethical approvals and export controls. Teams and email governance must enforce data handling conditions per project |

---

## 6. Summary: Governance Challenge Matrix by Persona

| Challenge Area | Staff | Students | Contractors | External Guests | Executives | IT Admins | Alumni / Leavers |
|----------------|-------|----------|-------------|-----------------|------------|-----------|-------------------|
| **Team creation control** | Moderate | High | Low (restrict) | N/A (cannot create) | Low | Low | N/A |
| **Team ownership** | High | High | High | N/A | Moderate | High (avoid default ownership) | N/A |
| **Guest access management** | High | High | Moderate | High | High | Moderate | N/A |
| **DLP in Teams** | High | Moderate | High | High | Very High | Moderate | N/A |
| **Teams lifecycle** | Moderate | Very High | High | N/A | Low | Moderate | N/A |
| **Email forwarding control** | High | High | Very High | N/A | Very High | Very High | High |
| **Email retention** | High | Moderate | Moderate | N/A | Very High | Moderate | High |
| **Phishing protection** | High | High | High | Moderate | Very High | Very High | Low |
| **Shared mailbox governance** | High | Moderate | High | N/A | Low | High | N/A |
| **Information barriers** | Moderate | Moderate | Low | Moderate | High | Low | N/A |
| **Communication compliance** | Moderate (regulated) | High (safeguarding) | Low | Low | High (insider) | Low | N/A |
| **Sensitivity labelling** | High | Moderate | High | Moderate | Very High | Moderate | N/A |
| **Device compliance** | Moderate | Low (BYOD) | Low (third-party) | Low (unmanaged) | High | Very High | N/A |

---

## 7. Key Microsoft Technologies Reference

| Technology | Governance Area |
|------------|----------------|
| **Microsoft Purview DLP** | Data loss prevention across Teams chat, channels and Exchange Online |
| **Microsoft Purview Sensitivity Labels** | Classification and encryption of documents, emails and Teams |
| **Microsoft Purview Retention Policies** | Retention and deletion of Teams messages, email and files |
| **Microsoft Purview Communication Compliance** | Monitoring communications for regulatory, safeguarding and policy violations |
| **Microsoft Purview Information Barriers** | Segmentation of communication between user groups |
| **Microsoft Purview eDiscovery (Premium)** | Legal search, hold and export of Teams and email content |
| **Microsoft Defender for Office 365** | Anti-phishing, safe attachments, safe links and attack simulation |
| **Microsoft Defender for Cloud Apps** | Session controls, shadow IT discovery and anomaly detection |
| **Entra ID Conditional Access** | Per-persona access policies based on identity, device, location and risk |
| **Entra ID External Collaboration Settings** | Guest invitation controls, guest expiration and cross-tenant access |
| **Entra ID Access Reviews** | Periodic recertification of team membership, group membership and guest access |
| **Entra ID Lifecycle Workflows** | Automated onboarding, mover and offboarding actions for Teams and Exchange |
| **Exchange Online Transport Rules** | Mail flow control, external forwarding blocks and disclaimer insertion |
| **Teams Admin Policies** | Messaging, meeting, app and channel creation policies per user group |
| **School Data Sync (SDS)** | Automated class team creation and membership from student information systems |
| **Microsoft Intune / MAM** | Device compliance and app protection policies for BYOD scenarios |
