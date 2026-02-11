# Microsoft SharePoint: Site Management and Lifecycle Options

**Date:** 11 February 2026

---

## Overview

SharePoint Online sites are a core collaboration and content management surface in Microsoft 365. Every site has an identity backing in Entra ID and an associated governance lifecycle. This document examines the options for SharePoint site creation, management, access assignment and lifecycle — ranging from fully self-service user requests through to centrally controlled manual provisioning.

---

## 1. SharePoint Site Types and Their Identity Backing

Before examining management options, it is important to understand the different site types and how each relates to identity objects in Entra ID and Active Directory.

| Site Type | Entra ID Backing | Created By | Default Use Case |
|-----------|-----------------|------------|------------------|
| **Team Site (M365 Group-connected)** | Microsoft 365 Group (unified group) — provides membership, shared mailbox, Teams team, Planner | Users, Teams, Outlook, admin | Department collaboration, project work, team file storage |
| **Team Site (no group)** | SharePoint site only — no M365 group, no shared mailbox, no Teams team | Admin only (PowerShell or admin centre) | Structured content without group collaboration overhead |
| **Communication Site** | No M365 group — standalone SharePoint site with site owner/member/visitor permissions | Users or admin | Intranet pages, news, broadcasts, organisational announcements |
| **Hub Site** | No additional identity object — hub registration associates existing sites | Admin only | Navigation and content aggregation across related sites |
| **Channel Site** | Linked to Teams private/shared channel — inherits from parent M365 group with channel-specific permissions | Teams (automatic on private/shared channel creation) | Dedicated content for private or shared Teams channels |

### Identity Implications

| Aspect | Group-Connected Team Site | Non-Group Team Site | Communication Site |
|--------|--------------------------|--------------------|--------------------|
| **Membership model** | M365 group owners and members — managed in Entra ID | SharePoint permission groups (owners, members, visitors) — managed in SharePoint only | SharePoint permission groups — managed in SharePoint only |
| **Entra ID governance** | Full — group expiration, access reviews, dynamic membership, sensitivity labels, Lifecycle Workflows | Limited — no group governance; permissions managed per-site | Limited — no group governance; permissions managed per-site |
| **Guest access** | Controlled via M365 group settings AND Entra ID external collaboration settings | Controlled via SharePoint sharing settings | Controlled via SharePoint sharing settings |
| **Dynamic membership** | Supported via Entra ID dynamic group rules (P1 licence) | Not supported — manual assignment only | Not supported — manual assignment only |
| **Teams integration** | Automatic — creating a Teams team creates a group-connected team site | None | None |

---

## 2. Site Creation Options

### 2.1 User Self-Service (Default)

**How it works:** Any licensed user can create SharePoint sites directly from the SharePoint start page, Outlook (when creating an M365 group) or Teams (when creating a team).

| Aspect | Detail |
|--------|--------|
| **Who can create** | All licensed M365 users by default |
| **What is created** | Group-connected team site (via Teams or Outlook) or communication site (via SharePoint start page) |
| **Approval** | None — instant creation |
| **Naming** | User chooses any name; M365 group naming policy applied if configured (prefix/suffix) |
| **Classification** | User selects sensitivity label if mandatory labelling is enabled; otherwise no classification |
| **Governance** | M365 group expiration policy applies to group-connected sites; no expiration for communication sites |

**Advantages:**
- Maximum agility — users create collaboration spaces when needed without waiting
- Low IT overhead — no ticket queue or approval bottleneck
- Native integration — creating a Teams team automatically creates the site

**Disadvantages:**
- Site sprawl — hundreds or thousands of sites created with no lifecycle plan
- Inconsistent structure — no standard libraries, metadata or information architecture
- Storage consumption — each site consumes tenant storage quota
- Naming inconsistency — similar sites created with different names
- Governance gap — communication sites have no group backing and no expiration

**Controls to Apply:**
- Restrict M365 group creation to a security group (Entra ID group creation policy) to limit team site creation
- Enforce mandatory sensitivity labels on site creation
- Apply M365 group naming policies (prefixes/suffixes)
- Enable M365 group expiration (90, 180 or 365 days)
- Configure default sharing links (e.g. "people in your organisation" rather than "anyone")

---

### 2.2 Managed Self-Service with Approval

**How it works:** Users request sites through a governed process. Requests are reviewed and approved before the site is provisioned. This can be implemented via several mechanisms.

#### Option A: Entra ID Entitlement Management (Access Packages)

| Aspect | Detail |
|--------|--------|
| **Mechanism** | Create an access package catalogue for SharePoint sites. Each access package grants membership to an M365 group that backs a team site |
| **Request process** | Users browse the My Access portal and request an access package. Alternatively, auto-assignment policies assign packages based on user attributes |
| **Approval workflow** | Multi-stage approval: line manager → site owner or IT governance team |
| **Time-bound access** | Access packages can have expiry dates (e.g. project end date), after which access is automatically removed |
| **Access reviews** | Periodic recertification of access package assignments |
| **Licence** | Entra ID Governance (or Entra ID P2 for basic access reviews) |

**Best for:** Controlling access to existing sites; particularly effective for project-based or time-limited site access.

**Limitation:** Entitlement Management manages access to sites, not site creation itself. Site provisioning must be handled separately (manually or via automation triggered by access package assignment).

#### Option B: Microsoft Forms + Power Automate

| Aspect | Detail |
|--------|--------|
| **Mechanism** | Users submit a site request via a Microsoft Form (site name, purpose, classification, owner, expected lifetime) |
| **Approval workflow** | Power Automate flow routes the request to an approver (IT, governance team or line manager) |
| **Provisioning** | On approval, Power Automate calls the SharePoint REST API or Microsoft Graph API to create the site with standardised settings (template, libraries, metadata, permissions) |
| **Naming** | Enforced by the flow — applies naming conventions automatically |
| **Classification** | Sensitivity label applied automatically based on the requester's selection |
| **Audit trail** | Form submissions and approval decisions logged in the flow run history |

**Best for:** Organisations wanting lightweight, customisable governance without third-party tooling.

**Limitation:** Power Automate flows require maintenance; complex provisioning logic (multiple templates, conditional configurations) can become difficult to manage.

#### Option C: ServiceNow / ITSM Integration

| Aspect | Detail |
|--------|--------|
| **Mechanism** | Users raise a service request in the ITSM platform (ServiceNow, BMC, Jira Service Management) |
| **Approval workflow** | ITSM approval workflow routes to IT, information governance or the requesting user's manager |
| **Provisioning** | ITSM calls Microsoft Graph API (via integration connector or custom script) to create the site on approval |
| **CMDB** | Site registered as a configuration item (CI) in the CMDB with owner, classification, review date and decommission date |
| **Audit trail** | Full ITSM ticket history for compliance evidence |

**Best for:** Organisations with mature ITSM practices and a requirement for sites to be tracked as managed assets.

**Limitation:** Higher implementation effort; ITSM integration with Graph API requires development and maintenance.

#### Option D: Third-Party Governance Platforms

| Platform | Approach |
|----------|----------|
| **ShareGate** | Site lifecycle management including provisioning, policy enforcement, clean-up and archival |
| **AvePoint Cloud Governance** | Request portal with approval workflows, automated provisioning with templates, lifecycle management and policy enforcement |
| **Rencore Governance** | Governance-as-code approach with automated policy checking and remediation |
| **CoreView** | M365 management platform with delegated site provisioning and lifecycle automation |
| **Orchestry** | Workspace provisioning with templates, naming, classification and lifecycle built for M365 |

**Best for:** Organisations requiring enterprise-grade governance beyond native M365 capabilities.

---

### 2.3 Centralised IT Provisioning (Manual)

**How it works:** Only IT administrators create SharePoint sites. Users submit requests via email, ticket or form, and IT provisions the site manually.

| Aspect | Detail |
|--------|--------|
| **Who can create** | SharePoint Administrators or Global Administrators only |
| **Request process** | User raises a request (email, ticket, verbal) with site name, purpose, owner and required permissions |
| **Provisioning** | Admin creates the site via SharePoint Admin Centre or PowerShell with standardised settings |
| **Naming** | Enforced manually by the admin following a naming convention document |
| **Classification** | Admin applies the sensitivity label based on the stated purpose |
| **Permissions** | Admin configures owners, members and visitors based on the request |
| **Timeline** | Depends on IT workload — typically hours to days |

**Advantages:**
- Full control over site creation, naming, classification and structure
- Every site is a deliberate, documented decision
- Consistent information architecture across all sites

**Disadvantages:**
- Bottleneck — IT becomes a constraint on user productivity
- Delay — users wait for provisioning, leading to shadow IT (personal OneDrive sharing, email attachments, third-party tools)
- Scaling problem — does not scale beyond a few hundred sites without automation
- Manual errors — inconsistent configuration when relying on individual admins

---

### 2.4 Automated Provisioning (Event-Driven)

**How it works:** Sites are provisioned automatically based on business events without any user request or IT intervention.

| Trigger | Mechanism | Example |
|---------|-----------|---------|
| **New project created** | Project management system (Project Online, Planner, third-party PPM) triggers Graph API call | New project approved → team site created with project template, document libraries and metadata |
| **New department or team** | HR system event (new department created) triggers Entra ID dynamic group → M365 group → team site | New department in Workday → Entra ID group created → site provisioned automatically |
| **New hire** | Lifecycle Workflow joiner task adds user to relevant M365 groups | New hire starts → added to department M365 group → gains access to department team site |
| **Academic term start** | Student Information System (SIS) triggers School Data Sync → class teams created | New semester → class teams and associated SharePoint sites created for each course |
| **Client engagement** | CRM system (Dynamics 365, Salesforce) triggers site creation for new client projects | New client deal closed → client collaboration site provisioned with standard template |

**Advantages:**
- Zero delay — sites available when needed
- Consistent — every site follows the template
- Scalable — handles hundreds or thousands of sites without IT bottleneck

**Disadvantages:**
- Requires integration development and maintenance
- Trigger data quality matters — incorrect HR data creates incorrect sites
- Over-provisioning risk — sites created for every event even when not needed

---

## 3. Permission and Access Assignment Options

Once a site exists, access must be assigned and maintained. The following options range from manual to fully automated.

### 3.1 Manual Permission Assignment

| Method | Detail |
|--------|--------|
| **SharePoint site permissions UI** | Site owners add/remove users directly via the site settings permissions page |
| **SharePoint Admin Centre** | SharePoint admins manage site collection administrators and permissions centrally |
| **PowerShell (PnP / SPO)** | `Add-PnPGroupMember`, `Set-SPOSite`, `Set-SPOUser` for scripted bulk changes |

**Governance challenges:**
- No approval workflow — owners add anyone without oversight
- No expiry — access granted indefinitely
- No audit — changes logged in SharePoint audit log but rarely reviewed
- No recertification — no one validates whether access is still appropriate

---

### 3.2 M365 Group Membership (Group-Connected Sites)

| Method | Detail |
|--------|--------|
| **Direct group membership** | Add users to the M365 group in Entra ID — they automatically receive site member access |
| **Dynamic group membership** | Define Entra ID dynamic membership rules (e.g. `department -eq "Finance"`) — users are added/removed automatically as attributes change |
| **Entra ID Access Packages** | Users request access via My Access portal → approval workflow → time-bound group membership → automatic removal on expiry |
| **Access Reviews** | Periodic recertification of M365 group membership — unconfirmed members automatically removed |
| **Lifecycle Workflows** | Joiner/mover/leaver workflows add or remove group memberships based on HR events |

**Governance advantages:**
- Single membership source — group membership governs site access, Teams access, shared mailbox access and Planner access simultaneously
- Full Entra ID governance — expiration, access reviews, dynamic membership, Conditional Access
- Audit trail — Entra ID audit logs capture all membership changes

---

### 3.3 SharePoint Permission Groups (Non-Group Sites)

For team sites without a group connection and communication sites, permissions are managed via SharePoint's own permission groups.

| Permission Group | Default Role | Typical Assignment |
|-----------------|-------------|-------------------|
| **Site Owners** | Full control — manage site settings, permissions, structure | Site administrators, project leads |
| **Site Members** | Edit — add/edit/delete content in lists and libraries | Team members, contributors |
| **Site Visitors** | Read — view content only | Stakeholders, auditors, wider organisation |

**Governance challenges:**
- No Entra ID governance — SharePoint permission groups are not subject to access reviews, expiration or dynamic membership
- Separate management surface — permissions managed in SharePoint, not in Entra Admin Centre
- Granular but complex — item-level and library-level permission breaking creates intricate, hard-to-audit access structures
- No time-bound access — no native expiry for SharePoint permission group membership

**Mitigation:**
- Assign Entra ID security groups to SharePoint permission groups rather than individual users — this brings membership under Entra ID control
- Use Entra ID access reviews targeting the security groups used for site permissions
- Avoid breaking permission inheritance at item or library level where possible

---

### 3.4 Sharing Links

SharePoint allows content sharing via links with varying scope and governance implications.

| Link Type | Scope | Identity Requirement | Governance Risk |
|-----------|-------|---------------------|----------------|
| **Anyone (anonymous)** | No authentication required | None — completely anonymous | Highest risk — no audit trail, no access control, no expiry enforcement |
| **People in your organisation** | Any authenticated user in the tenant | Entra ID authenticated user | Moderate — any internal user can access, even if not a site member |
| **People with existing access** | Only users who already have site permissions | Existing site member/visitor | Lowest risk — does not grant new access |
| **Specific people** | Named individuals (internal or external) | Entra ID user or guest account | Moderate — grants access to named individuals; guest accounts created for external recipients |

**Governance controls:**
- Disable **Anyone links** at the tenant or site level via SharePoint Admin Centre
- Set **default link type** to "People with existing access" or "Specific people"
- Enforce **link expiration** (e.g. 30 days for external sharing links)
- Require **re-authentication** for external access via Conditional Access session controls
- Use **Microsoft Purview DLP** to block sharing of classified or sensitive content via links
- Monitor sharing activity via the **SharePoint sharing report** and **M365 unified audit log**

---

### 3.5 Guest and External Access

| Method | Detail |
|--------|--------|
| **Entra ID B2B guest invitation** | External users invited as Entra ID guest accounts — can be added to M365 groups or SharePoint permission groups |
| **SharePoint external sharing** | Sharing a file or site with an external email address triggers a guest account creation in Entra ID (or a one-time passcode if B2B guest accounts are not enabled) |
| **Access Packages for externals** | External users request access via a published access package link — sponsor approval → time-bound access → automatic removal |
| **Cross-tenant access** | Entra ID cross-tenant access settings control which external tenants can access shared sites, with trust settings for MFA and device compliance |

**Governance controls:**
- Restrict external sharing per site (SharePoint Admin Centre: tenant-level and site-level sharing settings)
- Set **guest access expiration** (Entra ID: 30, 60 or 90 days with access review re-confirmation)
- Apply **Conditional Access** for guest users (require MFA, Terms of Use acceptance, block unmanaged devices)
- Apply **sensitivity labels** that block external sharing for confidential sites

---

## 4. Site Lifecycle Management

### 4.1 Lifecycle Stages

| Stage | Description | Governance Actions |
|-------|-------------|-------------------|
| **Request** | User or system initiates a site creation request | Validate purpose, classification, owner and expected lifetime |
| **Approval** | Request reviewed by governance authority (manager, IT, information owner) | Verify alignment with policies; prevent duplication; assign classification |
| **Provisioning** | Site created with standardised template, libraries, metadata, permissions and classification | Apply naming convention, sensitivity label, default sharing settings and retention policy |
| **Active Use** | Site is in active use by its members | Monitor storage, sharing activity, guest access and compliance; enforce DLP and retention |
| **Review** | Periodic recertification of site purpose, ownership and membership | Owner confirms continued need; access reviews validate membership; unused sites flagged |
| **Archival** | Site no longer actively used but content must be retained | Set site to read-only; remove active permissions; apply long-term retention label; reduce storage tier |
| **Deletion** | Retention period expired; content no longer needed | Delete site; soft-delete allows 93-day recovery; permanent deletion after retention |

---

### 4.2 Lifecycle Options Comparison

| Option | Request | Approval | Provisioning | Review | Archival | Deletion |
|--------|---------|----------|-------------|--------|---------|---------|
| **Full self-service** | User creates directly | None | Instant (user-configured) | M365 group expiration only (group-connected sites) | Manual by owner | Group expiration or manual |
| **Managed self-service (Forms + Power Automate)** | User submits form | Power Automate approval flow | Automated via Graph API on approval | Custom Power Automate reminder flows | Custom flow sets site to read-only | Custom flow or manual |
| **Managed self-service (Entitlement Management)** | User requests access package | Multi-stage Entra ID approval | Access granted via group membership (site must pre-exist) | Entra ID access reviews | Not directly supported — separate process | Access package expiry removes membership |
| **ITSM-driven** | Service request ticket | ITSM approval workflow | ITSM automation via Graph API | ITSM scheduled review tasks | ITSM change request → archive action | ITSM change request → delete action |
| **Third-party governance platform** | Platform request portal | Platform approval workflow | Platform provisioning engine with templates | Platform automated review campaigns | Platform archival with policy enforcement | Platform automated deletion with retention compliance |
| **Centralised IT (manual)** | Email / ticket to IT | IT team decision | Manual admin provisioning | Manual — calendar reminders or spreadsheet tracking | Manual — admin sets read-only | Manual — admin deletes |
| **Event-driven automation** | No request — triggered by business event | No approval — policy-based | Automatic via integration | Tied to source event lifecycle (project close, term end, client offboarding) | Automatic on source event completion | Automatic after retention period |

---

### 4.3 M365 Group Expiration (Group-Connected Sites)

The primary native lifecycle tool for group-connected team sites.

| Setting | Detail |
|---------|--------|
| **Policy scope** | All M365 groups, selected groups, or no groups |
| **Expiration period** | 90, 180 or 365 days (custom periods not supported) |
| **Renewal** | Owners receive email notifications at 30, 15 and 1 day before expiry. Accessing the group (Teams, SharePoint, Outlook, Yammer) can auto-renew if activity-based renewal is enabled |
| **Failure to renew** | Group soft-deleted → 30-day recovery period → permanent deletion (including SharePoint site, mailbox, Teams team, Planner and all content) |
| **Activity-based renewal** | If enabled, any user activity in the group's resources (Teams message, SharePoint file access, Outlook email, Yammer post) automatically renews the group, preventing expiry of actively used groups |
| **Limitation** | Only applies to M365 group-connected sites. Communication sites, non-group team sites and hub sites have NO native expiration mechanism |

---

### 4.4 Site Lifecycle for Non-Group Sites

Communication sites and non-group team sites have no native lifecycle management. Options include:

| Approach | Detail |
|----------|--------|
| **Custom Power Automate flows** | Scheduled flow queries SharePoint API for sites with no activity in the last 90/180 days → notifies site owner → archives or deletes if no response |
| **SharePoint Admin Centre inactive sites report** | Identifies sites with no activity — admin reviews and takes manual action |
| **Third-party governance tools** | AvePoint, ShareGate, Rencore and Orchestry provide lifecycle policies for all site types including communication sites |
| **Custom Graph API automation** | Azure Function or Azure Automation runbook queries site activity via Graph API, applies policy logic, and archives or deletes sites based on rules |
| **Manual register and review** | Maintain a site register (SharePoint list or CMDB) with review dates; site owners certify continued need at each review |

---

## 5. Storage and Quota Management

Storage is a shared tenant resource. Unmanaged site proliferation directly impacts cost and capacity.

| Aspect | Detail |
|--------|--------|
| **Tenant storage pool** | 1 TB base + 10 GB per licensed user (pooled across all sites) |
| **Default site quota** | 25 TB per site collection (configurable) |
| **Monitoring** | SharePoint Admin Centre storage report; per-site storage consumption visible in admin centre |
| **Governance controls** | Set per-site storage limits; alert on high-consumption sites; implement retention policies to auto-delete aged content; use archive sites with lower quotas for read-only content |

---

## 6. Retention and Compliance

| Requirement | Microsoft Tool | Detail |
|-------------|---------------|--------|
| **Retention policies** | Microsoft Purview retention policies | Apply to all SharePoint sites or specific sites; retain content for defined period then delete |
| **Retention labels** | Microsoft Purview retention labels | Apply to individual documents or libraries for content-specific retention (e.g. contracts retained 7 years) |
| **Legal hold** | Microsoft Purview eDiscovery hold | Preserve all content in a site regardless of retention policy for litigation or regulatory purposes |
| **DLP** | Microsoft Purview DLP | Detect and block sharing of sensitive content (PII, financial data, health records) in SharePoint |
| **Sensitivity labels** | Microsoft Purview sensitivity labels | Classify sites and documents; enforce encryption, access restrictions and sharing controls |
| **Audit** | M365 unified audit log | All SharePoint activities (file access, sharing, permission changes, site creation/deletion) logged |

---

## 7. Decision Framework: Choosing the Right Approach

| Factor | Self-Service | Managed Self-Service | Centralised IT | Event-Driven Automation |
|--------|-------------|---------------------|----------------|------------------------|
| **Organisation size** | Small (< 50) | Medium (50–500) | Any (but does not scale beyond ~500 sites) | Large (500+) |
| **IT maturity** | Low | Medium | Any | High |
| **Compliance requirements** | Low | Medium–High | High | High |
| **User productivity priority** | Highest | High | Lowest | Highest |
| **Governance rigour** | Lowest | High | Highest (if manual controls are followed) | High (if automation is well-designed) |
| **Shadow IT risk** | Lowest (users get what they need) | Low | Highest (users bypass IT with workarounds) | Lowest |
| **Ongoing maintenance** | Low (but governance debt accumulates) | Medium (flows, forms, approvals to maintain) | High (manual effort for every site) | Medium (integration maintenance) |
| **Recommended for** | Start-ups, small teams with low compliance needs | Most organisations as a balanced approach | Highly regulated environments with small site counts | Organisations with mature integration capabilities and high site volumes |

---

## 8. Summary: Key Governance Recommendations

| Recommendation | Detail |
|----------------|--------|
| **Prefer group-connected team sites** | M365 group backing provides the richest governance: Entra ID access reviews, group expiration, dynamic membership, sensitivity labels and Conditional Access |
| **Restrict creation, not collaboration** | Limit who can create M365 groups (and therefore team sites) but make the request process fast and frictionless |
| **Enforce sensitivity labels at creation** | Mandatory labelling ensures every site has a classification from day one, driving downstream protection policies |
| **Set group expiration** | Enable activity-based renewal with 365-day expiration as a safety net — actively used sites renew automatically; abandoned sites expire |
| **Govern communication sites separately** | Communication sites have no group backing and no native lifecycle. Implement custom automation or third-party tooling for lifecycle management |
| **Assign Entra ID groups to SharePoint permissions** | Even for non-group sites, use Entra ID security groups (not individual users) in SharePoint permission groups to bring membership under Entra ID governance |
| **Review guest access regularly** | Use Entra ID access reviews targeting guest users in M365 groups; set guest expiration policies; monitor external sharing reports |
| **Plan for archival** | Define an archival process (read-only, reduced permissions, long-term retention label) as an alternative to deletion for sites with compliance obligations |
| **Monitor storage proactively** | Set per-site quotas; alert on high-growth sites; use retention policies to control content volume |
| **Document everything** | Maintain a site register (even a SharePoint list) with owner, purpose, classification, creation date, review date and expected decommission date |
