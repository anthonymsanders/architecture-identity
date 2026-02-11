# Microsoft Teams — Membership Management and Lifecycle Options

**Date:** 11 February 2026

---

## Overview

This document examines the available options for managing Microsoft Teams membership, from fully manual administration through to automated, policy-driven assignment. The right approach depends on organisation size, licensing, governance maturity and the type of team (project, departmental, company-wide, classroom or external collaboration).

Every Microsoft Teams team is backed by a **Microsoft 365 Group** in **Entra ID**. Managing Teams membership is therefore fundamentally an identity and group management activity.

---

## 1. Teams Membership Fundamentals

### 1.1 Membership Roles

| Role | Permissions | Identity Requirement |
|------|-------------|---------------------|
| **Owner** | Full control — add/remove members, manage settings, delete channels, manage apps, delete the team | Entra ID user (licensed) — minimum two owners recommended |
| **Member** | Participate in conversations, share files, create channels (if permitted), add tabs and connectors | Entra ID user (licensed) |
| **Guest** | Participate in conversations and collaborate on files within teams they are invited to; restricted from certain actions (creating teams, discovering other teams, accessing other tenant resources) | Entra External ID (B2B guest) — authenticates via home identity provider |

### 1.2 Underlying Identity Object

| Teams Concept | Entra ID Object | Exchange Online Object |
|---------------|-----------------|----------------------|
| Team | Microsoft 365 Group (Unified Group) | Group mailbox |
| Team owner | M365 Group owner | N/A (mailbox access inherited) |
| Team member | M365 Group member | N/A (mailbox access inherited) |
| Team guest | Entra ID guest user added as M365 Group member | N/A |
| Private channel | Separate M365 Group (shared channel) or team-scoped ACL (private channel) | Separate SharePoint site (private/shared channels) |
| Shared channel | Entra ID B2B direct connect (cross-tenant) | Separate SharePoint site |

---

## 2. Membership Management Options

### Option 1: Manual Assignment by Team Owner

**How it works:** Team owners add and remove members directly within the Teams client, Teams Admin Centre or Entra Admin Centre.

| Attribute | Detail |
|-----------|--------|
| **Who manages** | Team owner(s) |
| **Interface** | Teams desktop/web client → Team settings → Members; Entra Admin Centre → Group → Members |
| **Trigger** | Owner decides to add/remove a user |
| **Approval workflow** | None — owner has full discretion |
| **Automation** | None |
| **Licensing** | Any M365 licence that includes Teams |
| **Best suited for** | Small teams, project teams, ad-hoc collaboration groups |

**Advantages:**
- Simple and immediate — no configuration required
- Owner retains full control over who is in the team
- No additional licensing or tooling needed

**Challenges:**
- Does not scale beyond small teams
- No audit trail of why a member was added (only that they were)
- Owner must remember to remove members when they leave the project or organisation
- If the owner leaves and no second owner exists, no one can manage membership
- No standard process — different owners manage membership differently

**Identity Governance Gaps:**
- No access reviews unless separately configured
- No automatic removal on role change or departure
- No approval process — any owner can add anyone

---

### Option 2: User Self-Service Request (Join / Leave)

**How it works:** Teams can be configured to allow users to discover and join public teams without owner approval, or to request access to private teams.

| Attribute | Detail |
|-----------|--------|
| **Who manages** | End user (join); Team owner (approve for private teams) |
| **Interface** | Teams client → Join or create a team → Browse teams |
| **Trigger** | User discovers a team and requests to join |
| **Approval workflow** | Public teams: no approval — immediate join. Private teams: owner receives a request and approves/denies |
| **Automation** | None |
| **Licensing** | Any M365 licence that includes Teams |
| **Best suited for** | Community teams, interest groups, company-wide information channels |

**Advantages:**
- Empowers users to find and access the teams they need
- Reduces administrative burden on team owners and IT
- Private team approval retains owner control

**Challenges:**
- Users may join teams containing data beyond their need-to-know
- No mechanism to ensure users leave teams when they no longer need access
- Public teams are discoverable by all users in the tenant — sensitive team names may reveal confidential initiatives
- No manager approval or secondary review for private team join requests
- No time-limited access — once joined, the user remains indefinitely

**Identity Governance Gaps:**
- No expiry on self-service joins
- No integration with HR data (role, department) to validate appropriateness
- No access recertification triggered by role change

---

### Option 3: IT/Helpdesk Manual Assignment via Admin Centre

**How it works:** IT administrators or helpdesk staff add and remove members via the Teams Admin Centre, Microsoft 365 Admin Centre or Entra Admin Centre on behalf of users, typically triggered by a service request (ticket).

| Attribute | Detail |
|-----------|--------|
| **Who manages** | IT administrators / helpdesk |
| **Interface** | Teams Admin Centre → Manage teams → Members; M365 Admin Centre → Groups; Entra Admin Centre → Groups |
| **Trigger** | Service request (ITSM ticket from ServiceNow, Jira, BMC, etc.) |
| **Approval workflow** | ITSM workflow — manager or team owner approval via ticket system |
| **Automation** | Manual fulfilment; ticket provides audit trail |
| **Licensing** | Any M365 licence that includes Teams; admin requires Teams Administrator or Groups Administrator role |
| **Best suited for** | Regulated environments where all access changes must be ticket-tracked; organisations without self-service governance tooling |

**Advantages:**
- Formal approval workflow via ITSM system
- Audit trail of who requested, who approved and who fulfilled the change
- Central IT maintains oversight of membership changes
- Can enforce segregation of duties (requester ≠ approver ≠ fulfiller)

**Challenges:**
- Slow — users wait for ticket resolution (hours to days)
- IT becomes a bottleneck for routine membership changes
- Does not scale for large organisations with thousands of teams
- Helpdesk staff may lack context to validate whether access is appropriate
- No automatic removal — relies on a separate "leaver" or "mover" ticket

**Identity Governance Gaps:**
- Governance depends on ITSM process maturity, not identity platform controls
- No automatic expiry or recertification unless separately managed
- Off-ticket changes (owner adds directly) bypass the process entirely

---

### Option 4: Entra ID Dynamic Membership Rules

**How it works:** The M365 Group backing the team is configured with a **dynamic membership rule** that automatically adds and removes members based on Entra ID user attributes (department, job title, location, company, etc.).

| Attribute | Detail |
|-----------|--------|
| **Who manages** | IT / Identity team (rule creation); HR system (attribute data) |
| **Interface** | Entra Admin Centre → Groups → Dynamic membership rules |
| **Trigger** | User attribute change in Entra ID (synced from AD DS or HR-driven provisioning) |
| **Approval workflow** | None — membership is automatic based on attribute match |
| **Automation** | Fully automated — Entra ID evaluates rules and adjusts membership |
| **Licensing** | Requires **Entra ID P1** (or P2) for dynamic group membership |
| **Best suited for** | Departmental teams, location-based teams, role-based teams, all-company teams |

**Rule Examples:**

```
# All employees in the Finance department
(user.department -eq "Finance") and (user.accountEnabled -eq true)

# All full-time staff in the London office
(user.city -eq "London") and (user.employeeType -eq "FullTime")

# All managers (users with direct reports)
(user.directReports -ne null)

# All employees excluding contractors
(user.userType -eq "Member") and (user.employeeType -ne "Contractor")
```

**Advantages:**
- Fully automated — zero manual intervention for routine membership changes
- Membership always reflects current organisational reality (department, location, role)
- Employees who move roles are automatically removed from old teams and added to new ones
- Leavers are automatically removed when their account is disabled or deleted
- Scales to any organisation size

**Challenges:**
- Requires high-quality, consistent attribute data in Entra ID (typically HR-driven)
- Rule syntax can be complex and is error-prone for sophisticated conditions
- Dynamic groups cannot have manually added members — membership is entirely rule-driven (no exceptions without a workaround)
- Rule evaluation can take minutes to hours depending on tenant size and change volume
- Guest users are excluded from dynamic membership rules by default
- Cannot combine dynamic and assigned (static) membership in the same group

**Identity Governance Gaps:**
- No approval workflow — if attributes match, the user is added automatically
- Data quality issues in HR or AD attributes directly corrupt team membership
- No mechanism to override the rule for individual exceptions (e.g. "everyone in Finance except this person")
- Rule changes affect all members immediately — a typo can add or remove hundreds of users

---

### Option 5: Entra ID Entitlement Management (Access Packages)

**How it works:** Teams membership is bundled into an **Access Package** in Entra ID Entitlement Management. Users request the package through a self-service portal, and access is granted after defined approval workflows, with optional time limits and periodic reviews.

| Attribute | Detail |
|-----------|--------|
| **Who manages** | Identity / governance team (package creation); business owners (approval); users (self-service request) |
| **Interface** | Entra ID Governance → Entitlement Management → Access Packages; My Access portal (myaccess.microsoft.com) for end users |
| **Trigger** | User requests access via My Access portal; auto-assignment based on rules; or direct assignment by admin |
| **Approval workflow** | Configurable: single-stage (manager), multi-stage (manager then resource owner), or auto-approved |
| **Automation** | Request-based with optional auto-assignment; automatic expiry and renewal |
| **Licensing** | Requires **Entra ID Governance** licence (or Entra ID P2 for basic functionality) |
| **Best suited for** | Governed team access in regulated environments; cross-organisational collaboration; project-based time-limited access |

**Access Package Components:**

| Component | Description |
|-----------|-------------|
| **Resources** | One or more M365 Groups (Teams), Entra ID roles, SharePoint sites or applications bundled together |
| **Policies** | Who can request, approval requirements, access duration, renewal rules, access review schedule |
| **Catalogs** | Groupings of access packages delegated to business units |
| **Connected organisations** | Allow external users from specific partner organisations to request access |
| **Auto-assignment** | Rules that automatically assign the package based on user attributes (similar to dynamic groups but with governance controls) |

**Advantages:**
- Formal, auditable request and approval workflow
- Time-limited access with automatic expiry (e.g. 90 days, 6 months, 1 year)
- Built-in access reviews — reviewers periodically recertify that users still need access
- Bundles Teams membership with related resources (SharePoint, applications) in a single request
- Supports external users via connected organisations
- Auto-assignment combines the automation of dynamic groups with governance controls (approval, expiry, review)
- Separation of duties — can mark access packages as incompatible (a user cannot hold both)
- Delegation — business units manage their own catalogs and packages without IT involvement

**Challenges:**
- Requires Entra ID Governance licensing (additional cost)
- More complex to configure than manual or dynamic group approaches
- Users must learn to use the My Access portal
- Auto-assignment rules have the same attribute data quality dependency as dynamic groups
- Access packages add a layer of abstraction that can confuse administrators accustomed to direct group management

**Identity Governance Gaps:**
- Minimal — this is the most governance-rich option. Gaps are typically in adoption and configuration maturity rather than platform capability

---

### Option 6: HR-Driven Provisioning with Lifecycle Workflows

**How it works:** The **HR system** (Workday, SAP SuccessFactors) drives identity creation and attribute updates via Entra ID HR-driven provisioning. **Lifecycle Workflows** then trigger automated actions — including Teams membership assignment — based on lifecycle events (joiner, mover, leaver).

| Attribute | Detail |
|-----------|--------|
| **Who manages** | HR (data source); Identity team (workflow configuration); Entra ID (automation engine) |
| **Interface** | Entra Admin Centre → Lifecycle Workflows; HR system (Workday/SuccessFactors) for data |
| **Trigger** | HR event: new hire start date, department change, termination date |
| **Approval workflow** | Pre-configured in the workflow — can include manager notification or approval tasks |
| **Automation** | Fully automated end-to-end from HR event to Teams membership change |
| **Licensing** | Requires **Entra ID Governance** licence |
| **Best suited for** | Enterprise organisations with mature HR systems; automating day-one access for joiners; automated offboarding |

**Lifecycle Workflow Examples:**

| Workflow | Trigger | Teams Actions |
|----------|---------|--------------|
| **Pre-hire** | X days before employee start date | Create Entra ID account; add to onboarding team |
| **Joiner — day one** | Employee start date | Add to departmental team; add to all-company team; send welcome message in Teams |
| **Mover** | Department or job title change in HR | Remove from old departmental team; add to new departmental team |
| **Leaver — last day** | Employment end date | Remove from all teams; revoke Teams sessions; disable account |
| **Post-leaver** | X days after departure | Delete account; purge from group membership cache |

**Advantages:**
- True end-to-end automation from authoritative HR data
- Day-one access — new hires have Teams access from their first day
- Movers automatically transition between teams when HR data changes
- Leavers are removed from all teams automatically on their last day
- Combines with dynamic groups and access packages for a layered approach

**Challenges:**
- Requires mature HR system integration (Workday or SAP SuccessFactors connector)
- HR data must be accurate and timely — delays in HR updates delay identity actions
- Custom HR systems require Graph API integration (no built-in connector)
- Lifecycle Workflows support a defined set of actions — complex scenarios may require custom extensions via Logic Apps or Azure Functions
- Not suitable for non-employee identities (guests, partners) unless managed in the HR system

---

### Option 7: PowerShell and Graph API Automation

**How it works:** Custom scripts using **Microsoft Graph API** or **PowerShell (Microsoft.Graph module)** programmatically manage Teams membership based on data from any source (HR systems, ITSM platforms, databases, CSV files).

| Attribute | Detail |
|-----------|--------|
| **Who manages** | IT / Identity team (script development and scheduling) |
| **Interface** | PowerShell scripts, Azure Automation runbooks, Azure Functions, Logic Apps |
| **Trigger** | Scheduled (daily/hourly), event-driven (webhook from HR/ITSM), or on-demand |
| **Approval workflow** | Custom — built into the ITSM or orchestration platform triggering the script |
| **Automation** | Fully automated but requires custom development and maintenance |
| **Licensing** | M365 licence for Teams; Azure subscription for Azure Automation/Functions; Entra ID app registration for Graph API permissions |
| **Best suited for** | Organisations with unique requirements not met by native tooling; bulk operations; integration with non-Microsoft systems |

**Common Graph API Operations:**

| Operation | Graph API Endpoint | PowerShell Cmdlet |
|-----------|-------------------|-------------------|
| List team members | `GET /teams/{id}/members` | `Get-MgTeamMember` |
| Add member | `POST /teams/{id}/members` | `New-MgTeamMember` |
| Remove member | `DELETE /teams/{id}/members/{id}` | `Remove-MgTeamMember` |
| Add owner | `POST /teams/{id}/members` (with owner role) | `New-MgTeamMember -Roles "owner"` |
| List all teams | `GET /groups?$filter=resourceProvisioningOptions/Any(x:x eq 'Team')` | `Get-MgGroup -Filter "resourceProvisioningOptions/Any(x:x eq 'Team')"` |
| Create team from group | `PUT /groups/{id}/team` | `New-MgTeam` |

**Advantages:**
- Maximum flexibility — any logic, any data source, any integration
- Can handle complex scenarios not supported by native Entra ID tooling
- Bulk operations (add 1,000 members from CSV) are straightforward
- Can integrate with non-Microsoft identity sources (LDAP directories, custom databases, third-party HR systems)
- Scheduled Azure Automation runbooks provide reliable, monitored execution

**Challenges:**
- Requires development and ongoing maintenance of custom code
- Scripts must handle error conditions, throttling (Graph API rate limits), pagination and retry logic
- No built-in approval workflow — must be integrated with external systems
- Security risk if scripts use high-privilege application permissions without proper secret management
- No native audit trail beyond what is logged in Entra ID audit logs and custom script logging
- Knowledge dependency — if the script author leaves, maintenance may stall

---

### Option 8: Third-Party Identity Governance Platforms (IGA)

**How it works:** An enterprise Identity Governance and Administration (IGA) platform manages Teams membership as part of a broader identity lifecycle, integrating with Entra ID via Graph API or SCIM.

| Attribute | Detail |
|-----------|--------|
| **Who manages** | Identity governance team (IGA platform); business owners (approval in IGA portal) |
| **Interface** | IGA platform portal (SailPoint, Saviynt, Omada, One Identity, etc.) |
| **Trigger** | HR event, access request, role change, periodic certification, policy violation detection |
| **Approval workflow** | Full multi-stage approval, SoD checks, risk scoring and policy evaluation within the IGA platform |
| **Automation** | Fully automated provisioning via Graph API connector |
| **Licensing** | IGA platform licence + M365 licence; Entra ID app registration for Graph API |
| **Best suited for** | Large enterprises with complex governance requirements; multi-platform environments; regulated industries |

**Common IGA Platforms with Teams Integration:**

| Platform | Integration Method | Key Capabilities |
|----------|--------------------|-----------------|
| **SailPoint IdentityNow / ISC** | Graph API connector | Access requests, certifications, SoD, role-based provisioning |
| **Saviynt** | Graph API connector | Application-level entitlement governance, risk-based access |
| **Omada Identity** | Graph API connector | Business role management, policy-driven access |
| **One Identity Manager** | Graph API connector | Full lifecycle management, attestation campaigns |
| **Microsoft Entra ID Governance** | Native | Access packages, lifecycle workflows, access reviews (covered in Options 5 and 6) |

**Advantages:**
- Comprehensive governance across Teams AND all other applications in a single platform
- Advanced features: role mining, SoD conflict detection, risk-based access decisions, orphan account detection
- Unified audit trail across all systems (not just Microsoft)
- Handles complex provisioning logic (role-based access models, attribute-based policies)
- Mature reporting and compliance dashboards

**Challenges:**
- Significant cost (platform licensing, implementation, ongoing operations)
- Complex implementation — typically 6–18 months for full deployment
- Requires dedicated IGA operations team
- Graph API connector must be maintained as Microsoft updates APIs
- Adds a layer of abstraction and a dependency on a third-party vendor

---

## 3. Option Comparison Matrix

| Criteria | Manual (Owner) | Self-Service (Join) | IT/Helpdesk (Ticket) | Dynamic Groups | Access Packages | HR + Lifecycle Workflows | Graph API / PowerShell | Third-Party IGA |
|----------|---------------|--------------------|-----------------------|---------------|----------------|------------------------|-----------------------|----------------|
| **Automation level** | None | Partial | None | Full | Partial (request + auto) | Full | Full (custom) | Full |
| **Approval workflow** | None | Owner (private teams) | ITSM ticket | None | Configurable multi-stage | Configurable | Custom | Full multi-stage |
| **Time-limited access** | No | No | No | No (attribute-driven) | Yes | Yes (workflow-driven) | Custom | Yes |
| **Access reviews** | No | No | No | No (must add separately) | Built-in | Must add separately | Custom | Built-in |
| **SoD enforcement** | No | No | No | No | Yes (incompatible packages) | No | Custom | Yes |
| **External / guest support** | Owner invites | No (internal only) | Ticket-based | No (guests excluded) | Yes (connected orgs) | Limited | Yes | Yes |
| **Scales to 1,000+ teams** | No | Partial | No | Yes | Yes | Yes | Yes | Yes |
| **HR data dependency** | None | None | None | High | Optional | High | Optional | Optional |
| **Licence requirement** | Base M365 | Base M365 | Base M365 | Entra ID P1 | Entra ID Governance | Entra ID Governance | Base M365 + Azure | IGA platform + M365 |
| **Implementation complexity** | None | Low | Low | Medium | Medium–High | High | High | Very High |
| **Audit trail** | Entra ID logs only | Entra ID logs only | ITSM + Entra ID logs | Entra ID logs | Full governance logs | Full governance logs | Custom + Entra ID logs | Comprehensive |
| **Best for** | Small teams, ad-hoc | Communities, info teams | Regulated, ticket-driven | Departmental, role-based | Governed, time-limited | Enterprise JML automation | Custom / bulk / integration | Enterprise multi-platform |

---

## 4. Recommended Approach by Organisation Size

### Small Organisation (1–50 employees)

| Recommended Options | Rationale |
|--------------------|-----------|
| **Option 1 (Manual)** + **Option 2 (Self-Service)** | Low overhead; team owners manage membership directly. Self-service join for company-wide teams. No additional licensing needed |
| Add **Option 4 (Dynamic Groups)** if Entra ID P1 is available | Automate all-company and departmental teams. Requires consistent user attributes |

### Medium Organisation (50–500 employees)

| Recommended Options | Rationale |
|--------------------|-----------|
| **Option 4 (Dynamic Groups)** for departmental and role-based teams | Automation reduces manual effort and ensures leavers are removed |
| **Option 3 (IT/Helpdesk)** for sensitive or project teams | Ticket-based process provides audit trail for regulated access |
| Add **Option 5 (Access Packages)** if Entra ID Governance is licensed | Provides approval workflows, time limits and access reviews for project and cross-functional teams |

### Large Organisation (500+ employees)

| Recommended Options | Rationale |
|--------------------|-----------|
| **Option 6 (HR + Lifecycle Workflows)** as the foundation | Automates joiner/mover/leaver processes end-to-end from authoritative HR data |
| **Option 4 (Dynamic Groups)** for standing departmental teams | Attribute-based membership that self-maintains |
| **Option 5 (Access Packages)** for project, cross-functional and external teams | Governance-rich request and approval with time limits, reviews and SoD |
| **Option 7 (Graph API)** for custom integrations and bulk operations | Handles edge cases and integrations with non-Microsoft systems |
| **Option 8 (IGA)** if multi-platform governance is required | Unified governance across Teams, SAP, ServiceNow, custom applications and other platforms |

---

## 5. Lifecycle Management Across Options

### 5.1 Joiner (New Team Member)

| Option | How a New Member is Added |
|--------|--------------------------|
| Manual | Owner adds in Teams client |
| Self-Service | User browses and joins (public) or requests (private) |
| IT/Helpdesk | Ticket raised → approved → helpdesk adds via Admin Centre |
| Dynamic Groups | User attribute matches rule → automatically added |
| Access Packages | User requests via My Access → approved → automatically added |
| HR + Lifecycle Workflows | HR new-hire event → workflow adds to designated teams |
| Graph API | Script/runbook evaluates source data → adds via API |
| IGA | Role/policy match → IGA provisions via Graph API connector |

### 5.2 Mover (Role or Department Change)

| Option | How Membership Changes on Role Move |
|--------|--------------------------------------|
| Manual | Old owner removes; new owner adds (if they remember) |
| Self-Service | User must leave old team and join new team manually |
| IT/Helpdesk | Mover ticket raised → old access removed, new access granted |
| Dynamic Groups | Attribute change triggers automatic removal from old group and addition to new group |
| Access Packages | Old package expires or is revoked; user requests new package |
| HR + Lifecycle Workflows | HR role change → mover workflow removes old teams, adds new teams |
| Graph API | Script detects attribute change → adjusts membership |
| IGA | Role recalculation → IGA adjusts entitlements across all systems |

### 5.3 Leaver (Departure)

| Option | How a Departing User is Removed |
|--------|----------------------------------|
| Manual | Owner must remember to remove; often forgotten |
| Self-Service | User cannot self-remove after account is disabled |
| IT/Helpdesk | Leaver ticket raised → helpdesk removes from all teams (if all teams are known) |
| Dynamic Groups | Account disabled → no longer matches rule → automatically removed |
| Access Packages | Account disabled → all package assignments revoked → automatically removed |
| HR + Lifecycle Workflows | HR termination event → leaver workflow removes from all teams and disables account |
| Graph API | Offboarding script removes from all teams programmatically |
| IGA | Account deprovisioning removes all entitlements including Teams membership |

---

## 6. Guest and External Member Management

Guest membership requires specific consideration as guests follow a different identity lifecycle.

| Option | Guest Support | Detail |
|--------|--------------|--------|
| **Manual** | Yes | Owner invites guests directly in Teams; guest receives email invitation and redeems via Entra External ID |
| **Self-Service** | No | Guests cannot browse or request to join teams |
| **IT/Helpdesk** | Yes | Guest invitation raised via ticket; helpdesk creates guest account and adds to team |
| **Dynamic Groups** | No | Dynamic membership rules do not evaluate guest users by default |
| **Access Packages** | Yes | Connected organisations allow external users to request access packages; sponsor approval; time-limited; access reviews |
| **HR + Lifecycle Workflows** | Limited | Lifecycle Workflows can trigger guest invitation but guests are not typically managed in HR systems |
| **Graph API** | Yes | Full guest invitation and membership management via `POST /invitations` and team member APIs |
| **IGA** | Yes | IGA platforms manage external identities as part of the broader identity lifecycle |

**Guest-Specific Governance Controls:**
- Set **guest access expiration** (Entra ID external collaboration settings)
- Require **MFA and Terms of Use** for all guests via Conditional Access
- Run **Entra ID access reviews** targeting guest members across all teams
- Use **sensitivity labels** to block guest access on classified teams
- Monitor guest activity via **Entra ID sign-in logs** and **Defender for Cloud Apps**

---

## 7. Key Microsoft Technologies Reference

| Technology | Role in Teams Membership Management |
|------------|-------------------------------------|
| **Microsoft Entra ID** | Identity store for all team members (internal and guest) |
| **Microsoft 365 Groups** | Underlying group object for every team — membership = team membership |
| **Entra ID Dynamic Groups (P1)** | Attribute-based automatic membership |
| **Entra ID Entitlement Management (Governance)** | Access packages with request, approval, expiry and review |
| **Entra ID Lifecycle Workflows (Governance)** | Automated joiner/mover/leaver workflows |
| **Entra ID Access Reviews (Governance/P2)** | Periodic recertification of team membership |
| **Entra ID HR-driven Provisioning** | Attribute sync from Workday/SuccessFactors driving dynamic membership |
| **Entra External ID (B2B)** | Guest user invitation and lifecycle |
| **Entra ID Cross-tenant Access Settings** | Trust and policy for shared channels and B2B collaboration |
| **Conditional Access** | Per-persona access policies for Teams |
| **Microsoft Purview Sensitivity Labels** | Team classification enforcing privacy, guest and sharing controls |
| **Teams Admin Centre** | Team and membership administration |
| **Microsoft Graph API** | Programmatic team and membership management |
| **Azure Automation** | Scheduled PowerShell runbooks for bulk and recurring operations |
| **Power Automate** | Low-code workflows for team provisioning and membership management |
