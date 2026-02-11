# Microsoft SharePoint — Site Management and Lifecycle Options

**Date:** 11 February 2026

---

## Overview

This document examines the available options for managing **SharePoint Online site provisioning, access and lifecycle** — from fully manual administration through to automated, policy-driven management. The right approach depends on organisation size, licensing, governance maturity, compliance requirements and the type of site (team site, communication site, hub site or project site).

SharePoint Online sites are tightly coupled with **Microsoft 365 Groups** and **Microsoft Entra ID**. A group-connected team site inherits its membership from the backing M365 Group, making SharePoint site governance fundamentally an identity management activity.

---

## 1. SharePoint Site Types and Their Identity Backing

### 1.1 Site Types

| Site Type | Backing Identity Object | Membership Model | Primary Purpose |
|-----------|------------------------|------------------|-----------------|
| **Group-connected Team Site** | Microsoft 365 Group (Entra ID Unified Group) | M365 Group owners and members | Team collaboration — files, lists, pages tied to a team |
| **Team Site (no group)** | No M365 Group — site-level permissions only | SharePoint permission groups (Owners, Members, Visitors) | Legacy or isolated team collaboration without M365 Group features |
| **Communication Site** | No M365 Group — site-level permissions only | SharePoint permission groups; often wide read access | Broadcasting news, reports and information to a broad audience |
| **Hub Site** | No M365 Group — registered as hub in SharePoint Admin Centre | Hub association policy controls which sites can join | Organising related sites under a common navigation and search scope |
| **Channel Site (Private Channel)** | Separate M365 Group (auto-created) | Private channel membership from Teams | Private channel file storage — separate from the parent team site |
| **Channel Site (Shared Channel)** | Entra ID B2B direct connect | Shared channel membership (cross-tenant) | Cross-tenant shared channel file storage |

### 1.2 Identity Implications

| Aspect | Group-Connected Team Site | Non-Group Team Site | Communication Site |
|--------|--------------------------|--------------------|--------------------|
| **Membership model** | M365 group owners and members — managed in Entra ID | SharePoint permission groups (owners, members, visitors) — managed in SharePoint only | SharePoint permission groups — managed in SharePoint only |
| **Entra ID governance** | Full — group expiration, access reviews, dynamic membership, sensitivity labels, Lifecycle Workflows | Limited — no group governance; permissions managed per-site | Limited — no group governance; permissions managed per-site |
| **Guest access** | Controlled via M365 group settings AND Entra ID external collaboration settings | Controlled via SharePoint sharing settings | Controlled via SharePoint sharing settings |
| **Dynamic membership** | Supported via Entra ID dynamic group rules (P1 licence) | Not supported — manual assignment only | Not supported — manual assignment only |
| **Teams integration** | Automatic — creating a Teams team creates a group-connected team site | None | None |

### 1.3 SharePoint Permission Levels

| Permission Level | Capabilities | Typical Assignment |
|-----------------|-------------|-------------------|
| **Full Control** | Manage all site settings, permissions, content and structure | Site collection administrators; site owners |
| **Design** | Create lists and document libraries; edit pages; apply themes | Intranet design team |
| **Edit** | Add, edit and delete list items and documents | Team members |
| **Contribute** | Add, edit and delete list items and documents (no list/library creation) | Standard contributors |
| **Read** | View pages and documents only | Visitors; information consumers |
| **View Only** | View pages and documents; cannot download | Restricted external viewers |

### 1.4 Permission Group to Identity Object Mapping

| SharePoint Permission Group | Group-Connected Team Site | Non-Group Site | Identity Object |
|----------------------------|--------------------------|----------------|-----------------|
| **Site Owners** | M365 Group owners | SharePoint security group (site-specific) | Entra ID users/groups or SharePoint groups |
| **Site Members** | M365 Group members | SharePoint security group (site-specific) | Entra ID users/groups or SharePoint groups |
| **Site Visitors** | Separately managed (not M365 Group) | SharePoint security group (site-specific) | Entra ID users/groups or SharePoint groups |

---

## 2. Site Provisioning Options

### Option 1: Manual Creation by End Users (Self-Service)

**How it works:** Users create SharePoint sites directly from the SharePoint start page, Microsoft Teams (which auto-creates a group-connected team site) or Outlook (when creating an M365 Group).

| Attribute | Detail |
|-----------|--------|
| **Who provisions** | Any licensed user (unless restricted) |
| **Interface** | SharePoint start page → Create site; Teams → Create team (auto-creates site); Outlook → Create group |
| **Trigger** | User decides they need a collaboration space |
| **Approval workflow** | None by default — site is created immediately |
| **Automation** | None |
| **Licensing** | Any M365 licence that includes SharePoint |
| **Best suited for** | Small organisations; agile teams; low-governance environments |

**Advantages:**
- Instant — users get a site immediately when they need one
- No IT involvement or ticket required
- Encourages adoption and self-sufficiency

**Challenges:**
- Site sprawl — hundreds or thousands of ungoverned sites created over time
- No consistent naming, classification or metadata
- No defined owner accountability or lifecycle policy
- Sensitive data may be stored in sites without appropriate protection
- Sites created via Teams inherit Teams governance gaps
- No site classification or sensitivity labelling enforced at creation

**Identity Governance Gaps:**
- No approval process — any user can create a site and grant access
- No access reviews on site membership
- No automatic lifecycle management — sites persist indefinitely
- Self-service sharing allows site owners to share with anyone, including external users

---

### Option 2: Controlled Self-Service with Governance Policies

**How it works:** Users can still create sites, but tenant-level policies enforce governance controls at the point of creation — naming conventions, classification, expiration and guest access controls.

| Attribute | Detail |
|-----------|--------|
| **Who provisions** | Licensed users (optionally restricted to specific Entra ID groups) |
| **Interface** | SharePoint start page; Teams; Outlook — with governance policies enforced |
| **Trigger** | User creates a site; policies applied automatically |
| **Approval workflow** | Optional — site creation approval via Power Automate or third-party tools |
| **Automation** | Policy enforcement is automatic; provisioning is user-initiated |
| **Licensing** | M365 licence + Entra ID P1 (for group creation restrictions, dynamic groups) |
| **Best suited for** | Medium organisations; organisations beginning their governance journey |

**Governance Policies Available:**

| Policy | Configuration Location | Effect |
|--------|----------------------|--------|
| **M365 Group creation restriction** | Entra ID → Group settings → restrict creation to security group | Only approved users can create group-connected sites and Teams |
| **Group naming policy** | Entra ID → Group settings → naming policy | Enforce prefix/suffix (e.g. `PRJ-`, `-Finance`, `{Department}-{GroupName}`) |
| **Group expiration policy** | Entra ID → Group settings → expiration | Sites expire after 90, 180 or 365 days unless owner renews |
| **Sensitivity labels** | Microsoft Purview → Sensitivity labels → group and site settings | Enforce privacy, guest access and sharing controls based on classification |
| **Default sharing links** | SharePoint Admin Centre → Sharing | Control default link type (specific people, organisation, anyone) |
| **External sharing restrictions** | SharePoint Admin Centre → Sharing; per-site override | Control whether sites allow external sharing and at what level |
| **Site creation settings** | SharePoint Admin Centre → Settings → Site creation | Enable/disable self-service; set default site storage limit; set default time zone |
| **Site storage quotas** | SharePoint Admin Centre → Active sites → per-site | Set individual storage limits per site |

**Advantages:**
- Users retain self-service capability but within defined guardrails
- Naming conventions and classification applied consistently at creation
- Expiration policies prevent indefinite site sprawl
- Sensitivity labels enforce data protection based on classification

**Challenges:**
- Group creation restriction prevents users from creating Teams and Outlook groups as well (not just SharePoint sites)
- Naming policies can be rigid and frustrate users with legitimate naming needs
- Expiration notifications go to owners — if ownership is unclear, sites expire and data is lost
- Policies apply at creation but do not retroactively fix existing ungoverned sites

---

### Option 3: IT/Helpdesk Provisioning via Service Request

**How it works:** Self-service site creation is disabled. Users raise a service request (ITSM ticket) and IT provisions the site manually via the SharePoint Admin Centre or PowerShell.

| Attribute | Detail |
|-----------|--------|
| **Who provisions** | IT administrators / helpdesk |
| **Interface** | SharePoint Admin Centre → Create site; PowerShell (`New-SPOSite`, `New-PnPSite`) |
| **Trigger** | ITSM ticket (ServiceNow, Jira, BMC, etc.) |
| **Approval workflow** | ITSM workflow — manager or data owner approval via ticket system |
| **Automation** | Manual fulfilment; ticket provides audit trail |
| **Licensing** | M365 licence; admin requires SharePoint Administrator role |
| **Best suited for** | Regulated environments; organisations requiring formal approval for all new sites |

**Advantages:**
- Full IT control over every site created
- Consistent configuration — IT applies naming, classification, storage limits and permissions at creation
- Audit trail via ITSM system (who requested, who approved, who provisioned)
- Can enforce data classification and sensitivity labelling before site is available

**Challenges:**
- Slow — users wait hours to days for site provisioning
- IT becomes a bottleneck; staff may resort to workarounds (personal OneDrive, email attachments, shadow IT)
- Does not scale for large organisations with frequent site creation needs
- Helpdesk may lack context to configure permissions and sharing appropriately
- Manual process is error-prone — inconsistent configurations across sites

---

### Option 4: Managed Self-Service with Approval Workflow

**How it works:** Users request sites through a governed process. Requests are reviewed and approved before the site is provisioned. Multiple implementation mechanisms are available.

#### Option 4A: Microsoft Forms + Power Automate

| Attribute | Detail |
|-----------|--------|
| **Request process** | User submits a Microsoft Form (site name, purpose, classification, owner, expected lifetime) |
| **Approval workflow** | Power Automate flow routes the request to an approver (IT, governance team or manager) |
| **Provisioning** | On approval, Power Automate calls the SharePoint REST API or Microsoft Graph API to create the site with standardised settings |
| **Naming** | Enforced by the flow — applies naming conventions automatically |
| **Classification** | Sensitivity label applied automatically based on the requester's selection |
| **Audit trail** | Form submissions and approval decisions logged in the flow run history |
| **Licensing** | Power Automate standard (included in most M365 plans) |

#### Option 4B: ServiceNow / ITSM Integration

| Attribute | Detail |
|-----------|--------|
| **Request process** | Users raise a service request in the ITSM platform |
| **Approval workflow** | ITSM approval workflow routes to IT, information governance or the manager |
| **Provisioning** | ITSM calls Microsoft Graph API (via integration connector or custom script) |
| **CMDB** | Site registered as a configuration item (CI) with owner, classification, review date and decommission date |
| **Audit trail** | Full ITSM ticket history for compliance evidence |
| **Licensing** | ITSM platform licence + M365 |

**Advantages (both):**
- Formal approval before site creation
- Consistent naming, classification and configuration
- Audit trail of every request, approval and provisioning action
- Can enforce data classification before the site exists

**Challenges (both):**
- Adds friction to site creation (users must wait for approval)
- Requires development and maintenance of flows/integrations
- Complex provisioning logic can become difficult to manage

---

### Option 5: Automated Provisioning via Site Templates and Site Designs

**How it works:** IT creates **site templates** (formerly site designs and site scripts) that pre-configure sites with defined structure, branding, permissions, navigation, content types and columns.

| Attribute | Detail |
|-----------|--------|
| **Who provisions** | Users (from template catalog), IT, or automated workflows |
| **Interface** | SharePoint start page → select template; SharePoint Admin Centre; PnP Provisioning; PowerShell |
| **Trigger** | User selects a template when creating a site; or automated workflow triggers template deployment |
| **Approval workflow** | Optional — can combine with Power Automate approval before template deployment |
| **Automation** | Template application is automatic; provisioning trigger varies |
| **Licensing** | M365 licence; PnP Provisioning is free (open source) |
| **Best suited for** | Organisations with repeatable site patterns (project sites, department sites, client sites) |

**Template Types:**

| Template Mechanism | Scope | Capabilities |
|-------------------|-------|-------------|
| **SharePoint look book templates** | Pre-built Microsoft templates | Standard layouts for communication and team sites |
| **Custom site templates (site designs)** | JSON-based site scripts | Set theme, create lists/libraries, apply content types, set navigation, configure regional settings, trigger Power Automate flows |
| **PnP Provisioning Templates** | XML/JSON-based (PnP Framework) | Full-fidelity template: pages, web parts, lists, libraries, content types, permissions, navigation, branding, term sets, search configuration |
| **Organisation site templates** | SharePoint Admin Centre → Site templates | Curated templates visible to users during site creation |

**Advantages:**
- Consistent site structure and configuration across all sites of the same type
- Pre-configured permissions, content types and metadata reduce setup time
- Users can self-serve while still getting a properly governed site
- PnP Provisioning enables full-fidelity site replication including complex configurations

**Challenges:**
- Template development requires technical skills (JSON, PowerShell, PnP Framework)
- Templates can become outdated if SharePoint features change
- Site scripts have limitations (cannot configure all site settings; max 300 actions)
- PnP Provisioning requires a provisioning engine (Azure Function, scheduled job)
- Templates apply at creation — changes to the template do not retroactively update existing sites

---

### Option 6: Event-Driven Automated Provisioning

**How it works:** Sites are provisioned automatically based on business events without any user request or IT intervention.

| Trigger | Mechanism | Example |
|---------|-----------|---------|
| **New project created** | Project management system triggers Graph API call | New project approved → team site created with project template |
| **New department or team** | HR system event triggers Entra ID dynamic group → M365 group → team site | New department in Workday → site provisioned automatically |
| **New hire** | Lifecycle Workflow joiner task adds user to relevant M365 groups | New hire starts → gains access to department team site |
| **Academic term start** | Student Information System triggers School Data Sync → class teams created | New semester → class teams and associated SharePoint sites created |
| **Client engagement** | CRM system triggers site creation for new client projects | New client deal closed → client collaboration site provisioned |

**Advantages:**
- Zero delay — sites available when needed
- Consistent — every site follows the template
- Scalable — handles hundreds or thousands of sites without IT bottleneck

**Challenges:**
- Requires integration development and maintenance
- Trigger data quality matters — incorrect HR data creates incorrect sites
- Over-provisioning risk — sites created for every event even when not needed

---

## 3. Site Access Management Options

### 3.1 Direct Permission Assignment

| Method | Description | Identity Object |
|--------|-------------|-----------------|
| **Add individual users** | Grant a specific user access at a defined permission level | Entra ID user added to SharePoint permission group |
| **Add Entra ID security group** | Grant all group members access at a defined permission level | Entra ID security group added to SharePoint permission group |
| **Add M365 Group** | Group-connected sites: M365 Group membership = site membership | M365 Group (owners → site owners; members → site members) |
| **Add Entra ID dynamic group** | Dynamic group based on user attributes added to SharePoint permission group | Entra ID dynamic group (P1 licence) |
| **Share via link** | Generate a sharing link (specific people, organisation, or anyone) | Link-based access; may not require explicit group membership |

### 3.2 Access Request and Approval

| Method | Description | Configuration |
|--------|-------------|---------------|
| **SharePoint access request (native)** | Users who lack access can request it; request goes to site owners for approval | Site settings → Access requests and invitations → Allow access requests: On |
| **Entitlement Management access package** | Users request a package containing site access via My Access portal; formal approval workflow | Entra ID Governance → Access Packages |
| **ITSM ticket** | Users raise a ticket requesting site access; IT or site owner fulfils after approval | ITSM platform (ServiceNow, Jira, etc.) |
| **Power Automate approval flow** | Custom flow triggered by a form or email; routes approval to site owner and adds user on approval | Power Automate + SharePoint connector |

### 3.3 Entra ID Entitlement Management (Access Packages) for SharePoint

| Scenario | Access Package Configuration |
|----------|------------------------------|
| **Project site access** | Package contains: M365 Group membership (grants site member access) + related Teams team + Planner board |
| **Read-only access to a communication site** | Package contains: Entra ID security group added as site visitor; no M365 Group involvement |
| **External partner access** | Connected organisation policy allows external users to request a package granting M365 Group guest membership |
| **Departmental site access** | Auto-assignment rule adds users to the package when `department` attribute matches |
| **Time-limited project access** | Package with 90-day expiry; user must request renewal with justification |

**Advantages:**
- Formal, auditable request and approval workflow for site access
- Time-limited access with automatic expiry
- Built-in access reviews — owners periodically recertify site membership
- Bundles SharePoint access with related Teams, applications and roles in a single request
- External user support via connected organisations
- Separation of duties — incompatible packages prevent conflicting access

**Licensing:** Requires **Entra ID Governance** licence.

### 3.4 HR-Driven Access via Lifecycle Workflows

| Workflow | Trigger | SharePoint Actions |
|----------|---------|-------------------|
| **Pre-hire** | X days before start date | Add to onboarding M365 Group (grants onboarding team site access) |
| **Joiner — day one** | Employee start date | Add to departmental M365 Group (department team site); add to all-company visitor group (communication site) |
| **Mover** | Department or job title change | Remove from old departmental group; add to new — site access adjusts automatically |
| **Leaver — last day** | Employment end date | Remove from all M365 Groups and security groups; disable account |
| **Post-leaver** | X days after departure | Delete account; content owned by leaver reassigned via retention policy |

**Licensing:** Requires **Entra ID Governance** licence.

### 3.5 External Sharing Controls

| Sharing Level | Description | Risk Level |
|---------------|-------------|-----------|
| **Anyone (anonymous)** | Anyone with the link can access — no sign-in required | Very High |
| **New and existing guests** | External users must sign in with a guest account or one-time passcode | Medium |
| **Existing guests only** | Only external users already in the directory can access | Low |
| **Only people in your organisation** | No external sharing permitted | None |

External sharing can be set at **tenant level** (global default) and **per-site** (site-level override, but cannot exceed tenant level). Sensitivity labels can also enforce sharing restrictions.

---

## 4. Site Lifecycle Management

### 4.1 Lifecycle Stages

```
┌──────────────┐     ┌──────────────┐     ┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│   Request /  │────▶│   Active     │────▶│   Inactive   │────▶│   Archived   │────▶│   Deleted    │
│   Provision  │     │              │     │   (Warning)  │     │  (Read-Only) │     │              │
└──────────────┘     └──────────────┘     └──────────────┘     └──────────────┘     └──────────────┘
                           │                                          │
                           │         ┌──────────────┐                 │
                           └────────▶│   Renewed    │◀────────────────┘
                                     │  (by owner)  │
                                     └──────────────┘
```

### 4.2 M365 Group Expiration (Group-Connected Sites)

The primary native lifecycle tool for group-connected team sites.

| Setting | Detail |
|---------|--------|
| **Policy scope** | All M365 groups, selected groups, or no groups |
| **Expiration period** | 90, 180 or 365 days (custom periods not supported) |
| **Renewal** | Owners receive email notifications at 30, 15 and 1 day before expiry |
| **Activity-based renewal** | If enabled, any user activity (Teams message, SharePoint file access, Outlook email, Yammer post) automatically renews the group |
| **Failure to renew** | Group soft-deleted → 30-day recovery period → permanent deletion (including SharePoint site, mailbox, Teams team, Planner and all content) |
| **Limitation** | Only applies to M365 group-connected sites. Communication sites, non-group team sites and hub sites have NO native expiration mechanism |

### 4.3 Non-Group Site Lifecycle

Communication sites and non-group team sites have no native lifecycle management. Options include:

| Approach | Detail |
|----------|--------|
| **Custom Power Automate flows** | Scheduled flow queries SharePoint API for inactive sites → notifies owner → archives or deletes if no response |
| **SharePoint Admin Centre inactive sites report** | Identifies sites with no activity — admin reviews and takes manual action |
| **Third-party governance tools** | AvePoint, ShareGate, Rencore and Orchestry provide lifecycle policies for all site types |
| **Custom Graph API automation** | Azure Function or Automation runbook applies policy logic and archives or deletes based on rules |
| **Manual register and review** | Maintain a site register with review dates; site owners certify continued need at each review |

### 4.4 Inactive Site Detection

| Method | Data Source | Threshold Examples |
|--------|-----------|-------------------|
| **M365 Group expiration** | Group activity (email, Teams, SharePoint, Yammer) | 90, 180 or 365 days with no group activity |
| **SharePoint Admin Centre** | Active sites report → filter by `Last activity` | Sites with no activity in 90+ days |
| **PowerShell** | `Get-SPOSite` → `LastContentModifiedDate` attribute | Content not modified in 180+ days |
| **Microsoft 365 usage reports** | M365 Admin Centre → Usage → SharePoint site usage | Sites with zero file views in 90+ days |
| **Third-party platform** | Aggregated activity data (views, edits, unique visitors, storage growth) | Configurable multi-signal inactivity scoring |

### 4.5 Lifecycle Management Comparison

| Lifecycle Stage | Native M365 (Group Expiration) | PowerShell / Graph API | Third-Party Platform | Entra ID Governance |
|----------------|-------------------------------|----------------------|---------------------|-------------------|
| **Provisioning** | User self-service or IT manual | Scripted provisioning from templates | Portal with approval workflow | Access package triggers group creation |
| **Active use** | No monitoring | Custom scripts check `LastContentModifiedDate` | Dashboard showing activity metrics | N/A |
| **Inactivity detection** | Group expiration notification (group-connected only) | `Get-SPOSite` filtered by last modified date | Inactive site detection with configurable thresholds | N/A |
| **Owner notification** | Expiration email 30, 15 and 1 day before expiry | Custom notification via Power Automate or email | Multi-stage notifications with escalation | Access review notifications |
| **Renewal** | Owner clicks renewal link in email | Custom renewal process | Renewal via governance portal with justification | Access package renewal request |
| **Archival** | Not native — requires custom process | `Set-SPOSite -LockState ReadOnly` | Automated archive with metadata retention | N/A |
| **Deletion** | Automatic on group expiry (soft-delete, 30-day recovery) | `Remove-SPOSite` (soft-delete, 93-day recovery) | Automated deletion with pre-deletion backup | Group deletion on package expiry |

---

## 5. Permission Management Challenges

### 5.1 Common Permission Issues

| Issue | Description | Impact | Resolution |
|-------|-------------|--------|------------|
| **Broken inheritance** | Site owners break permission inheritance on libraries, folders or items to grant unique permissions | Complex, opaque permission structure difficult to audit; users may access items they should not | Minimise inheritance breaks; use separate libraries or sites instead of folder-level unique permissions |
| **Direct user permissions** | Users granted access directly rather than via groups | Difficult to manage at scale; users accumulate direct permissions; offboarding misses direct grants | Enforce group-based access only; audit and migrate direct permissions to Entra ID security groups |
| **Sharing link sprawl** | Users create sharing links freely, including "anyone" links for sensitive documents | Data accessible to anyone with the link; no audit of who accessed; links persist indefinitely unless expiry is set | Restrict sharing link types via sensitivity labels and tenant/site-level policy; set link expiry |
| **External user accumulation** | External users granted access without lifecycle management | Former collaborators retain access indefinitely; no review or expiry | Enforce guest access expiration via Entra ID; run access reviews targeting guest users |
| **Site collection admin sprawl** | Multiple users granted site collection administrator rights over time | Over-privileged access to all site content and settings | Regular audit of site collection admins; minimise grants |
| **Permission level misuse** | Users granted "Full Control" when "Edit" or "Contribute" would suffice | Over-privileged access; users can modify site structure and permissions | Define permission level standards by role; audit and remediate |

### 5.2 SharePoint-Specific vs. Group-Based Permissions

| Scenario | Group-Connected Team Site | Non-Group Site |
|----------|--------------------------|----------------|
| **How members are added** | Add to M365 Group → automatically get site member access | Add directly to SharePoint Members group or via Entra ID security group |
| **How members are removed** | Remove from M365 Group → site access revoked | Remove from SharePoint Members group or Entra ID security group |
| **JML automation** | Lifecycle Workflows, dynamic groups and access packages manage M365 Group membership → site access follows | Must explicitly manage Entra ID security group membership |
| **Access review** | Entra ID access reviews target M365 Group membership | Entra ID access reviews target the Entra ID security group; SharePoint-only groups not reviewable via Entra ID |
| **Offboarding gap** | Removing from M365 Group revokes team site access but does NOT revoke direct permissions, sharing links or site collection admin grants | All access must be explicitly revoked; no single action removes all site access |

---

## 6. Storage and Quota Management

| Aspect | Detail |
|--------|--------|
| **Tenant storage pool** | 1 TB base + 10 GB per licensed user (pooled across all sites) |
| **Default site quota** | 25 TB per site collection (configurable) |
| **Monitoring** | SharePoint Admin Centre storage report; per-site storage consumption visible in admin centre |
| **Governance controls** | Set per-site storage limits; alert on high-consumption sites; implement retention policies to auto-delete aged content; use archive sites with lower quotas |

---

## 7. Retention and Compliance

| Requirement | Microsoft Tool | Detail |
|-------------|---------------|--------|
| **Retention policies** | Microsoft Purview retention policies | Apply to all SharePoint sites or specific sites; retain content for defined period then delete |
| **Retention labels** | Microsoft Purview retention labels | Apply to individual documents or libraries for content-specific retention (e.g. contracts retained 7 years) |
| **Legal hold** | Microsoft Purview eDiscovery hold | Preserve all content in a site regardless of retention policy for litigation or regulatory purposes |
| **DLP** | Microsoft Purview DLP | Detect and block sharing of sensitive content (PII, financial data, health records) |
| **Sensitivity labels** | Microsoft Purview sensitivity labels | Classify sites and documents; enforce encryption, access restrictions and sharing controls |
| **Audit** | M365 unified audit log | All SharePoint activities (file access, sharing, permission changes, site creation/deletion) logged |

---

## 8. Option Comparison Matrix

| Criteria | Self-Service (Uncontrolled) | Controlled Self-Service | IT/Helpdesk (Ticket) | Approval Workflow (Forms/ITSM) | Templates + Automation | Event-Driven |
|----------|---------------------------|------------------------|----------------------|-------------------------------|----------------------|-------------|
| **Provisioning speed** | Instant | Instant (with guardrails) | Hours–Days | Minutes–Hours (after approval) | Minutes (automated) | Automatic |
| **Governance at creation** | None | Naming, classification, expiration | Full (IT-controlled) | Full (approved, classified) | Template-enforced | Template-enforced |
| **Approval workflow** | None | Optional | ITSM ticket | Built-in (Power Automate or ITSM) | Optional | None (policy-based) |
| **Lifecycle management** | None | Group expiration only | Manual | Custom (flow-based) | Custom scripts | Tied to source event lifecycle |
| **External sharing control** | Tenant policy only | Sensitivity labels + sharing policy | IT-controlled per site | Enforced at provisioning | Template-enforced | Template-enforced |
| **Scales to 1,000+ sites** | Yes (uncontrolled) | Yes | No | Yes | Yes | Yes |
| **Licence requirement** | Base M365 | M365 + Entra ID P1 | Base M365 | Base M365 + Power Automate/ITSM | Base M365 (+ Azure for automation) | M365 + integration platform |
| **Implementation effort** | None | Low–Medium | Low | Medium | Medium–High | High |

---

## 9. Recommended Approach by Organisation Size

### Small Organisation (1–50 employees)

| Recommendation | Rationale |
|----------------|-----------|
| **Option 1 (Self-Service)** with tenant-level sharing restrictions | Keep it simple; restrict external sharing and "anyone" links at tenant level |
| Add **Option 2 (Controlled Self-Service)** when sites exceed 20–30 | Naming conventions and group expiration prevent early sprawl |

### Medium Organisation (50–500 employees)

| Recommendation | Rationale |
|----------------|-----------|
| **Option 2 (Controlled Self-Service)** as the baseline | Naming, classification and expiration policies |
| **Option 4 (Approval Workflow)** for sensitive or project sites | Forms + Power Automate provides lightweight approval without third-party cost |
| **Option 5 (Templates)** for repeatable site types | Project sites, client sites, department sites provisioned consistently |
| **Entitlement Management** for governed access to sensitive sites | Approval workflows and time-limited access for project and external collaboration |
| Regular **permission audits** via PnP PowerShell | Detect broken inheritance, direct user permissions and sharing link sprawl |

### Large Organisation (500+ employees)

| Recommendation | Rationale |
|----------------|-----------|
| **HR + Lifecycle Workflows** for department site access | Automate joiner/mover/leaver access to standing departmental sites |
| **Entitlement Management** for project and cross-functional sites | Governed request, approval, time limits, access reviews |
| **Option 5 (Templates)** + **PowerShell/Graph API** for provisioning | Standardised provisioning with custom automation for complex scenarios |
| **Option 6 (Event-Driven)** for project and client sites | Automatic provisioning from project management or CRM systems |
| **Third-party platform** if native tooling is insufficient | Full lifecycle management, inactive site detection, permission analytics |
| Combine with **Entra ID dynamic groups** for broad-access sites | All-company communication sites, regional intranets |

---

## 10. Third-Party Governance Platforms

| Platform | Key Capabilities |
|----------|-----------------|
| **AvePoint Cloud Governance** | Site provisioning with approval; lifecycle management; policy enforcement; recertification; inactive site detection |
| **ShareGate** | Migration, governance reporting, permission management, external sharing audit |
| **Orchestry** | Provisioning with templates and approval; lifecycle management; workspace directory; governance dashboard |
| **Rencore Governance** | Policy enforcement; customisation governance; tenant health; lifecycle automation |
| **CoreView** | M365 management and governance; delegation; reporting; automated remediation |
| **SysKit Point** | Permission management; access reviews; lifecycle management; compliance reporting |

---

## 11. Key Microsoft Technologies Reference

| Technology | Role in SharePoint Governance |
|------------|-------------------------------|
| **Microsoft Entra ID** | Identity store for all site users (internal and guest) |
| **Microsoft 365 Groups** | Backing identity object for group-connected team sites |
| **Entra ID Dynamic Groups (P1)** | Attribute-based automatic site membership |
| **Entra ID Entitlement Management (Governance)** | Access packages for site access with request, approval, expiry and review |
| **Entra ID Lifecycle Workflows (Governance)** | Automated JML workflows adjusting site access |
| **Entra ID Access Reviews (Governance/P2)** | Periodic recertification of site membership |
| **Entra External ID (B2B)** | Guest user identity for external collaboration |
| **Microsoft Purview Sensitivity Labels** | Site classification enforcing privacy, sharing and guest access controls |
| **Microsoft Purview DLP** | Data loss prevention for documents stored in SharePoint |
| **Microsoft Purview Retention Policies** | Retention and deletion of SharePoint content |
| **SharePoint Admin Centre** | Tenant-level site management, sharing controls, storage quotas |
| **SharePoint Access Requests** | Native self-service access request to site owners |
| **PnP PowerShell** | Community-supported PowerShell for SharePoint provisioning, permissions and governance |
| **SharePoint Online Management Shell** | Microsoft-supported PowerShell for tenant and site administration |
| **Microsoft Graph API** | Programmatic site and group management |
| **Power Automate** | Low-code workflows for provisioning approval and lifecycle automation |
| **Azure Automation** | Scheduled PowerShell runbooks for bulk and recurring operations |
| **Site Templates (Site Designs)** | Pre-configured site structure and settings applied at creation |

---

## 12. Key Governance Recommendations

| Recommendation | Detail |
|----------------|--------|
| **Prefer group-connected team sites** | M365 group backing provides the richest governance: Entra ID access reviews, group expiration, dynamic membership, sensitivity labels and Conditional Access |
| **Restrict creation, not collaboration** | Limit who can create M365 groups (and therefore team sites) but make the request process fast and frictionless |
| **Enforce sensitivity labels at creation** | Mandatory labelling ensures every site has a classification from day one, driving downstream protection policies |
| **Set group expiration** | Enable activity-based renewal with 365-day expiration as a safety net — actively used sites renew automatically; abandoned sites expire |
| **Govern communication sites separately** | Communication sites have no group backing and no native lifecycle — implement custom automation or third-party tooling |
| **Assign Entra ID groups to SharePoint permissions** | Even for non-group sites, use Entra ID security groups (not individual users) in SharePoint permission groups to bring membership under Entra ID governance |
| **Review guest access regularly** | Use Entra ID access reviews targeting guest users; set guest expiration policies; monitor external sharing reports |
| **Plan for archival** | Define an archival process (read-only, reduced permissions, long-term retention label) as an alternative to deletion |
| **Monitor storage proactively** | Set per-site quotas; alert on high-growth sites; use retention policies to control content volume |
| **Document site ownership** | Maintain a site register with owner, purpose, classification, creation date, review date and expected decommission date |
