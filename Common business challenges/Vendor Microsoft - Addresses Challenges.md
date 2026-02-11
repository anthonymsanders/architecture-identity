# Vendor Microsoft — Addresses Challenges

**Date:** 11 February 2026

---

## Overview

This document describes how **Microsoft** technologies address the common identity challenges identified for small, medium and large organisations. All guidance assumes a **hybrid environment** using **Microsoft Entra ID** (formerly Azure AD) alongside **on-premises Active Directory Domain Services (AD DS)**, connected via **Microsoft Entra Connect** (formerly Azure AD Connect).

---

## Small Organisation Challenges (1–50 employees)

### 1. No Centralised Identity Provider

**Challenge:** User accounts are created independently in each SaaS application.

**Microsoft Hybrid Guidance:**

- Deploy **Microsoft Entra ID** as the single cloud identity provider for all SaaS applications. For small organisations, **Microsoft Entra ID Free** or **P1** licensing provides SSO and basic directory services.
- If on-premises Active Directory already exists (e.g. for file servers or legacy line-of-business applications), deploy **Microsoft Entra Connect** to synchronise on-premises AD identities to Entra ID, establishing a single identity across both environments.
- If no on-premises AD exists, use **Entra ID as the primary directory** and consider cloud-only identities managed entirely in the Microsoft 365 Admin Centre or Entra Admin Centre.
- Use the **Entra ID Enterprise Application Gallery** to integrate SaaS applications (e.g. Salesforce, Dropbox, Slack) with pre-built SSO connectors, eliminating per-application credentials.
- Enable **Microsoft Entra Connect cloud sync** as a lightweight alternative to the full Entra Connect agent for simple single-forest topologies.

**Key Microsoft Technologies:**
| Technology | Purpose |
|------------|---------|
| Microsoft Entra ID | Cloud identity provider and SSO platform |
| Microsoft Entra Connect / Cloud Sync | Hybrid identity synchronisation between AD DS and Entra ID |
| Enterprise Application Gallery | Pre-integrated SSO connectors for thousands of SaaS apps |
| Microsoft 365 Admin Centre | Simplified user and licence management for small organisations |

---

### 2. Shared Accounts

**Challenge:** Teams share logins for cost reasons or convenience.

**Microsoft Hybrid Guidance:**

- Assign **individual Microsoft Entra ID accounts** to every user, even for shared resources. Microsoft 365 Business Basic licensing provides low-cost per-user identities.
- For genuinely shared resources such as team inboxes, use **Microsoft 365 Shared Mailboxes** (no additional licence required) rather than sharing a single user account.
- Use **Microsoft Entra ID Security Groups** to grant multiple individuals access to the same resources without sharing credentials.
- For shared workstations, configure **Windows shared device mode** or use **Microsoft Entra ID multi-user sign-in** on shared devices so each person authenticates individually.
- In Active Directory, audit for accounts where multiple users share credentials using **Entra ID Sign-in Logs** to detect concurrent sessions from different locations.

**Key Microsoft Technologies:**
| Technology | Purpose |
|------------|---------|
| Microsoft 365 Shared Mailboxes | Shared email without shared credentials |
| Entra ID Security Groups | Group-based access to resources |
| Entra ID Sign-in Logs | Detect concurrent or anomalous sign-in patterns |
| Shared Device Mode | Individual sign-in on shared hardware |

---

### 3. Weak or No MFA Adoption

**Challenge:** Multi-factor authentication is often not enforced.

**Microsoft Hybrid Guidance:**

- Enable **Microsoft Entra ID Security Defaults** as an immediate baseline. This enforces MFA registration for all users and prompts MFA for administrative actions and risky sign-ins at no additional licence cost.
- For more granular control, deploy **Entra ID Conditional Access policies** (requires P1 licensing) to enforce MFA based on user, application, location, device state or risk level.
- Use **Microsoft Authenticator app** as the primary MFA method. It supports push notifications, number matching and passwordless phone sign-in.
- For hybrid environments, ensure **Entra ID MFA** protects cloud application access while on-premises resources can be protected using the **Microsoft Entra multifactor authentication NPS extension** for VPN and Remote Desktop Gateway.
- Plan a migration towards **passwordless authentication** using FIDO2 security keys, Windows Hello for Business, or Microsoft Authenticator passwordless sign-in.

**Key Microsoft Technologies:**
| Technology | Purpose |
|------------|---------|
| Security Defaults | Zero-cost baseline MFA enforcement |
| Conditional Access | Risk-based, granular MFA policies |
| Microsoft Authenticator | Push-based MFA and passwordless authentication |
| NPS Extension for MFA | Extend Entra MFA to on-premises VPN and RDP |
| Windows Hello for Business | Passwordless sign-in for Windows devices |

---

### 4. No Joiners/Movers/Leavers Process

**Challenge:** When staff leave, accounts are often not disabled promptly.

**Microsoft Hybrid Guidance:**

- Use the **Microsoft 365 Admin Centre** to disable user accounts immediately upon departure. When Entra Connect is deployed, disabling the account in on-premises AD automatically synchronises the disabled state to Entra ID (and vice versa with writeback).
- Implement **Entra ID Lifecycle Workflows** (requires Entra ID Governance licence) to automate pre-hire, onboarding and offboarding tasks such as generating temporary access passes, sending welcome emails and revoking access.
- For hybrid environments, when an account is disabled in AD DS, Entra Connect syncs the disabled status within the next sync cycle (default 30 minutes). Use **Entra Connect on-demand provisioning** to force immediate synchronisation for urgent offboarding.
- Configure **Entra ID Continuous Access Evaluation (CAE)** to ensure that token revocation takes effect in near real-time when an account is disabled, rather than waiting for token expiry.
- Use **Entra ID access reviews** to periodically verify that all active accounts correspond to current employees.

**Key Microsoft Technologies:**
| Technology | Purpose |
|------------|---------|
| Entra ID Lifecycle Workflows | Automated onboarding and offboarding task orchestration |
| Entra Connect on-demand sync | Immediate hybrid sync for urgent account changes |
| Continuous Access Evaluation (CAE) | Near real-time token revocation on account disable |
| Entra ID Access Reviews | Periodic validation of active accounts |

---

### 5. Password Reuse

**Challenge:** Without a password manager or SSO, users reuse passwords across accounts.

**Microsoft Hybrid Guidance:**

- Deploy **Microsoft Entra ID SSO** across all integrated applications so users authenticate once and access all resources with a single credential, eliminating the need to create and remember separate passwords.
- Enable **Microsoft Entra Password Protection** to block commonly used and organisation-specific weak passwords in both Entra ID and on-premises Active Directory (via the Password Protection DC Agent and Proxy).
- Activate **Entra ID Identity Protection** (requires P2 licensing) to detect credentials that have appeared in known breach databases. Users with leaked credentials are automatically prompted to change their passwords.
- On-premises AD password policies should be aligned with Entra ID by deploying the **Entra Password Protection proxy** on-premises, ensuring the same banned password list applies across hybrid environments.
- Accelerate adoption of **passwordless authentication** (FIDO2, Windows Hello for Business, Microsoft Authenticator) to remove passwords entirely.

**Key Microsoft Technologies:**
| Technology | Purpose |
|------------|---------|
| Entra ID SSO | Single sign-on to eliminate per-app passwords |
| Entra Password Protection | Banned password lists in cloud and on-premises AD |
| Entra ID Identity Protection | Leaked credential detection and automated remediation |
| FIDO2 / Windows Hello for Business | Passwordless authentication |

---

### 6. Shadow IT

**Challenge:** Staff adopt tools and services without IT oversight.

**Microsoft Hybrid Guidance:**

- Deploy **Microsoft Defender for Cloud Apps** (formerly Microsoft Cloud App Security) to discover and assess cloud applications in use across the organisation by analysing firewall and proxy logs.
- Use the **Cloud App Catalogue** in Defender for Cloud Apps to risk-score discovered applications and identify unsanctioned SaaS tools.
- Create **Entra ID Conditional Access policies** that block sign-in to unsanctioned applications or require MFA and compliant devices for accessing sanctioned ones.
- Establish an **Entra ID Application Registration** and consent governance process. Configure **admin consent workflow** so that users can request access to new applications, which are reviewed and approved centrally before being added to the enterprise app catalogue.
- Use **Entra ID App Governance** to monitor OAuth app permissions and detect overprivileged or suspicious third-party applications that users have consented to.

**Key Microsoft Technologies:**
| Technology | Purpose |
|------------|---------|
| Microsoft Defender for Cloud Apps | Shadow IT discovery and cloud app risk assessment |
| Conditional Access | Block or control access to unsanctioned applications |
| Admin Consent Workflow | Centralised approval for new application access |
| App Governance | Monitor and control OAuth app permissions |

---

### 7. No Access Reviews

**Challenge:** There is no formal process to review who has access to what.

**Microsoft Hybrid Guidance:**

- Implement **Entra ID Access Reviews** (requires Entra ID Governance or P2 licence) to create recurring review campaigns where resource owners or managers certify that user access is still appropriate.
- Configure access reviews for **Entra ID group memberships**, **Entra ID application assignments**, and **Entra ID role assignments** (including privileged roles).
- Set reviews to **auto-apply results** so that access is automatically removed for users whose access is not confirmed by the reviewer within the review period.
- For hybrid environments, group membership changes made via Entra ID access reviews can be written back to on-premises AD groups using **Entra ID group writeback**.
- Use the **Entra ID Governance dashboard** for a consolidated view of access review status, pending reviews and compliance posture.

**Key Microsoft Technologies:**
| Technology | Purpose |
|------------|---------|
| Entra ID Access Reviews | Recurring access certification campaigns |
| Entra ID Governance Dashboard | Centralised compliance and review status |
| Group Writeback | Sync cloud access review decisions to on-premises AD groups |
| Auto-apply Results | Automated access removal for unconfirmed entitlements |

---

### 8. Owner Dependency

**Challenge:** A single individual holds all administrative credentials.

**Microsoft Hybrid Guidance:**

- Assign at least **two Global Administrator accounts** in Entra ID to eliminate single points of failure. Microsoft recommends two to four Global Admins, with at least one being a **break-glass (emergency access) account** that is excluded from Conditional Access policies and MFA.
- Break-glass accounts should use **long, complex passwords** stored securely offline, should not be associated with any individual, and should be monitored with **Entra ID audit log alerts** that trigger on any sign-in.
- Apply the **principle of least privilege** by using **Entra ID built-in roles** (e.g. User Administrator, Application Administrator, Helpdesk Administrator) rather than granting Global Admin to all IT staff.
- Enable **Entra ID Privileged Identity Management (PIM)** (requires P2 licence) to make admin role assignments **eligible** rather than permanent. Admins activate their role on-demand with time-bound access and approval workflows.
- In on-premises AD, follow the **Microsoft tiered administration model** and use **Protected Users security group** and **Authentication Policy Silos** to safeguard privileged accounts.

**Key Microsoft Technologies:**
| Technology | Purpose |
|------------|---------|
| Break-glass Accounts | Emergency admin access immune to policy lockouts |
| Entra ID Built-in Roles | Least-privilege role assignment |
| Privileged Identity Management (PIM) | Just-in-time, time-bound privileged role activation |
| Protected Users Group (AD DS) | Hardened on-premises privileged account protection |
| Entra ID Audit Logs & Alerts | Monitoring of administrative account activity |

---

## Medium Organisation Challenges (50–500 employees)

### 1. Identity Silos

**Challenge:** Different departments manage their own user directories.

**Microsoft Hybrid Guidance:**

- Consolidate all on-premises Active Directory forests and domains into a single **Entra ID tenant** using Microsoft Entra Connect. Entra Connect supports **multi-forest synchronisation**, allowing multiple AD forests to sync into one Entra ID directory.
- Where full forest consolidation is not yet possible, use **Entra Connect with multiple connector configurations** to sync identities from each forest while applying **join rules** to match and merge duplicate accounts.
- Establish Entra ID as the **single source of truth** for cloud application access. All SaaS and Microsoft 365 access should be governed through a single tenant, even if multiple on-premises directories exist.
- Use **Entra ID Administrative Units** to delegate management of specific user populations to departmental IT teams without giving them tenant-wide permissions, preserving autonomy while centralising the directory.
- Plan and execute **AD forest consolidation** using the **Active Directory Migration Tool (ADMT)** for long-term simplification of on-premises infrastructure.

**Key Microsoft Technologies:**
| Technology | Purpose |
|------------|---------|
| Entra Connect Multi-Forest Sync | Synchronise multiple AD forests into a single Entra ID tenant |
| Administrative Units | Delegated departmental management within a single tenant |
| ADMT | Active Directory forest and domain consolidation |
| Join Rules (Entra Connect) | Merge duplicate identities from multiple sources |

---

### 2. Inconsistent Provisioning

**Challenge:** No standardised process for granting access.

**Microsoft Hybrid Guidance:**

- Deploy **Entra ID Entitlement Management** (part of Entra ID Governance) to create **Access Packages** — pre-defined bundles of group memberships, application assignments and role assignments that users can request through a self-service portal.
- Define **approval workflows** within Access Packages so that requests are routed to the appropriate manager or resource owner for approval before access is granted.
- Use **Entra ID automatic user provisioning (SCIM)** to provision and deprovision user accounts in integrated SaaS applications automatically when group memberships change.
- For hybrid provisioning, when a new user is created in on-premises AD and synced to Entra ID via Entra Connect, configure **Entra ID HR-driven provisioning** (from Workday or SAP SuccessFactors) to automate account creation in AD based on HR system events.
- Implement **Entra ID dynamic groups** to automatically assign users to groups based on attributes (e.g. department, job title, location), ensuring consistent baseline access without manual intervention.

**Key Microsoft Technologies:**
| Technology | Purpose |
|------------|---------|
| Entitlement Management / Access Packages | Self-service access request with approval workflows |
| SCIM Automatic Provisioning | Automated user provisioning to SaaS applications |
| HR-driven Provisioning | Automated AD/Entra account creation from HR systems |
| Dynamic Groups | Attribute-based automatic group membership |

---

### 3. Privilege Creep

**Challenge:** Old access is rarely revoked when employees change roles.

**Microsoft Hybrid Guidance:**

- Use **Entra ID Access Reviews** to run quarterly or role-change-triggered reviews of group memberships, application assignments and directory role assignments.
- Configure **Access Package assignment policies** with expiry dates in Entitlement Management, so that access automatically expires and must be re-requested or re-approved.
- Implement **Entra ID dynamic groups** based on HR attributes (department, job title, cost centre). When an employee moves roles and their HR attributes update, they are automatically removed from old groups and added to new ones.
- Deploy **Entra ID Lifecycle Workflows** to trigger mover workflows when user attributes change, automating the removal of old access and assignment of new access.
- For privileged roles, enforce **time-bound assignments** via **Privileged Identity Management (PIM)** so that elevated access automatically expires and must be explicitly re-activated.

**Key Microsoft Technologies:**
| Technology | Purpose |
|------------|---------|
| Access Reviews | Periodic certification of existing access |
| Access Package Expiry | Automatic access expiration with renewal workflows |
| Dynamic Groups | Automatic group reassignment on attribute change |
| Lifecycle Workflows (Mover) | Automated access adjustment when role changes |
| PIM Time-bound Assignments | Auto-expiring privileged role assignments |

---

### 4. Contractor and Third-Party Access

**Challenge:** Growing use of external contractors without a clear identity lifecycle.

**Microsoft Hybrid Guidance:**

- Use **Microsoft Entra External ID (B2B collaboration)** to invite external contractors and partners as guest users in the Entra ID tenant. Guests authenticate using their own organisation's identity provider (or a one-time passcode) and are granted access only to specific resources.
- Create dedicated **Access Packages** in Entitlement Management for external users, with **sponsor approval**, **time-limited assignments** and **mandatory access reviews** at regular intervals.
- Apply **Conditional Access policies** specific to guest users that enforce MFA, limit access to compliant or managed devices, and restrict access to only the applications they need.
- Configure **Entra ID cross-tenant access settings** to control which external organisations can collaborate and what level of trust is extended to their MFA and device claims.
- Set **automatic guest account expiration** and use Entra ID access reviews to periodically ask sponsors to re-confirm whether external access is still needed. Unconfirmed guests are automatically disabled and deleted.

**Key Microsoft Technologies:**
| Technology | Purpose |
|------------|---------|
| Entra External ID (B2B) | Guest access using external identity providers |
| Entitlement Management for Externals | Time-bound, sponsor-approved external access packages |
| Conditional Access for Guests | MFA and device compliance policies for external users |
| Cross-tenant Access Settings | Control B2B collaboration trust and policy |
| Guest Account Expiration | Automatic lifecycle management for external identities |

---

### 5. Compliance Gaps

**Challenge:** Regulations require demonstrable access controls that informal processes cannot satisfy.

**Microsoft Hybrid Guidance:**

- Use the **Microsoft Entra ID Governance** suite (Access Reviews, Entitlement Management, Lifecycle Workflows) to establish auditable, repeatable access management processes that satisfy ISO 27001, GDPR and Cyber Essentials requirements.
- Generate **Entra ID audit logs** and **sign-in logs** and export them to **Microsoft Sentinel** or a SIEM for long-term retention and compliance reporting. Entra ID retains sign-in logs for 30 days (P1/P2) by default; longer retention requires export.
- Use **Entra ID Conditional Access** to enforce policies that align with compliance frameworks (e.g. requiring MFA for all users, blocking legacy authentication, requiring compliant devices).
- Deploy **Microsoft Purview Compliance Manager** to assess compliance posture across Microsoft 365 workloads and generate improvement action plans aligned with regulatory frameworks.
- For on-premises AD, enable **Advanced Audit Policies** and forward AD security event logs to Microsoft Sentinel via the **Azure Monitor Agent** for unified hybrid audit and compliance reporting.

**Key Microsoft Technologies:**
| Technology | Purpose |
|------------|---------|
| Entra ID Governance Suite | Auditable access management lifecycle |
| Entra Audit & Sign-in Logs | Activity logging for compliance evidence |
| Microsoft Sentinel | SIEM for long-term log retention and compliance alerting |
| Microsoft Purview Compliance Manager | Compliance posture assessment and improvement tracking |
| Azure Monitor Agent | Forward on-premises AD logs to Sentinel |

---

### 6. Lack of SSO Across All Applications

**Challenge:** SSO is implemented for some applications but not others.

**Microsoft Hybrid Guidance:**

- Systematically onboard all applications to **Entra ID SSO** using the Enterprise Application Gallery (pre-integrated) or **custom SAML/OIDC registrations** for applications not in the gallery.
- For on-premises web applications that do not support modern authentication, deploy **Microsoft Entra Application Proxy** to publish them externally with Entra ID pre-authentication, SSO and Conditional Access — without opening inbound firewall ports.
- For on-premises applications using **Kerberos** or **Windows Integrated Authentication (WIA)**, Entra Application Proxy provides **Kerberos Constrained Delegation (KCD)** to seamlessly pass Entra ID authentication through to the on-premises application.
- For legacy applications using **header-based authentication**, use Entra Application Proxy with partner integrations (e.g. F5, Zscaler) or the built-in header-based SSO capability.
- Use the **Entra ID application activity report** to identify applications with low adoption or where users are still signing in with local credentials, and prioritise these for SSO onboarding.

**Key Microsoft Technologies:**
| Technology | Purpose |
|------------|---------|
| Enterprise Application Gallery | Pre-built SSO connectors for thousands of SaaS apps |
| Entra Application Proxy | Secure remote access and SSO for on-premises web apps |
| Kerberos Constrained Delegation (KCD) | SSO pass-through for Kerberos-based on-premises apps |
| Header-based SSO | SSO for legacy header-authenticated applications |
| Application Activity Report | Identify apps not yet onboarded to SSO |

---

### 7. Weak Privileged Access Management

**Challenge:** Admin and service accounts are not adequately secured.

**Microsoft Hybrid Guidance:**

- Deploy **Microsoft Entra Privileged Identity Management (PIM)** to convert all permanent administrative role assignments to **eligible** assignments. Admins must explicitly activate their role, with time limits, justification and optional approval.
- Enable **PIM for Groups** to extend just-in-time activation to security group memberships that grant elevated access (e.g. Domain Admins synced from on-premises AD).
- Implement **Conditional Access policies for privileged roles** requiring phishing-resistant MFA (FIDO2 or Windows Hello for Business), compliant devices and trusted network locations.
- For on-premises AD, deploy **Microsoft Identity Manager (MIM) Privileged Access Management** or implement the **Enhanced Security Admin Environment (ESAE)** tiered administration model to isolate Tier 0 (domain controllers, AD infrastructure) from standard administration.
- Enforce **Entra ID Workload Identity protection** for service accounts and managed identities, applying Conditional Access to non-human identities where supported.

**Key Microsoft Technologies:**
| Technology | Purpose |
|------------|---------|
| Privileged Identity Management (PIM) | Just-in-time, approval-based role activation |
| PIM for Groups | JIT activation for group-based elevated access |
| Conditional Access for Admins | Phishing-resistant MFA and device compliance for privileged users |
| Tiered Administration Model (ESAE) | On-premises AD privilege isolation |
| Workload Identity Protection | Conditional Access for service accounts and managed identities |

---

### 8. Incomplete Offboarding

**Challenge:** Leavers retain access because there is no single source of truth for entitlements.

**Microsoft Hybrid Guidance:**

- Establish the **HR system** (Workday, SAP SuccessFactors) as the authoritative source for employment status, connected to Entra ID via **HR-driven provisioning**. When an employee is terminated in HR, the Entra ID account is automatically disabled.
- In hybrid environments, HR-driven provisioning can create and disable accounts in **on-premises AD**, which then syncs to Entra ID via Entra Connect. This ensures both environments are updated from a single trigger.
- Deploy **Entra ID Lifecycle Workflows** with a **leaver workflow** that executes on the employee's last day: disabling the account, revoking all refresh tokens, removing group and application assignments, converting the mailbox to a shared mailbox and notifying the manager.
- Enable **Continuous Access Evaluation (CAE)** to ensure that sessions in progress are terminated within minutes of account disablement, rather than waiting for token expiry.
- Conduct post-offboarding **access reviews** targeting recently disabled accounts to verify that all access has been fully revoked across all connected systems.

**Key Microsoft Technologies:**
| Technology | Purpose |
|------------|---------|
| HR-driven Provisioning | Automated account disable triggered by HR system |
| Lifecycle Workflows (Leaver) | Orchestrated offboarding task execution |
| Continuous Access Evaluation (CAE) | Immediate session termination on account disable |
| Entra Connect Sync | Propagate disable status between AD and Entra ID |
| Post-offboarding Access Reviews | Verify complete access revocation |

---

### 9. Limited Visibility and Reporting

**Challenge:** No consolidated view of who has access to what.

**Microsoft Hybrid Guidance:**

- Use the **Entra ID Admin Centre** as the centralised dashboard for viewing user accounts, group memberships, application assignments, role assignments and sign-in activity.
- Export **Entra ID audit logs** and **sign-in logs** to **Microsoft Sentinel** for long-term storage, cross-correlation with on-premises AD logs, and custom compliance dashboards.
- Deploy the **Microsoft Sentinel Identity Threat Detection** solution (formerly Azure AD Identity Protection connector) to correlate identity events across Entra ID and on-premises AD.
- Use **Entra ID workbooks** (built on Azure Monitor Workbooks) for pre-built and customisable reports covering sign-in patterns, Conditional Access policy impact, provisioning activity and more.
- For on-premises AD visibility, deploy **Microsoft Defender for Identity** (formerly Azure ATP) which monitors AD Domain Controller traffic to detect suspicious identity-related activity and lateral movement.

**Key Microsoft Technologies:**
| Technology | Purpose |
|------------|---------|
| Entra Admin Centre | Centralised identity dashboard |
| Microsoft Sentinel | SIEM with identity-focused analytics and dashboards |
| Entra ID Workbooks | Customisable identity reports and visualisations |
| Defender for Identity | On-premises AD threat detection and monitoring |
| Unified Audit Logs | Correlated cloud and on-premises identity event data |

---

### 10. Growing Machine Identity Footprint

**Challenge:** Increasing use of APIs, automation and cloud services creates unmanaged non-human identities.

**Microsoft Hybrid Guidance:**

- Use **Managed Identities** (system-assigned and user-assigned) for Azure workloads instead of storing credentials. Managed Identities authenticate to Azure resources without secrets, keys or certificates.
- For non-Azure workloads and on-premises applications, use **Microsoft Entra Workload ID** with **federated identity credentials** to eliminate stored secrets by trusting external identity providers (e.g. GitHub Actions, Kubernetes).
- Inventory existing service accounts and application registrations using the **Entra ID App Registrations** blade and **Enterprise Applications** blade. Identify applications with expiring or long-lived secrets and certificates.
- Apply **Entra ID Conditional Access for Workload Identities** (requires Workload Identities Premium licence) to enforce location-based restrictions on service principals.
- For on-premises AD service accounts, migrate to **Group Managed Service Accounts (gMSA)** which provide automatic password rotation managed by AD, eliminating manual password management.

**Key Microsoft Technologies:**
| Technology | Purpose |
|------------|---------|
| Managed Identities | Credential-free authentication for Azure workloads |
| Workload ID Federation | Secret-free authentication for external workloads |
| Conditional Access for Workload Identities | Policy enforcement for non-human identities |
| App Registration & Secret Monitoring | Inventory and lifecycle management of app credentials |
| Group Managed Service Accounts (gMSA) | Automatic password rotation for on-premises service accounts |

---

## Large Organisation Challenges (500+ employees)

### 1. Identity Governance at Scale

**Challenge:** Managing hundreds of thousands of identities across multiple systems requires mature tooling and processes.

**Microsoft Hybrid Guidance:**

- Deploy the full **Microsoft Entra ID Governance** suite comprising Entitlement Management, Access Reviews, Lifecycle Workflows and Privileged Identity Management as the enterprise identity governance platform.
- Use **Entitlement Management catalogs** to delegate governance to business units. Each business unit manages its own catalog of resources and access packages while central IT retains oversight and policy control.
- Implement **Entra ID HR-driven provisioning** at scale, connecting to enterprise HR platforms (Workday, SAP SuccessFactors) to automate the creation, update and deactivation of identities across the entire employee population.
- Deploy **Entra Connect with staging servers** for high availability and use **Entra Connect Health** to monitor synchronisation health, latency and errors across large-scale hybrid environments.
- Use **Microsoft Graph API** and **Entra ID Governance APIs** to build custom automation, reporting and integration with enterprise ITSM platforms (ServiceNow, BMC) for identity lifecycle orchestration.

**Key Microsoft Technologies:**
| Technology | Purpose |
|------------|---------|
| Entra ID Governance Suite | Enterprise-scale identity governance platform |
| Entitlement Management Catalogs | Delegated, business-unit-level governance |
| HR-driven Provisioning at Scale | Automated lifecycle management from HR systems |
| Entra Connect Health | Hybrid sync monitoring and alerting |
| Microsoft Graph API | Custom automation and ITSM integration |

---

### 2. Merger and Acquisition Integration

**Challenge:** Acquired entities bring their own identity infrastructure that must be rationalised.

**Microsoft Hybrid Guidance:**

- Establish **Entra ID cross-tenant synchronisation** to sync user identities from the acquired organisation's tenant into the parent tenant, enabling access to shared resources during the integration period.
- Use **Entra ID cross-tenant access settings** to configure B2B collaboration trust between tenants, allowing users in the acquired tenant to access parent applications with their existing credentials and MFA.
- Deploy **Entra ID multi-tenant organisation (MTO)** settings for seamless collaboration across tenants, including shared people search and Teams interop, while maintaining separate tenant boundaries during transition.
- Plan long-term tenant consolidation using **Entra ID tenant-to-tenant migration** tooling, migrating users, groups, application registrations and Conditional Access policies to the target tenant.
- For on-premises AD, use **AD forest trusts** during the transition period and plan eventual **forest consolidation using ADMT** once business integration is complete.

**Key Microsoft Technologies:**
| Technology | Purpose |
|------------|---------|
| Cross-tenant Synchronisation | Sync identities between acquiring and acquired tenants |
| Cross-tenant Access Settings | B2B trust and policy between tenants |
| Multi-tenant Organisation (MTO) | Seamless collaboration across organisational tenants |
| Tenant-to-tenant Migration | Long-term tenant consolidation tooling |
| AD Forest Trusts / ADMT | On-premises AD integration and consolidation |

---

### 3. Regulatory Complexity

**Challenge:** Operating across jurisdictions requires compliance with multiple overlapping regulations.

**Microsoft Hybrid Guidance:**

- Use **Microsoft Entra ID data residency controls** and **multi-geo capabilities** in Microsoft 365 to ensure identity data is stored in the correct geographic region to meet data sovereignty requirements (e.g. EU Data Boundary).
- Deploy **Conditional Access named locations** and **GPS-based location policies** to enforce jurisdiction-specific access controls (e.g. users in the EU can only access EU-hosted applications).
- Configure **Entra ID audit and sign-in log export** to **Microsoft Sentinel** with region-specific workspaces for localised log retention in compliance with national regulations.
- Use **Microsoft Purview Compliance Manager** with assessment templates for GDPR, HIPAA, SOX, NIS2 and other frameworks to track compliance posture and generate regulator-ready reports.
- Implement **Entra ID Terms of Use** to present jurisdiction-specific acceptable use policies that users must accept before accessing resources, with evidence of acceptance retained for compliance.

**Key Microsoft Technologies:**
| Technology | Purpose |
|------------|---------|
| Data Residency / EU Data Boundary | Geographic control of identity data storage |
| Conditional Access Named Locations | Jurisdiction-specific access policies |
| Microsoft Sentinel (Multi-workspace) | Region-specific log retention and compliance reporting |
| Purview Compliance Manager | Multi-framework compliance assessment |
| Terms of Use | Jurisdiction-specific policy acceptance and evidence |

---

### 4. Orphaned and Dormant Accounts

**Challenge:** The sheer volume of accounts makes it difficult to identify unused accounts.

**Microsoft Hybrid Guidance:**

- Use the **Entra ID inactive users report** (Entra ID Governance) to identify accounts that have not signed in within a defined period. Configure policies to flag accounts inactive for 30, 60 or 90 days.
- Deploy **Entra ID Access Reviews** targeting inactive users. Configure reviews to auto-disable or auto-delete accounts that are not recertified by their manager.
- Enable **Entra ID Identity Protection risk detections** which flag accounts showing no activity as potentially orphaned, and correlate with HR system data to confirm employment status.
- For on-premises AD, use the **lastLogonTimestamp** attribute (replicated across domain controllers) to identify dormant AD accounts. Automate detection using PowerShell scripts scheduled via Azure Automation, reporting into Sentinel.
- Implement **Entra ID Lifecycle Workflows** with a **leaver workflow triggered by account inactivity**, automatically executing offboarding steps when an account has been inactive beyond the defined threshold.

**Key Microsoft Technologies:**
| Technology | Purpose |
|------------|---------|
| Inactive Users Report | Identify accounts with no recent sign-in activity |
| Access Reviews (Inactive Users) | Automated review and removal of dormant accounts |
| lastLogonTimestamp (AD DS) | On-premises dormant account detection |
| Lifecycle Workflows (Inactivity) | Automated offboarding triggered by inactivity |
| Azure Automation | Scheduled PowerShell scripts for hybrid account hygiene |

---

### 5. Excessive Standing Privileges

**Challenge:** Users and service accounts retain persistent elevated access.

**Microsoft Hybrid Guidance:**

- Deploy **Microsoft Entra Privileged Identity Management (PIM)** across all Entra ID directory roles, Azure resource roles and PIM for Groups. Convert all permanent privileged assignments to **eligible** assignments requiring on-demand activation.
- Configure PIM activation to require **justification**, **MFA re-authentication**, and **approval** from a designated approver for high-impact roles (Global Admin, Exchange Admin, Security Admin).
- Set **maximum activation durations** (e.g. 4-8 hours) so that elevated access automatically expires. Use **PIM alerts** to monitor for anomalies such as roles not being used or excessive activations.
- For on-premises AD, implement **Privileged Access Management (PAM)** using Microsoft Identity Manager (MIM) to provide time-bound membership of privileged AD groups (e.g. Domain Admins, Schema Admins).
- Use **Entra ID Conditional Access authentication context** to require step-up authentication when accessing sensitive resources, even for users who have already activated a privileged role.

**Key Microsoft Technologies:**
| Technology | Purpose |
|------------|---------|
| PIM (Directory, Azure, Groups) | Just-in-time, time-bound privileged access |
| PIM Approval Workflows | Multi-party approval for high-impact role activation |
| PIM Alerts | Monitoring and anomaly detection for privilege usage |
| MIM PAM | Time-bound on-premises AD privileged group membership |
| Authentication Context | Step-up authentication for sensitive resource access |

---

### 6. Decentralised Application Ownership

**Challenge:** Business units own applications independently with inconsistent identity integration.

**Microsoft Hybrid Guidance:**

- Establish a centralised **application onboarding standard** requiring all applications to integrate with Entra ID for authentication and authorisation, regardless of which business unit owns them.
- Use **Entra ID Entitlement Management catalogs** to delegate day-to-day access management to application owners within their business unit, while central IT controls the tenant-wide identity policies (Conditional Access, MFA, session controls).
- Require all application registrations to go through a governed process using **Entra ID App Registration policies** that restrict who can register applications and limit consent permissions.
- Deploy **Entra ID Conditional Access** at the tenant level with baseline policies that apply to all applications (e.g. require MFA, block legacy authentication, require compliant devices), ensuring a consistent security posture regardless of application ownership.
- Use **Entra ID Application Governance** and **Microsoft Defender for Cloud Apps** to monitor application behaviour, detect anomalous activity and enforce data loss prevention policies across all integrated applications.

**Key Microsoft Technologies:**
| Technology | Purpose |
|------------|---------|
| Application Onboarding Standards | Mandatory Entra ID integration requirements |
| Entitlement Management Catalogs | Delegated access management per business unit |
| App Registration Policies | Controlled application registration and consent |
| Tenant-wide Conditional Access | Consistent baseline security for all applications |
| App Governance / Defender for Cloud Apps | Application behaviour monitoring and DLP |

---

### 7. Machine Identity Explosion

**Challenge:** Thousands of service accounts, API keys and certificates are poorly tracked and rarely rotated.

**Microsoft Hybrid Guidance:**

- Conduct a full inventory of non-human identities using the **Entra ID Enterprise Applications** blade (service principals), **App Registrations** (application objects) and on-premises AD service accounts.
- Migrate Azure workloads to **Managed Identities** (system-assigned or user-assigned) to eliminate stored credentials entirely. Managed Identities are automatically managed by the platform with no secrets to rotate.
- For cross-platform and third-party workloads, implement **Workload Identity Federation** using federated identity credentials, eliminating the need for stored secrets.
- Deploy **Entra ID Workload Identities Premium** to apply Conditional Access policies to service principals, detect compromised workload identities via Identity Protection and review app role assignments via Access Reviews.
- On-premises, migrate service accounts from standard AD accounts to **Group Managed Service Accounts (gMSA)** or **Delegated Managed Service Accounts (dMSA)** for automatic credential rotation. Monitor service account activity using **Defender for Identity**.

**Key Microsoft Technologies:**
| Technology | Purpose |
|------------|---------|
| Managed Identities | Credential-free Azure workload authentication |
| Workload Identity Federation | Secret-free cross-platform authentication |
| Workload Identities Premium | Conditional Access, Identity Protection and Access Reviews for non-human identities |
| gMSA / dMSA | Automatic credential rotation for on-premises service accounts |
| Defender for Identity | Service account monitoring in on-premises AD |

---

### 8. Complex Access Certification

**Challenge:** Periodic access reviews become unmanageable at scale.

**Microsoft Hybrid Guidance:**

- Deploy **Entra ID Access Reviews** with **multi-stage reviews**: first-stage review by the user's direct manager, second-stage review by the resource owner, ensuring informed decisions at each level.
- Use **machine learning-assisted recommendations** in Access Reviews, which analyse user sign-in activity and peer group access patterns to recommend approval or denial, reducing reviewer burden.
- Scope access reviews using **Access Packages** in Entitlement Management rather than reviewing raw group memberships. Reviewing whether a user should retain an access package (which bundles related entitlements) is more meaningful than reviewing individual group memberships.
- Implement **Access Review decision helpers** that display the last sign-in date for each user and whether peers in the same role have similar access, helping reviewers make informed decisions quickly.
- Configure **auto-apply and auto-removal** for unconfirmed access to ensure that reviews result in concrete action even if reviewers do not respond within the review period.

**Key Microsoft Technologies:**
| Technology | Purpose |
|------------|---------|
| Multi-stage Access Reviews | Layered review by manager then resource owner |
| ML-assisted Recommendations | AI-driven approval/denial suggestions |
| Access Package Reviews | Review meaningful bundles rather than raw entitlements |
| Decision Helpers | Last sign-in and peer comparison data for reviewers |
| Auto-apply / Auto-removal | Enforce outcomes when reviewers do not respond |

---

### 9. Identity as an Attack Surface

**Challenge:** Threat actors target identity infrastructure as a primary attack vector.

**Microsoft Hybrid Guidance:**

- Deploy **Microsoft Entra ID Identity Protection** (requires P2) to detect and automatically remediate identity-based risks including leaked credentials, impossible travel, anonymous IP sign-ins, password spray and token replay attacks.
- Configure **risk-based Conditional Access policies** that require MFA or block access when Identity Protection detects medium or high sign-in risk, and force password change when user risk is elevated.
- Deploy **Microsoft Defender for Identity** on all domain controllers to detect on-premises AD attacks including Pass-the-Hash, Pass-the-Ticket, Golden Ticket, DCSync, reconnaissance and lateral movement.
- Enable **Entra ID token protection (token binding)** to bind tokens to the device they were issued on, mitigating token theft and replay attacks.
- Implement **Entra ID Continuous Access Evaluation (CAE)** to ensure that compromised sessions are revoked in near real-time when risk is detected, rather than waiting for token expiry.
- Protect the on-premises AD tier using the **Microsoft tiered administration model**, deploy **Local Administrator Password Solution (LAPS)** managed through Entra ID, and enforce **SMB signing** and **LDAP signing** to prevent protocol-level attacks.

**Key Microsoft Technologies:**
| Technology | Purpose |
|------------|---------|
| Entra ID Identity Protection | Automated identity risk detection and remediation |
| Risk-based Conditional Access | Adaptive access control based on real-time risk |
| Defender for Identity | On-premises AD attack detection |
| Token Protection (Token Binding) | Mitigate token theft and replay |
| Continuous Access Evaluation (CAE) | Near real-time session revocation |
| LAPS via Entra ID | Cloud-managed local admin password rotation |

---

### 10. Legacy System Integration

**Challenge:** Older systems do not support modern authentication protocols.

**Microsoft Hybrid Guidance:**

- Deploy **Microsoft Entra Application Proxy** for on-premises web applications that use legacy authentication (Kerberos, NTLM, header-based, forms-based). Application Proxy provides Entra ID pre-authentication, SSO and Conditional Access without modifying the application.
- For applications requiring **Kerberos** authentication, configure **Kerberos Constrained Delegation (KCD)** on the Application Proxy connector to perform protocol transition from Entra ID tokens to Kerberos tickets.
- For applications requiring **NTLM** authentication, Application Proxy connectors handle NTLM seamlessly when joined to the same AD domain.
- For applications using **header-based authentication**, use the built-in header injection capability of Application Proxy or deploy a **Secure Hybrid Access (SHA) partner integration** (F5, Zscaler, Akamai, Citrix) for complex scenarios.
- For thick-client or non-web legacy applications, use **Entra ID Application Proxy with native client support** or configure **Entra Private Access** (part of Microsoft Global Secure Access) for zero-trust network access to private applications without VPN.
- Where applications provide **LDAP-only** authentication, deploy **Entra Domain Services** (managed AD DS in Azure) which provides LDAP and Kerberos endpoints synchronised from Entra ID, without the need for self-managed domain controllers.

**Key Microsoft Technologies:**
| Technology | Purpose |
|------------|---------|
| Entra Application Proxy | SSO and Conditional Access for on-premises web apps |
| Kerberos Constrained Delegation (KCD) | Protocol transition for Kerberos-based apps |
| Secure Hybrid Access (SHA) Partners | Complex legacy authentication integration |
| Entra Private Access (Global Secure Access) | Zero-trust access to private apps replacing VPN |
| Entra Domain Services | Managed LDAP/Kerberos for legacy cloud workloads |

---

### 11. Zero Trust Adoption

**Challenge:** Transitioning from perimeter-based security to identity-centric Zero Trust architecture.

**Microsoft Hybrid Guidance:**

- Adopt the **Microsoft Zero Trust identity pillar** framework: verify explicitly, use least-privilege access, assume breach. Entra ID and Conditional Access serve as the primary policy enforcement point.
- Deploy **Conditional Access** as the Zero Trust policy engine, evaluating every access request against signals including user identity, device compliance (via Intune), location, application sensitivity, real-time risk (via Identity Protection) and session context.
- Implement **device-based controls** by enrolling devices in **Microsoft Intune** and requiring device compliance or Entra ID hybrid join as a condition of access via Conditional Access.
- Deploy **Microsoft Global Secure Access** (Entra Internet Access and Entra Private Access) to replace traditional VPN with identity-aware, Zero Trust network access for both internet and private application traffic.
- Enforce **continuous validation** with Continuous Access Evaluation (CAE), token protection and sign-in frequency policies to ensure that trust is re-evaluated throughout the session, not just at initial sign-in.
- Use the **Microsoft Zero Trust assessment** tool and **Entra ID secure score** to benchmark progress and identify the highest-impact improvements.

**Key Microsoft Technologies:**
| Technology | Purpose |
|------------|---------|
| Conditional Access | Zero Trust policy engine for every access request |
| Microsoft Intune | Device compliance as an access condition |
| Global Secure Access (Internet + Private) | Identity-aware network access replacing VPN |
| Continuous Access Evaluation (CAE) | Ongoing trust validation during sessions |
| Entra ID Secure Score | Zero Trust maturity benchmarking |

---

### 12. Cross-Domain Identity Federation

**Challenge:** Establishing trust with partners, suppliers and customers across identity domains.

**Microsoft Hybrid Guidance:**

- Use **Entra External ID (B2B collaboration)** as the primary federation mechanism for partner and supplier access. B2B collaboration supports federation with other Entra ID tenants, SAML/WS-Fed identity providers, Google, Facebook and one-time passcode for unmanaged accounts.
- Configure **Entra ID cross-tenant access settings** for each partner organisation, defining inbound and outbound trust settings for MFA, device compliance and Conditional Access policy acceptance.
- For large-scale multi-party federation, use **Entra ID multi-tenant organisation (MTO)** capabilities to establish a trust fabric across a group of related tenants with seamless user experience.
- For customer-facing identity federation (B2C), deploy **Microsoft Entra External ID** (external-facing tenant configuration) which provides customisable sign-up/sign-in experiences, social identity provider federation, and progressive profiling.
- Ensure **on-premises federated applications** (those using AD FS) are migrated to Entra ID using the **AD FS to Entra ID migration tool**, consolidating federation endpoints and reducing the attack surface.

**Key Microsoft Technologies:**
| Technology | Purpose |
|------------|---------|
| Entra External ID (B2B) | Partner and supplier federation |
| Cross-tenant Access Settings | Per-partner trust and policy configuration |
| Multi-tenant Organisation (MTO) | Trust fabric across affiliated tenants |
| Entra External ID (External Tenant) | Customer-facing identity and social federation |
| AD FS Migration Tool | Migrate federated apps to Entra ID |

---

### 13. Identity Data Quality

**Challenge:** Inconsistent, incomplete or outdated identity attributes undermine automation.

**Microsoft Hybrid Guidance:**

- Establish the **HR system** (Workday, SAP SuccessFactors) as the authoritative source for core identity attributes (name, job title, department, manager, cost centre, location, employment status) via **Entra ID HR-driven provisioning**.
- Configure **attribute mapping rules** in Entra Connect and HR provisioning to ensure consistent transformation and normalisation of attributes across HR, AD DS and Entra ID.
- Use **Entra ID dynamic groups** and **Entitlement Management auto-assignment policies** as a litmus test for data quality: if dynamic group membership is incorrect, the underlying attribute data needs correction.
- Deploy **custom security attributes** in Entra ID to store identity metadata that does not originate from HR or AD (e.g. risk classification, data access tier, project assignment) in a governed, schema-defined manner.
- Implement **Entra ID provisioning logs** monitoring to detect attribute sync failures, mismatches and missing values. Alert on provisioning errors via Microsoft Sentinel or Azure Monitor.

**Key Microsoft Technologies:**
| Technology | Purpose |
|------------|---------|
| HR-driven Provisioning | Authoritative attribute source from HR systems |
| Attribute Mapping Rules | Consistent attribute transformation and normalisation |
| Dynamic Groups (as data quality test) | Validate attribute accuracy through group membership |
| Custom Security Attributes | Governed storage for additional identity metadata |
| Provisioning Logs Monitoring | Detect and alert on attribute sync errors |

---

### 14. Separation of Duties Conflicts

**Challenge:** Ensuring no single identity holds conflicting permissions at scale.

**Microsoft Hybrid Guidance:**

- Use **Entitlement Management incompatible access packages** to define separation of duties rules. When two access packages are marked as incompatible, a user cannot hold both simultaneously (e.g. "Accounts Payable Processor" and "Accounts Payable Approver").
- Configure **Entitlement Management incompatible group rules** to prevent a user from being a member of two conflicting security groups simultaneously.
- Deploy **Entra ID Access Reviews** that specifically target users who hold multiple privileged roles, flagging potential SoD conflicts for manual review and remediation.
- Use **PIM role activation alerts** to detect when a single user activates multiple privileged roles that together create a conflict (e.g. simultaneously holding User Administrator and Privileged Role Administrator).
- For complex SoD scenarios beyond Entra ID's native capabilities, integrate with a dedicated **Identity Governance and Administration (IGA)** platform (e.g. SailPoint, Saviynt, Omada) that connects to Entra ID via Microsoft Graph API for advanced SoD policy enforcement and mining.

**Key Microsoft Technologies:**
| Technology | Purpose |
|------------|---------|
| Incompatible Access Packages | Prevent conflicting access package assignments |
| Incompatible Group Rules | Prevent conflicting group memberships |
| Access Reviews (Multi-role) | Review users with multiple privileged roles |
| PIM Activation Alerts | Detect conflicting concurrent role activations |
| IGA Platform Integration (via Graph API) | Advanced SoD policy enforcement for complex scenarios |
