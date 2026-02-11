# Common Identity Challenges: OpenText Product Guidance

This document maps each common identity challenge (from small, medium and large organisations) to the relevant **OpenText** product capabilities. OpenText acquired Micro Focus in 2023, inheriting the **NetIQ** identity and security portfolio. All guidance assumes an environment where OpenText products are deployed alongside existing directory infrastructure (Active Directory, LDAP, cloud directories).

Where no known OpenText product directly addresses a challenge, this is explicitly noted.

---

## OpenText Identity and Security Product Portfolio Reference

| Product | Category | Description |
|---------|----------|-------------|
| **OpenText Identity Manager (NetIQ)** | Identity Lifecycle Management | Automated provisioning, deprovisioning and identity lifecycle orchestration via policy-driven workflows and a broad connector framework |
| **OpenText Identity Governance (NetIQ)** | Identity Governance and Administration | Access certification campaigns, separation of duties, role mining, access request and approval, compliance reporting |
| **OpenText Access Manager (NetIQ)** | Access Management / SSO | Single sign-on, federation (SAML, OAuth, OIDC, WS-Federation), web access management, risk-based authentication |
| **OpenText Advanced Authentication (NetIQ)** | Multi-Factor Authentication | Centralised MFA framework supporting FIDO2, biometrics, smartcards, push notifications, OTP, passwordless authentication |
| **OpenText Privileged Account Manager (NetIQ)** | Privileged Access Management | Privileged session management, credential vaulting, command-level access control, session recording |
| **OpenText Directory and Resource Administrator (DRA)** | Delegated AD Administration | Delegated, policy-based administration of Active Directory, Azure AD/Entra ID, Exchange and Microsoft 365 |
| **OpenText Change Guardian (NetIQ)** | Change Monitoring | Real-time change detection and monitoring for Active Directory, file systems, Group Policy and Windows registry |
| **OpenText ArcSight** | SIEM | Security information and event management, log correlation, compliance reporting and threat detection |
| **OpenText eDirectory (NetIQ)** | LDAP Directory | Cross-platform LDAP directory service for identity storage and authentication |
| **OpenText Universal Discovery and CMDB** | Asset and Configuration Management | Discovery of infrastructure, applications and services for configuration management |

---

## Small Organisation Challenges

### 1. No Centralised Identity Provider

**Challenge:** User accounts are created independently in each SaaS application.

**OpenText Guidance:**

- Deploy **OpenText Identity Manager** as the centralised identity hub. Identity Manager connects to multiple target systems (Active Directory, LDAP directories, SaaS applications, databases) via its extensive **driver framework**, establishing a single authoritative identity source.
- Use the **SCIM driver** in Identity Manager to provision and deprovision user accounts in cloud/SaaS applications (e.g. Salesforce, SAP, ServiceNow) that support the SCIM standard, eliminating per-application manual account creation.
- Deploy **OpenText Access Manager** to provide **single sign-on (SSO)** across web applications using SAML, OAuth 2.0 and OpenID Connect federation, so users authenticate once against a central identity provider.
- Identity Manager's **Identity Vault** (built on OpenText eDirectory) serves as the central metadirectory, synchronising identity data across all connected systems regardless of the underlying directory technology.

**Key OpenText Technologies:**
| Technology | Purpose |
|------------|---------|
| Identity Manager + Driver Framework | Centralised identity provisioning to all connected systems |
| SCIM Driver | Cloud/SaaS application provisioning |
| Access Manager | SSO and federation across web applications |
| Identity Vault (eDirectory) | Central metadirectory for identity data |

---

### 2. Shared Accounts

**Challenge:** Teams share logins for cost reasons or convenience.

**OpenText Guidance:**

- Use **OpenText Identity Manager** to automate the creation of **individual user accounts** across all connected systems, reducing the cost and effort of per-user account provisioning that often drives shared account usage.
- Deploy **OpenText Access Manager** to provide SSO so that individual users can access all required applications with a single credential, removing the convenience argument for sharing accounts.
- **OpenText Privileged Account Manager** can manage shared privileged accounts (e.g. root, administrator) by vaulting credentials and providing individual users with checked-out, time-bound access. All sessions are attributed to the individual who checked out the credential, maintaining accountability.
- Use **OpenText ArcSight** to correlate sign-in events and detect anomalous patterns (e.g. concurrent logins from different locations on the same account) that indicate credential sharing.

**Key OpenText Technologies:**
| Technology | Purpose |
|------------|---------|
| Identity Manager | Automated individual account provisioning |
| Access Manager (SSO) | Single credential for all applications |
| Privileged Account Manager | Managed shared privileged accounts with individual attribution |
| ArcSight | Detection of credential-sharing patterns |

---

### 3. Weak or No MFA Adoption

**Challenge:** Multi-factor authentication is often not enforced.

**OpenText Guidance:**

- Deploy **OpenText Advanced Authentication** as a centralised MFA framework. It supports a wide range of authentication methods including **FIDO2/passkeys, smartphone push, biometrics, smartcards, OTP tokens, SMS and voice**.
- Advanced Authentication provides a **single policy console** to manage authentication methods and enforcement policies across all integrated applications, ensuring consistent MFA enforcement.
- Integrate Advanced Authentication with **OpenText Access Manager** to enforce MFA at the point of SSO, so all federated applications are protected without modifying individual applications.
- Advanced Authentication supports **risk-based (adaptive) authentication**, evaluating contextual factors (location, device, time, behaviour) to determine when to step up to MFA, balancing security and user experience.
- Advanced Authentication supports **passwordless authentication** via FIDO2, passkeys, smartcards and biometrics, aligning with modern authentication strategies.

**Key OpenText Technologies:**
| Technology | Purpose |
|------------|---------|
| Advanced Authentication | Centralised MFA with FIDO2, push, biometrics, OTP and more |
| Access Manager + Advanced Authentication | MFA enforcement at the SSO layer |
| Risk-based Authentication | Adaptive step-up MFA based on context |
| FIDO2 / Passkey Support | Passwordless authentication |

---

### 4. No Joiners/Movers/Leavers Process

**Challenge:** When staff leave, accounts are often not disabled promptly.

**OpenText Guidance:**

- **OpenText Identity Manager** is purpose-built for identity lifecycle management. Connect it to **HR systems** (Workday, SAP HR, ADP, UltiPro, Paycom) using built-in drivers to automatically trigger account creation (joiner), modification (mover) and disablement/deletion (leaver) based on HR events.
- Identity Manager's **policy-driven workflow engine** orchestrates the entire lifecycle: when an employee's status changes in HR, Identity Manager automatically provisions or deprovisions accounts across all connected target systems (AD, email, SaaS applications, databases).
- For movers, Identity Manager recalculates **role-based access** when an employee's department, job title or location changes in the HR system, automatically adding new entitlements and removing old ones.
- Configure Identity Manager to **immediately disable** accounts across all connected systems upon leaver events, rather than waiting for manual IT intervention.

**Key OpenText Technologies:**
| Technology | Purpose |
|------------|---------|
| Identity Manager (HR Drivers) | Automated lifecycle management triggered by HR events |
| Identity Manager Workflow Engine | Policy-driven joiner/mover/leaver orchestration |
| Role-based Provisioning | Automatic entitlement recalculation on role change |
| Multi-system Deprovisioning | Simultaneous account disable across all connected systems |

---

### 5. Password Reuse

**Challenge:** Without a password manager or SSO, users reuse passwords across accounts.

**OpenText Guidance:**

- Deploy **OpenText Access Manager** for enterprise SSO across all web applications (SAML, OIDC, WS-Federation), so users only need to manage a single password, reducing the incentive for password reuse.
- Use **OpenText Identity Manager** to enforce **consistent password policies** across all connected systems via the Identity Manager password policy framework. Policies can enforce complexity, length, history and custom rules uniformly.
- Deploy **OpenText Advanced Authentication** to enable **passwordless authentication** (FIDO2, biometrics, smartcards, push notifications), removing passwords from the equation entirely.
- Identity Manager supports **password synchronisation** across connected systems, so when a user changes their password in one place, it propagates to all connected directories and applications, further reducing the temptation to maintain separate passwords.

**Key OpenText Technologies:**
| Technology | Purpose |
|------------|---------|
| Access Manager (SSO) | Single credential across all applications |
| Identity Manager (Password Policies) | Consistent password policy enforcement across systems |
| Advanced Authentication (Passwordless) | Eliminate passwords via FIDO2, biometrics, passkeys |
| Password Synchronisation | Propagate password changes across all connected systems |

---

### 6. Shadow IT

**Challenge:** Staff adopt tools and services without IT oversight.

> **Not fully addressed by OpenText products.**
>
> OpenText does not offer a direct equivalent to a Cloud Access Security Broker (CASB) or shadow IT discovery tool that analyses network traffic to identify unsanctioned SaaS applications. However, partial mitigation is possible:

- **OpenText ArcSight** can correlate network and proxy logs to identify unusual outbound connections to unknown SaaS providers, though this requires manual rule creation and is not a purpose-built shadow IT discovery capability.
- **OpenText Access Manager** can enforce that all web application access is routed through a central SSO gateway, and applications not integrated with Access Manager can be identified through access logs.
- **OpenText Identity Governance** provides visibility into which applications have been integrated into the identity governance framework, highlighting gaps where unmanaged applications may exist.

For comprehensive shadow IT discovery and cloud app risk scoring, organisations using OpenText would typically need to supplement with a dedicated CASB or cloud security product.

**Key OpenText Technologies:**
| Technology | Purpose |
|------------|---------|
| ArcSight (Partial) | Log correlation to detect unusual SaaS access patterns |
| Access Manager (Partial) | Centralised SSO gateway to identify non-integrated apps |
| Identity Governance (Partial) | Visibility into governed vs ungoverned applications |

---

### 7. No Access Reviews

**Challenge:** There is no formal process to review who has access to what.

**OpenText Guidance:**

- Deploy **OpenText Identity Governance** to run **access certification campaigns**. Identity Governance provides a business-friendly interface for managers and resource owners to review and certify user access on a recurring schedule.
- Identity Governance supports **micro-certifications** — targeted, event-driven reviews triggered by specific changes (e.g. role change, new high-risk access granted) — in addition to scheduled full certification campaigns.
- Configure **automated remediation** so that access denied during certification is automatically revoked via integration with Identity Manager.
- Identity Governance provides **dashboards and compliance reports** showing certification completion rates, overdue reviews and access risk posture across the organisation.

**Key OpenText Technologies:**
| Technology | Purpose |
|------------|---------|
| Identity Governance (Certification Campaigns) | Recurring access certification and review |
| Micro-certifications | Event-driven, targeted access reviews |
| Identity Governance + Identity Manager | Automated remediation of denied access |
| Governance Dashboards | Compliance reporting and review tracking |

---

### 8. Owner Dependency

**Challenge:** A single individual holds all administrative credentials.

**OpenText Guidance:**

- Deploy **OpenText Directory and Resource Administrator (DRA)** to enable **delegated, role-based administration** of Active Directory, Entra ID, Exchange and Microsoft 365. DRA allows administrative responsibilities to be distributed across multiple staff with granular, least-privilege permissions rather than relying on a single owner with full admin rights.
- Use **OpenText Privileged Account Manager** to vault all administrative credentials (local admin, domain admin, root, service accounts). Individual administrators check out credentials on demand with time-bound access, justification and full session recording, eliminating any single person holding persistent administrative access.
- **OpenText Change Guardian** monitors Active Directory, Group Policy and critical system configurations for changes, alerting if the emergency/break-glass accounts are used or if administrative permissions are modified.
- **OpenText ArcSight** provides a consolidated audit trail of all administrative actions across the environment, ensuring that no single administrator can operate without oversight.

**Key OpenText Technologies:**
| Technology | Purpose |
|------------|---------|
| Directory and Resource Administrator (DRA) | Delegated, least-privilege AD administration |
| Privileged Account Manager | Credential vaulting and time-bound checkout |
| Change Guardian | Monitor and alert on administrative changes |
| ArcSight | Consolidated admin activity audit trail |

---

## Medium Organisation Challenges

### 1. Identity Silos

**Challenge:** Different departments manage their own user directories.

**OpenText Guidance:**

- Deploy **OpenText Identity Manager** as a **metadirectory** to connect and synchronise identities across all directory silos (Active Directory forests, LDAP directories, HR systems, SaaS directories). Identity Manager's driver framework supports hundreds of target systems.
- Identity Manager's **Identity Vault** (eDirectory) serves as the canonical identity store, reconciling and correlating duplicate identities from multiple sources using configurable **matching and merge rules**.
- Use **OpenText Directory and Resource Administrator (DRA)** to provide a **unified administration console** across multiple Active Directory forests and domains, enabling centralised management without requiring forest consolidation.
- **OpenText Identity Governance** provides a **single pane of glass** for governance and compliance across all connected identity silos, regardless of the underlying directory technology.

**Key OpenText Technologies:**
| Technology | Purpose |
|------------|---------|
| Identity Manager (Metadirectory) | Cross-directory identity synchronisation and correlation |
| Identity Vault (eDirectory) | Canonical identity store with matching and merge rules |
| DRA | Unified administration across multiple AD forests |
| Identity Governance | Consolidated governance view across all identity silos |

---

### 2. Inconsistent Provisioning

**Challenge:** No standardised process for granting access.

**OpenText Guidance:**

- **OpenText Identity Manager** provides a **self-service access request portal** where users and managers can request access through standardised, auditable workflows with configurable approval chains.
- Identity Manager supports **role-based access control (RBAC)** with automatic provisioning: define business roles mapped to technical entitlements, and when a user is assigned a role (manually or based on HR attributes), all associated access is provisioned automatically across connected systems.
- Use the **SCIM driver** and other **built-in connectors** (AD, LDAP, SAP, Salesforce, ServiceNow, databases, REST/SOAP APIs) to automate provisioning to target systems without manual intervention.
- **OpenText Identity Manager HR drivers** (Workday, SAP HR, ADP, UltiPro) enable **HR-driven provisioning** so that new hires automatically receive the correct baseline access based on their HR attributes on their start date.

**Key OpenText Technologies:**
| Technology | Purpose |
|------------|---------|
| Identity Manager (Self-service Portal) | Standardised access request and approval workflows |
| Role-based Access Control (RBAC) | Automatic provisioning based on business roles |
| SCIM Driver / Connector Framework | Automated provisioning to SaaS and enterprise systems |
| HR Drivers | Automated account creation from HR system events |

---

### 3. Privilege Creep

**Challenge:** Old access is rarely revoked when employees change roles.

**OpenText Guidance:**

- **OpenText Identity Manager** recalculates entitlements when HR attributes change (department, job title, location). Role-based policies automatically **remove old access** and **grant new access** in a single workflow, preventing the accumulation of stale entitlements.
- Deploy **OpenText Identity Governance** with recurring **access certification campaigns** to periodically review and certify that all user access remains appropriate. Uncertified access is automatically revoked through integration with Identity Manager.
- Identity Governance provides **role mining** capabilities that analyse existing access patterns to identify and rationalise over-entitled users, helping organisations clean up historical privilege creep.
- Use Identity Manager's **time-bound access** capabilities to grant access with automatic expiry dates, requiring explicit renewal for continued access.

**Key OpenText Technologies:**
| Technology | Purpose |
|------------|---------|
| Identity Manager (Mover Workflows) | Automatic entitlement recalculation on role change |
| Identity Governance (Certification) | Periodic review and revocation of stale access |
| Role Mining | Identify over-entitled users and rationalise access |
| Time-bound Access | Automatic access expiry with renewal requirements |

---

### 4. Contractor and Third-Party Access

**Challenge:** Growing use of external contractors without a clear identity lifecycle.

**OpenText Guidance:**

- **OpenText Identity Manager** manages the full lifecycle for external identities (contractors, partners, vendors) with separate policies and workflows. External users can be onboarded via a **sponsor-initiated request** with appropriate approval chains.
- Configure **time-limited provisioning** in Identity Manager so that contractor accounts automatically expire on the contract end date and are disabled across all connected systems.
- **OpenText Identity Governance** can scope **dedicated certification campaigns for external users**, requiring sponsors to re-certify contractor access at defined intervals (e.g. every 90 days).
- **OpenText Access Manager** can apply **differentiated access policies** for external users, such as restricting accessible applications, enforcing stronger authentication or limiting session durations.

**Key OpenText Technologies:**
| Technology | Purpose |
|------------|---------|
| Identity Manager (External Lifecycles) | Sponsor-initiated external user onboarding and offboarding |
| Time-limited Provisioning | Automatic contractor account expiry |
| Identity Governance (External Reviews) | Recurring sponsor certification of contractor access |
| Access Manager (Policy Differentiation) | Stricter access policies for external users |

---

### 5. Compliance Gaps

**Challenge:** Regulations require demonstrable access controls that informal processes cannot satisfy.

**OpenText Guidance:**

- **OpenText Identity Governance** provides **pre-built compliance reporting** for access certifications, separation of duties violations, access request audit trails and entitlement snapshots, supporting ISO 27001, GDPR, SOX and other frameworks.
- Identity Governance generates **historical point-in-time reports** showing who had access to what at any given date, which is essential for regulatory audits.
- **OpenText ArcSight** provides long-term **security event log retention** and **compliance dashboards** (PCI-DSS, SOX, HIPAA, GDPR) with pre-built compliance content packs for regulatory reporting.
- **OpenText Change Guardian** creates an auditable record of all changes to Active Directory, Group Policy and critical systems, supporting change management compliance requirements.
- Identity Manager's workflow engine generates a full **audit trail** of all provisioning decisions, approvals and actions, providing evidence for compliance reviews.

**Key OpenText Technologies:**
| Technology | Purpose |
|------------|---------|
| Identity Governance (Compliance Reports) | Access certification and entitlement compliance reporting |
| Identity Governance (Point-in-time Reports) | Historical access snapshots for audits |
| ArcSight | Long-term log retention and regulatory compliance dashboards |
| Change Guardian | Auditable change records for AD and critical systems |
| Identity Manager (Audit Trail) | Provisioning workflow audit evidence |

---

### 6. Lack of SSO Across All Applications

**Challenge:** SSO is implemented for some applications but not others.

**OpenText Guidance:**

- **OpenText Access Manager** provides SSO across web applications using **SAML 2.0, OAuth 2.0, OpenID Connect and WS-Federation**. It includes a **preconfigured connector catalogue** for common SaaS applications and a toolkit for custom integrations.
- For on-premises and legacy web applications that do not support federation protocols, Access Manager provides a **reverse proxy (Access Gateway)** that handles authentication on behalf of the application, injecting credentials or headers without modifying the application itself.
- Access Manager supports **Kerberos-based SSO** for desktop-to-web application single sign-on in Windows domain-joined environments.
- For thick-client and non-web applications, Access Manager offers a **desktop agent** that provides credential injection-based SSO.
- Use Access Manager's **analytics and reporting** to identify applications that are not yet integrated with SSO and prioritise onboarding.

**Key OpenText Technologies:**
| Technology | Purpose |
|------------|---------|
| Access Manager (Federation) | SSO via SAML, OAuth, OIDC, WS-Federation |
| Access Gateway (Reverse Proxy) | SSO for legacy web apps without application modification |
| Kerberos SSO | Desktop-to-web SSO in Windows environments |
| Desktop Agent | SSO for thick-client and non-web applications |

---

### 7. Weak Privileged Access Management

**Challenge:** Admin and service accounts are not adequately secured.

**OpenText Guidance:**

- Deploy **OpenText Privileged Account Manager** to vault all privileged credentials (domain admin, local admin, root, service accounts, database admin). Administrators check out credentials on demand with time-bound access, justification and approval workflows.
- Privileged Account Manager provides **full session recording** (keystroke and video) for all privileged sessions, ensuring complete accountability and forensic capability.
- Configure **command-level access control** so privileged users can only execute pre-approved commands on target systems (Unix, Linux, Windows), even when using root or administrator accounts.
- **OpenText Change Guardian** monitors Active Directory and Group Policy for unauthorised changes made by privileged accounts, providing an independent audit trail.
- Integrate Privileged Account Manager with **OpenText ArcSight** for centralised monitoring, alerting and correlation of all privileged account activity.

**Key OpenText Technologies:**
| Technology | Purpose |
|------------|---------|
| Privileged Account Manager (Credential Vault) | Secure storage and checked-out access to privileged credentials |
| Session Recording | Full audit trail of privileged sessions |
| Command-level Access Control | Restrict what commands privileged users can execute |
| Change Guardian | Independent change detection for AD and Group Policy |
| ArcSight Integration | Centralised privileged activity monitoring and alerting |

---

### 8. Incomplete Offboarding

**Challenge:** Leavers retain access because there is no single source of truth for entitlements.

**OpenText Guidance:**

- **OpenText Identity Manager** serves as the **single orchestration point** for offboarding. When connected to the HR system, a termination event triggers an automated workflow that disables or deletes the user's accounts across **all connected target systems** simultaneously.
- Identity Manager maintains a **complete entitlement map** for each identity (all accounts, group memberships, application assignments, roles), ensuring nothing is missed during offboarding.
- Configure **leaver workflows** in Identity Manager to execute organisation-specific offboarding tasks: disable accounts, remove group memberships, revoke application access, convert mailboxes, transfer file ownership, notify the manager and archive identity data.
- **OpenText Identity Governance** can generate **post-offboarding verification reports** confirming that all access has been revoked across all governed systems.

**Key OpenText Technologies:**
| Technology | Purpose |
|------------|---------|
| Identity Manager (HR-triggered Offboarding) | Automated multi-system account deprovisioning |
| Complete Entitlement Map | Single view of all access for each identity |
| Leaver Workflows | Orchestrated offboarding task execution |
| Identity Governance (Verification) | Post-offboarding access revocation confirmation |

---

### 9. Limited Visibility and Reporting

**Challenge:** No consolidated view of who has access to what.

**OpenText Guidance:**

- **OpenText Identity Governance** provides a **unified access view** across all connected systems, showing every user's entitlements, roles, group memberships and application assignments in a single interface, regardless of the underlying target system.
- Identity Governance includes **pre-built dashboards** for access analytics, risk scoring, certification status, separation of duties violations and entitlement distribution.
- **OpenText ArcSight** provides **SIEM-based visibility** into identity-related events across the entire hybrid environment, correlating sign-in activity, access changes and security events from Active Directory, Entra ID, SaaS applications and on-premises systems.
- **OpenText Change Guardian** provides focused visibility into changes to Active Directory objects, Group Policy, file systems and registry, highlighting who changed what and when.
- Identity Manager's **reporting module** generates operational reports on provisioning activity, workflow status, pending approvals and identity lifecycle events.

**Key OpenText Technologies:**
| Technology | Purpose |
|------------|---------|
| Identity Governance (Unified Access View) | Consolidated entitlement visibility across all systems |
| Identity Governance (Dashboards) | Access analytics, risk scoring and compliance status |
| ArcSight | SIEM-based identity event correlation and reporting |
| Change Guardian | AD and system change visibility and audit |
| Identity Manager (Reports) | Provisioning and lifecycle operational reporting |

---

### 10. Growing Machine Identity Footprint

**Challenge:** Increasing use of APIs, automation and cloud services creates unmanaged non-human identities.

**OpenText Guidance:**

- **OpenText Identity Manager** can manage **service account lifecycles** by treating service accounts as identities with owners, expiry dates and approval workflows. Service accounts can be provisioned, rotated and deprovisioned through the same lifecycle processes as human identities.
- **OpenText Privileged Account Manager** vaults service account credentials and supports **automated credential rotation** on a defined schedule, ensuring service account passwords are changed regularly without manual intervention.
- **OpenText Identity Governance** can include service accounts in **access certification campaigns**, requiring designated owners to periodically certify that each service account is still needed and appropriately scoped.

> **Partial gap:** OpenText does not provide a direct equivalent to cloud-native **managed identities** or **workload identity federation** (credential-free authentication for cloud workloads). For Azure, AWS or GCP workloads, organisations would need to supplement with the cloud provider's native managed identity capabilities. OpenText's coverage focuses on traditional service account lifecycle management and credential rotation rather than credential elimination.

**Key OpenText Technologies:**
| Technology | Purpose |
|------------|---------|
| Identity Manager (Service Account Lifecycle) | Managed provisioning, ownership and expiry for service accounts |
| Privileged Account Manager (Credential Rotation) | Automated service account password rotation |
| Identity Governance (Service Account Reviews) | Certification campaigns for non-human identities |

---

## Large Organisation Challenges

### 1. Identity Governance at Scale

**Challenge:** Managing hundreds of thousands of identities across multiple systems requires mature tooling and processes.

**OpenText Guidance:**

- **OpenText Identity Manager** is designed for enterprise-scale deployments, supporting **millions of identities** across hundreds of connected target systems via its driver framework. It supports clustered, high-availability configurations for large environments.
- **OpenText Identity Governance** provides **delegated governance** through scoped views, allowing business unit owners to manage certifications and access reviews for their populations while central IT maintains policy control and oversight.
- Identity Manager integrates with enterprise **HR platforms** (Workday, SAP HR, ADP) and **ITSM platforms** (ServiceNow) for end-to-end identity lifecycle orchestration at scale.
- Use the **Identity Manager REST API** and **Identity Governance API** to build custom integrations, dashboards and automation for large-scale operational requirements.
- Identity Governance's **role mining** and **analytics** capabilities help large organisations continuously rationalise and optimise their role model as the organisation evolves.

**Key OpenText Technologies:**
| Technology | Purpose |
|------------|---------|
| Identity Manager (Enterprise Scale) | High-availability lifecycle management for millions of identities |
| Identity Governance (Delegated Governance) | Business-unit-level governance with central oversight |
| HR and ITSM Integration | End-to-end lifecycle orchestration with enterprise platforms |
| REST / Governance APIs | Custom integration and automation |
| Role Mining and Analytics | Continuous role model optimisation |

---

### 2. Merger and Acquisition Integration

**Challenge:** Acquired entities bring their own identity infrastructure that must be rationalised.

**OpenText Guidance:**

- **OpenText Identity Manager** acts as a **metadirectory bridge** between the acquiring and acquired organisation's directories. By deploying drivers to both organisations' AD forests (or other directories), Identity Manager can synchronise, correlate and eventually consolidate identities across environments.
- **OpenText Directory and Resource Administrator (DRA)** provides a **unified administration console** spanning multiple AD forests (parent and acquired), enabling IT teams to manage users across both environments from a single tool during the transition.
- **OpenText Identity Governance** can be deployed to govern access across **both organisations' systems simultaneously**, providing a consolidated view of entitlements and running cross-organisational certification campaigns.
- Identity Manager's **matching and merge rules** can identify and reconcile duplicate identities that exist in both the acquiring and acquired organisations' directories.

> **Partial gap:** OpenText does not offer native **tenant-to-tenant synchronisation** or **multi-tenant organisation (MTO)** capabilities equivalent to those in Entra ID. For M&A scenarios involving Microsoft 365/Entra ID tenants, the Microsoft-native cross-tenant tools would typically be used alongside OpenText for broader lifecycle and governance functions.

**Key OpenText Technologies:**
| Technology | Purpose |
|------------|---------|
| Identity Manager (Metadirectory) | Cross-organisation identity synchronisation and correlation |
| DRA (Multi-forest) | Unified AD administration across acquiring and acquired forests |
| Identity Governance (Cross-org) | Consolidated governance across both organisations |
| Matching and Merge Rules | Duplicate identity reconciliation |

---

### 3. Regulatory Complexity

**Challenge:** Operating across jurisdictions requires compliance with multiple overlapping regulations.

**OpenText Guidance:**

- **OpenText Identity Governance** supports multiple concurrent **compliance frameworks**, allowing organisations to run certification campaigns and generate reports aligned to GDPR, SOX, HIPAA, ISO 27001, NIS2 and other regulations simultaneously.
- Identity Governance provides **point-in-time access snapshots** and **historical entitlement reports** that meet regulator requirements for demonstrating access controls at specific dates.
- **OpenText ArcSight** provides **compliance content packs** (pre-built dashboards, reports and correlation rules) for major regulatory frameworks, and supports **multi-region log storage** to meet data residency requirements for audit logs.
- Identity Manager's workflow engine generates a complete **audit trail** of all provisioning decisions, approvals and actions that can be presented as compliance evidence.

> **Partial gap:** OpenText does not provide native **identity data residency controls** (i.e. ensuring identity records are stored in specific geographic regions). Data residency for the identity store itself would need to be managed through infrastructure deployment decisions (deploying Identity Vault instances in specific regions) rather than through built-in data boundary features.

**Key OpenText Technologies:**
| Technology | Purpose |
|------------|---------|
| Identity Governance (Multi-framework) | Concurrent compliance reporting across regulations |
| Point-in-time Reports | Historical access snapshots for auditor evidence |
| ArcSight (Compliance Packs) | Pre-built regulatory dashboards and multi-region log storage |
| Identity Manager (Audit Trail) | Complete provisioning decision evidence |

---

### 4. Orphaned and Dormant Accounts

**Challenge:** The sheer volume of accounts makes it difficult to identify unused accounts.

**OpenText Guidance:**

- **OpenText Identity Governance** can identify **orphaned accounts** (accounts in target systems that do not correlate to an active identity in the authoritative source) and **dormant accounts** (accounts with no recent sign-in activity) through its data collection and analytics capabilities.
- Configure Identity Governance to generate **automated reports and alerts** for accounts that have been inactive beyond a defined threshold, routing them to owners for review or automatic disablement.
- **OpenText Identity Manager** can be configured with **scheduled reconciliation jobs** that compare accounts in target systems against the authoritative HR source, flagging or automatically disabling accounts with no matching active employee record.
- **OpenText ArcSight** can correlate sign-in logs across systems to identify accounts with no authentication activity over extended periods.

**Key OpenText Technologies:**
| Technology | Purpose |
|------------|---------|
| Identity Governance (Orphan Detection) | Identify accounts without matching active identities |
| Identity Governance (Dormancy Reporting) | Flag accounts with no recent activity |
| Identity Manager (Reconciliation) | Scheduled comparison of accounts against HR source |
| ArcSight (Activity Correlation) | Cross-system sign-in activity analysis |

---

### 5. Excessive Standing Privileges

**Challenge:** Users and service accounts retain persistent elevated access.

**OpenText Guidance:**

- Deploy **OpenText Privileged Account Manager** to implement **just-in-time privileged access**. Administrators request access to privileged credentials through a checkout process with time limits, justification and approval workflows. Credentials are automatically checked back in and rotated after the session ends.
- Privileged Account Manager provides **command filtering** so that even when users have checked out a privileged credential, they are restricted to executing only pre-approved commands, limiting the blast radius.
- **OpenText Identity Governance** can identify users with **excessive entitlements** through analytics and role mining, flagging accounts with permissions beyond what their peers in similar roles hold.
- Identity Governance supports **time-limited role assignments** in certification campaigns, where access can be granted with mandatory expiry and re-certification dates.

> **Partial gap:** OpenText Privileged Account Manager focuses on **infrastructure-level privileged accounts** (OS, database, network). For **application-level and cloud directory role escalation** (e.g. Entra ID Global Admin just-in-time activation), organisations would typically supplement with the cloud provider's native PIM capabilities.

**Key OpenText Technologies:**
| Technology | Purpose |
|------------|---------|
| Privileged Account Manager (JIT Access) | Time-bound, approval-based privileged credential checkout |
| Command Filtering | Restrict executable commands during privileged sessions |
| Identity Governance (Analytics) | Identify users with excessive entitlements vs peers |
| Time-limited Role Assignments | Access with mandatory expiry and re-certification |

---

### 6. Decentralised Application Ownership

**Challenge:** Business units own applications independently with inconsistent identity integration.

**OpenText Guidance:**

- **OpenText Identity Manager** provides a **centralised integration layer** that connects to all applications regardless of business unit ownership. By deploying drivers to each application, Identity Manager enforces consistent provisioning and deprovisioning policies across the organisation.
- **OpenText Identity Governance** provides **delegated governance** where application owners certify access to their own applications through a business-friendly interface, while the central identity team maintains overall governance policies and standards.
- **OpenText Access Manager** enforces **centralised authentication and SSO policies** across all integrated applications, ensuring a consistent security posture regardless of which business unit owns the application.
- Identity Governance's **compliance dashboards** provide central IT with visibility into the governance status of all applications, highlighting those with overdue certifications, unresolved SoD violations or ungoverned access.

**Key OpenText Technologies:**
| Technology | Purpose |
|------------|---------|
| Identity Manager (Centralised Integration) | Consistent provisioning across all business unit applications |
| Identity Governance (Delegated Governance) | Application owner-led certification with central oversight |
| Access Manager (Central SSO) | Consistent authentication policies across all apps |
| Compliance Dashboards | Visibility into governance status across all applications |

---

### 7. Machine Identity Explosion

**Challenge:** Thousands of service accounts, API keys and certificates are poorly tracked and rarely rotated.

**OpenText Guidance:**

- **OpenText Privileged Account Manager** provides a **credential vault** for service accounts and supports **automated password rotation** on a schedule or on demand across Unix, Linux and Windows systems.
- **OpenText Identity Manager** can treat service accounts as **governed identities** with designated owners, business justification, approval workflows and expiry dates, ensuring they follow a managed lifecycle.
- **OpenText Identity Governance** includes service accounts in **access certification campaigns**, requiring owners to periodically confirm that each service account is still required, appropriately scoped and has a valid business justification.

> **Partial gap:** OpenText does not provide native capabilities for **cloud-native workload identity management** such as managed identities, workload identity federation or certificate-based workload authentication. For cloud-native machine identities (Azure Managed Identities, AWS IAM roles, Kubernetes service accounts), organisations would need to use cloud-provider-native tooling. OpenText's strength is in managing **traditional service accounts** in on-premises and hybrid infrastructure.

**Key OpenText Technologies:**
| Technology | Purpose |
|------------|---------|
| Privileged Account Manager (Rotation) | Automated service account credential rotation |
| Identity Manager (Service Account Lifecycle) | Governed lifecycle with owners, justification and expiry |
| Identity Governance (Certification) | Periodic owner certification of service accounts |

---

### 8. Complex Access Certification

**Challenge:** Periodic access reviews become unmanageable at scale.

**OpenText Guidance:**

- **OpenText Identity Governance** is purpose-built for large-scale access certification. It supports **full certification campaigns** (all access across the organisation) and **micro-certifications** (targeted reviews triggered by specific events such as role changes, new high-risk access grants or policy violations).
- Identity Governance provides **risk-based prioritisation** in certification campaigns, highlighting high-risk entitlements that require the most scrutiny and allowing reviewers to focus their effort where it matters most.
- The **business-friendly reviewer interface** presents access information in non-technical terms, with contextual information (role descriptions, peer comparison, last access date) to help reviewers make informed decisions.
- Identity Governance supports **multi-level approval chains** and **escalation policies** for certifications that are not completed within the review period.
- Integration with **Identity Manager** ensures that certification decisions (deny, revoke) are **automatically enforced** across target systems without manual IT intervention.

**Key OpenText Technologies:**
| Technology | Purpose |
|------------|---------|
| Identity Governance (Full Campaigns) | Organisation-wide access certification |
| Micro-certifications | Event-driven, targeted access reviews |
| Risk-based Prioritisation | Focus reviewer attention on high-risk entitlements |
| Multi-level Approval and Escalation | Configurable approval chains with escalation |
| Auto-remediation via Identity Manager | Automatic enforcement of certification decisions |

---

### 9. Identity as an Attack Surface

**Challenge:** Threat actors target identity infrastructure as a primary attack vector.

**OpenText Guidance:**

- **OpenText ArcSight** provides **enterprise SIEM capabilities** with pre-built correlation rules for identity-based threats including brute force attacks, credential stuffing, impossible travel, privilege escalation, lateral movement and anomalous sign-in patterns.
- **OpenText Change Guardian** monitors **Active Directory** in real time for unauthorised changes to privileged groups (Domain Admins, Enterprise Admins), Group Policy Objects, trust relationships and schema modifications — detecting attacks such as DCSync, Golden Ticket and AdminSDHolder manipulation.
- **OpenText Advanced Authentication** with **risk-based adaptive authentication** evaluates contextual signals at sign-in time and can step up authentication requirements or block access when anomalous behaviour is detected.
- **OpenText Privileged Account Manager** reduces the attack surface by vaulting privileged credentials, enforcing just-in-time access and recording all privileged sessions for forensic analysis.

> **Partial gap:** OpenText does not provide a direct equivalent to **cloud identity threat detection** capabilities such as Entra ID Identity Protection (leaked credential detection, real-time sign-in risk scoring, token theft detection). For organisations using Entra ID, Microsoft's native Identity Protection capabilities would supplement OpenText's on-premises focused detection. ArcSight can ingest and correlate Entra ID sign-in and audit logs for unified monitoring.

**Key OpenText Technologies:**
| Technology | Purpose |
|------------|---------|
| ArcSight (Identity Threat Detection) | SIEM correlation rules for identity-based attacks |
| Change Guardian (AD Monitoring) | Real-time detection of AD-level attacks |
| Advanced Authentication (Adaptive) | Risk-based step-up authentication at sign-in |
| Privileged Account Manager | Reduce attack surface via credential vaulting and JIT access |

---

### 10. Legacy System Integration

**Challenge:** Older systems do not support modern authentication protocols.

**OpenText Guidance:**

- **OpenText Access Manager** provides a **reverse proxy (Access Gateway)** that sits in front of legacy web applications and handles authentication on their behalf. The Access Gateway can inject credentials, headers or cookies into the backend application without modifying the application code.
- Access Manager supports **Kerberos Constrained Delegation** for SSO to Windows Integrated Authentication (WIA) applications and can perform protocol translation from modern federation protocols (SAML, OIDC) to legacy authentication mechanisms.
- For non-web legacy applications, Access Manager's **desktop agent** provides credential injection SSO for thick-client applications.
- **OpenText Identity Manager** provides **connectors for legacy systems** including mainframes (RACF, ACF2, Top Secret), AS/400, Lotus Notes, legacy LDAP directories and custom databases via JDBC, enabling identity lifecycle management for systems that do not support modern provisioning protocols.

**Key OpenText Technologies:**
| Technology | Purpose |
|------------|---------|
| Access Manager (Access Gateway) | Reverse proxy SSO for legacy web apps |
| Kerberos Constrained Delegation | Protocol translation for WIA applications |
| Desktop Agent | SSO for thick-client and non-web applications |
| Identity Manager (Legacy Connectors) | Provisioning for mainframes, AS/400, Notes and legacy directories |

---

### 11. Zero Trust Adoption

**Challenge:** Transitioning from perimeter-based security to identity-centric Zero Trust architecture.

**OpenText Guidance:**

- **OpenText Access Manager** serves as a **policy enforcement point** that evaluates access requests based on identity, authentication strength, device context and risk score — aligning with Zero Trust principles of "never trust, always verify".
- Access Manager's built-in **risk engine** continuously scores each access request based on contextual factors (location, device, time, behaviour), enforcing adaptive policies that step up authentication or block access when risk is elevated.
- **OpenText Advanced Authentication** enables **continuous authentication** by re-evaluating user identity throughout the session using behavioural biometrics and contextual signals.
- **OpenText Identity Governance** supports the Zero Trust principle of **least privilege** by continuously identifying and remediating excessive entitlements through certifications, role mining and analytics.

> **Partial gap:** OpenText does not offer native **Zero Trust Network Access (ZTNA)** capabilities equivalent to Microsoft Global Secure Access or similar products that replace VPN with identity-aware network-level access controls. For ZTNA, organisations would need to supplement with a dedicated ZTNA product (e.g. Zscaler, Cloudflare, Palo Alto Prisma). OpenText's contribution to Zero Trust is focused on the **identity verification, authentication and governance** pillars rather than network access.

**Key OpenText Technologies:**
| Technology | Purpose |
|------------|---------|
| Access Manager (Policy Enforcement) | Identity and context-based access decisions |
| Risk Engine (Access Manager) | Continuous risk scoring for adaptive access |
| Advanced Authentication (Continuous) | Session-level re-authentication and behavioural biometrics |
| Identity Governance (Least Privilege) | Continuous entitlement rationalisation |

---

### 12. Cross-Domain Identity Federation

**Challenge:** Establishing trust with partners, suppliers and customers across identity domains.

**OpenText Guidance:**

- **OpenText Access Manager** provides comprehensive **federation capabilities** supporting SAML 2.0, OAuth 2.0, OpenID Connect and WS-Federation as both an identity provider (IdP) and a service provider (SP), enabling trust relationships with external organisations.
- Access Manager includes a **preconfigured connector catalogue** for common federation targets (Microsoft 365, AWS, Google Workspace, Salesforce) and a toolkit for custom federation configurations.
- Access Manager supports **identity provider chaining**, where an external user authenticates with their own organisation's IdP and Access Manager trusts the assertion, enabling seamless cross-organisational access without duplicating external accounts.
- **OpenText Identity Manager** can provision and deprovision **external partner accounts** in local directories based on federated identity data, maintaining a governed lifecycle for cross-domain identities.

> **Not directly addressed:** OpenText does not offer a purpose-built **customer identity and access management (CIAM)** platform for B2C scenarios (self-service registration, social login, progressive profiling, consent management) equivalent to Microsoft Entra External ID or dedicated CIAM products (e.g. Okta CIC, Ping Identity). For B2C identity, organisations would need a dedicated CIAM solution.

**Key OpenText Technologies:**
| Technology | Purpose |
|------------|---------|
| Access Manager (Federation) | SAML, OAuth, OIDC and WS-Fed federation as IdP and SP |
| Connector Catalogue | Pre-built federation integrations |
| IdP Chaining | Trust external identity providers without duplicating accounts |
| Identity Manager (External Lifecycle) | Governed lifecycle for cross-domain partner identities |

---

### 13. Identity Data Quality

**Challenge:** Inconsistent, incomplete or outdated identity attributes undermine automation.

**OpenText Guidance:**

- **OpenText Identity Manager** provides **attribute mapping, transformation and normalisation rules** across all connected systems. The Identity Vault serves as the master record, and synchronisation policies enforce consistent attribute formats across HR, AD, LDAP and SaaS directories.
- Identity Manager's **HR driver** (Workday, SAP HR, ADP) establishes the HR system as the **authoritative source** for core identity attributes, ensuring that all downstream systems reflect accurate HR data.
- **Data validation rules** in Identity Manager can reject or flag provisioning events where required attributes are missing or malformed, preventing bad data from propagating to target systems.
- **OpenText Identity Governance** serves as a data quality indicator: if role-based or attribute-based access assignments are producing incorrect results, it signals that the underlying attribute data needs correction.

**Key OpenText Technologies:**
| Technology | Purpose |
|------------|---------|
| Identity Manager (Attribute Mapping) | Consistent attribute transformation and normalisation |
| HR Driver (Authoritative Source) | HR system as the single source of truth for core attributes |
| Data Validation Rules | Prevent propagation of incomplete or malformed data |
| Identity Governance (Quality Indicator) | Identify data quality issues through governance outcomes |

---

### 14. Separation of Duties Conflicts

**Challenge:** Ensuring no single identity holds conflicting permissions at scale.

**OpenText Guidance:**

- **OpenText Identity Governance** provides comprehensive **separation of duties (SoD) management**. Organisations define SoD rules specifying which entitlements, roles or group memberships are mutually exclusive (e.g. "Accounts Payable Clerk" and "Accounts Payable Approver" cannot be held simultaneously).
- Identity Governance **enforces SoD rules at request time**: when a user requests access that would create a conflict, the request is either blocked or routed to a designated approver who can grant an exception with documented justification.
- **SoD violation detection** identifies existing conflicts in the current access landscape, generating reports of all users who currently hold conflicting entitlements so that remediation can be planned.
- Identity Governance supports **configurable risk levels** for SoD violations (low, medium, high, critical), allowing organisations to prioritise remediation of the most impactful conflicts.
- Integration with **Identity Manager** ensures that SoD rules are also enforced at the **provisioning layer**, preventing conflicting access from being granted through automated role-based provisioning.

**Key OpenText Technologies:**
| Technology | Purpose |
|------------|---------|
| Identity Governance (SoD Rules) | Define mutually exclusive entitlement combinations |
| Request-time Enforcement | Block or escalate access requests that create SoD conflicts |
| SoD Violation Detection | Identify existing conflicts in the current access landscape |
| Risk-level Classification | Prioritise remediation by SoD violation severity |
| Identity Manager (SoD at Provisioning) | Enforce SoD rules during automated provisioning |

---

## Summary: OpenText Coverage vs Gaps

| Capability | OpenText Product | Coverage Level |
|------------|-----------------|----------------|
| Identity Lifecycle (JML) | Identity Manager | Full |
| Access Certification | Identity Governance | Full |
| Separation of Duties | Identity Governance | Full |
| Single Sign-On (SSO) | Access Manager | Full |
| Multi-Factor Authentication | Advanced Authentication | Full |
| Privileged Access Management | Privileged Account Manager | Full |
| Delegated AD Administration | DRA | Full |
| AD Change Monitoring | Change Guardian | Full |
| SIEM / Compliance Reporting | ArcSight | Full |
| HR-driven Provisioning | Identity Manager (HR Drivers) | Full |
| Legacy Application SSO | Access Manager (Gateway/Agent) | Full |
| Shadow IT Discovery | No dedicated product | Gap |
| Cloud-native Workload Identity | No dedicated product | Gap |
| ZTNA / VPN Replacement | No dedicated product | Gap |
| Customer Identity (CIAM / B2C) | No dedicated product | Gap |
| Cloud Identity Threat Detection | ArcSight (via log ingestion) | Partial |
| Cloud Tenant Management (M&A) | Identity Manager (Metadirectory) | Partial |
| Identity Data Residency Controls | Infrastructure-level only | Partial |
