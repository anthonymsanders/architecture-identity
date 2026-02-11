# Microsoft Entra Applications — Management and Lifecycle Options

**Date:** 11 February 2026

---

## Overview

This document examines the available options for managing **Microsoft Entra ID application registrations, enterprise applications and their user assignments** — from fully manual administration through to automated, policy-driven governance. Entra ID applications are the gateway to organisational resources; every SaaS integration, custom application and API relies on an Entra ID application object for authentication and authorisation.

Effective application lifecycle management controls who can access which applications, how they authenticate, what permissions they hold and when access should be reviewed or revoked.

---

## 1. Entra ID Application Object Model

### 1.1 Application Objects

| Object Type | Description | Where It Lives | Example |
|-------------|-------------|----------------|---------|
| **App Registration** | The application's identity definition — client ID, redirect URIs, certificates/secrets, API permissions, token configuration | Home tenant (developer's tenant) | A custom-built HR portal registered in your tenant |
| **Enterprise Application (Service Principal)** | An instance of an application in your tenant — represents the app's presence for user assignment, SSO configuration, Conditional Access and provisioning | Each tenant that uses the app | Salesforce added from the gallery; your custom HR portal's local instance |
| **Managed Identity** | A special type of service principal automatically managed by Azure — no credentials to manage | Azure tenant (tied to an Azure resource) | A Virtual Machine or Azure Function authenticating to Key Vault |
| **Workload Identity** | Umbrella term for non-human identities: app registrations, service principals, managed identities and federated credentials | Entra ID tenant | A GitHub Actions workflow authenticating to Azure via federated credential |

### 1.2 Key Relationships

```
┌─────────────────────┐         ┌──────────────────────────┐
│   App Registration  │────────▶│  Enterprise Application  │
│   (Application      │  creates│  (Service Principal)     │
│    Object)          │         │                          │
│                     │         │  - User assignments       │
│  - Client ID        │         │  - SSO configuration      │
│  - Secrets/Certs    │         │  - Conditional Access      │
│  - API permissions  │         │  - Provisioning (SCIM)     │
│  - Redirect URIs    │         │  - App roles               │
└─────────────────────┘         └──────────────────────────┘
                                          │
                                          ▼
                                ┌──────────────────────┐
                                │   Users & Groups     │
                                │   assigned to app    │
                                │                      │
                                │  - Direct assignment  │
                                │  - Group membership   │
                                │  - Dynamic groups     │
                                │  - Access packages    │
                                └──────────────────────┘
```

### 1.3 Application Assignment Modes

| Mode | Description | Configuration |
|------|-------------|---------------|
| **User assignment required = Yes** | Only users and groups explicitly assigned to the application can access it. All others are blocked | Enterprise Application → Properties → User assignment required: Yes |
| **User assignment required = No** | All users in the tenant can access the application. No explicit assignment needed | Enterprise Application → Properties → User assignment required: No |

**Recommendation:** Set **User assignment required = Yes** for all applications to ensure that access is explicit, auditable and governable. This is the foundation of all application governance.

---

## 2. Application User Assignment Options

### Option 1: Manual Assignment by IT Administrator

**How it works:** IT administrators assign individual users or groups to enterprise applications via the Entra Admin Centre.

| Attribute | Detail |
|-----------|--------|
| **Who manages** | IT administrator (Application Administrator, Cloud Application Administrator or Global Administrator role) |
| **Interface** | Entra Admin Centre → Enterprise Applications → [Application] → Users and groups → Add user/group |
| **Trigger** | Admin decides to grant access based on request, ticket or policy |
| **Approval workflow** | None — admin has full discretion |
| **Automation** | None |
| **Licensing** | Entra ID Free (basic assignment); P1 for group-based assignment |
| **Best suited for** | Small organisations; low application count; initial setup |

**Advantages:**
- Simple and direct — admin selects users and assigns them
- Full control — admin can assign specific app roles per user
- No additional tooling or licensing required

**Challenges:**
- Does not scale beyond a few dozen applications and a few hundred users
- No audit trail of why access was granted (only that it was)
- No automatic removal on role change or departure
- No approval workflow — any admin can assign anyone
- Admin must know which app roles to assign and to whom

**Identity Governance Gaps:**
- No access reviews unless separately configured
- No time-limited access
- No self-service — users depend entirely on IT

---

### Option 2: Group-Based Assignment

**How it works:** Entra ID security groups or M365 groups are assigned to enterprise applications. All members of the group receive application access.

| Attribute | Detail |
|-----------|--------|
| **Who manages** | Group owner (membership); IT (application-to-group mapping) |
| **Interface** | Entra Admin Centre → Enterprise Applications → Users and groups → Add group |
| **Trigger** | User added to group → gains application access; removed from group → loses access |
| **Approval workflow** | Depends on how group membership is managed (see below) |
| **Automation** | Membership changes automatically propagate to application access |
| **Licensing** | Requires **Entra ID P1** for group-based application assignment |
| **Best suited for** | Role-based access; departmental applications; standardised access profiles |

**Group Types for Application Assignment:**

| Group Type | Membership Management | Best For |
|------------|----------------------|----------|
| **Assigned (static) security group** | Manual add/remove by group owner or admin | Applications with a stable, known user base |
| **Dynamic security group** | Automatic based on Entra ID user attributes (department, job title, location, etc.) | Departmental applications where access aligns to HR attributes |
| **M365 Group** | Manual, dynamic or via access packages | Applications tied to a collaboration group (SharePoint, Teams) |
| **Role-assignable group** | Restricted membership management; can be assigned to Entra ID directory roles | Privileged applications requiring strict membership control |

**Advantages:**
- Scales well — manage hundreds of users via a single group
- Dynamic groups automate access based on HR-driven attributes
- Single change (group membership) grants or revokes access to multiple applications
- Group ownership can be delegated to business owners

**Challenges:**
- Requires disciplined group management — stale groups create stale access
- Dynamic group rules depend on attribute data quality
- Group-to-application mapping must be documented and maintained
- A single group may grant access to multiple applications — removing a user from the group revokes all of them (may not be desired)
- App role assignment granularity is limited to one role per group assignment

---

### Option 3: User Self-Service Request (MyApps / My Access)

**How it works:** Users discover and request application access through the **MyApps portal** (myapps.microsoft.com) or the **My Access portal** (myaccess.microsoft.com). Access is granted after approval.

| Attribute | Detail |
|-----------|--------|
| **Who manages** | Users (request); designated approvers (approval); IT (configuration) |
| **Interface** | MyApps portal → Request access; My Access portal → Request access package |
| **Trigger** | User discovers an application and submits a request |
| **Approval workflow** | Configurable: app owner, manager, or IT approves the request |
| **Automation** | Request and approval are automated; assignment happens on approval |
| **Licensing** | MyApps self-service group: Entra ID P1. Access packages: Entra ID Governance |
| **Best suited for** | Empowering users to find and request access without raising tickets |

**Self-Service Mechanisms:**

| Mechanism | How It Works | Configuration |
|-----------|-------------|---------------|
| **Self-service group membership** | Application assigned to a group; users request to join the group via MyApps; group owner approves | Entra Admin Centre → Groups → [Group] → Self-service → Allow users to request membership |
| **Entitlement Management access package** | Application included in an access package; users request via My Access portal; multi-stage approval, time limits, reviews | Entra Admin Centre → Entitlement Management → Access Packages |

**Advantages:**
- Users can find and request access without raising a ticket
- Approval workflow ensures appropriate oversight
- Reduces IT burden for routine access requests
- Access packages provide time limits, expiry and reviews

**Challenges:**
- Users must know the MyApps or My Access portal exists (adoption)
- Self-service group membership has limited governance (no time limits, no SoD)
- Access packages require Entra ID Governance licensing
- Configuration effort to set up self-service for each application

---

### Option 4: Entra ID Entitlement Management (Access Packages)

**How it works:** Application access is bundled into **Access Packages** in Entra ID Entitlement Management. Users request packages through the My Access portal, with defined approval workflows, time limits and periodic reviews.

| Attribute | Detail |
|-----------|--------|
| **Who manages** | Identity / governance team (package creation); business owners (approval via catalogs); users (self-service request) |
| **Interface** | Entra Admin Centre → Entitlement Management → Access Packages; My Access portal |
| **Trigger** | User request via My Access portal; auto-assignment based on rules; direct assignment by admin |
| **Approval workflow** | Configurable: single-stage (manager), multi-stage (manager then app owner), or auto-approved |
| **Automation** | Request-based with optional auto-assignment; automatic expiry and renewal |
| **Licensing** | Requires **Entra ID Governance** licence |
| **Best suited for** | Governed application access; project-based time-limited access; external user access; regulated environments |

**Access Package Components for Applications:**

| Component | Description |
|-----------|-------------|
| **Resource roles** | Specific app roles assigned to the user (e.g. "Reader", "Contributor", "Admin") |
| **Bundled resources** | Multiple applications, groups and SharePoint sites in a single package (e.g. "Finance Analyst" package grants access to SAP, Power BI Finance dashboard and Finance SharePoint site) |
| **Policies** | Who can request, approval requirements, access duration (30 days, 90 days, 1 year), renewal rules, access review schedule |
| **Catalogs** | Groupings delegated to business units — HR manages HR application packages; Finance manages Finance packages |
| **Connected organisations** | Allow external users from specific partner organisations to request application access |
| **Auto-assignment** | Rules that automatically assign the package based on user attributes |
| **Incompatible packages** | Separation of duties — a user cannot hold conflicting packages simultaneously |

**Advantages:**
- Most governance-rich native option
- Time-limited access with automatic expiry
- Built-in access reviews for periodic recertification
- Bundles application access with related resources (groups, sites, roles)
- External user support via connected organisations
- Separation of duties enforcement
- Delegated management via catalogs

**Challenges:**
- Requires Entra ID Governance licensing (additional cost)
- More complex to configure than direct assignment
- Users must learn the My Access portal
- Auto-assignment rules depend on attribute data quality

---

### Option 5: Entra ID Dynamic Groups + Application Assignment

**How it works:** A dynamic security group is assigned to an enterprise application. Entra ID automatically manages group membership based on user attribute rules, and application access follows automatically.

| Attribute | Detail |
|-----------|--------|
| **Who manages** | IT / Identity team (rule creation); HR system (attribute data) |
| **Interface** | Entra Admin Centre → Groups → Dynamic membership rules; Enterprise Applications → Users and groups |
| **Trigger** | User attribute change in Entra ID (synced from AD DS or HR-driven provisioning) |
| **Approval workflow** | None — membership is automatic based on attribute match |
| **Automation** | Fully automated |
| **Licensing** | Requires **Entra ID P1** |
| **Best suited for** | Departmental applications; role-based standard access; applications assigned to all employees |

**Rule Examples:**

```
# All employees in the Finance department
(user.department -eq "Finance") and (user.accountEnabled -eq true)

# All staff in the UK
(user.country -eq "United Kingdom") and (user.userType -eq "Member")

# All managers
(user.directReports -ne null)

# All employees excluding contractors
(user.employeeType -ne "Contractor") and (user.accountEnabled -eq true)
```

**Advantages:**
- Fully automated — zero manual intervention
- Access always reflects current organisational reality
- Leavers are automatically removed when their account is disabled
- Scales to any organisation size

**Challenges:**
- Requires high-quality, consistent attribute data in Entra ID
- No approval workflow — if attributes match, access is granted
- Cannot combine dynamic and assigned (static) membership
- Rule evaluation can take minutes to hours
- No time-limited access — access persists as long as attributes match
- A rule error can grant or revoke access for hundreds of users instantly

---

### Option 6: HR-Driven Provisioning with Lifecycle Workflows

**How it works:** The HR system drives identity creation and attribute updates. **Lifecycle Workflows** trigger automated actions — including application assignment — based on lifecycle events.

| Attribute | Detail |
|-----------|--------|
| **Who manages** | HR (data source); Identity team (workflow configuration) |
| **Interface** | Entra Admin Centre → Lifecycle Workflows |
| **Trigger** | HR event: new hire start date, department change, termination |
| **Approval workflow** | Pre-configured in the workflow |
| **Automation** | Fully automated end-to-end from HR event |
| **Licensing** | Requires **Entra ID Governance** licence |
| **Best suited for** | Enterprise organisations with mature HR systems; day-one application access |

**Lifecycle Workflow Examples:**

| Workflow | Trigger | Application Actions |
|----------|---------|---------------------|
| **Pre-hire** | X days before start date | Create Entra ID account; add to baseline application groups |
| **Joiner — day one** | Employee start date | Add to departmental application groups; generate Temporary Access Pass for first sign-in |
| **Mover** | Department or job title change | Remove from old departmental app groups; add to new departmental app groups |
| **Leaver — last day** | Employment end date | Remove from all application assignments; revoke all refresh tokens; disable account |
| **Post-leaver** | X days after departure | Delete account |

**Advantages:**
- Day-one application access for new hires
- Movers automatically transition between application sets
- Leavers lose all application access automatically
- Driven by authoritative HR data

**Challenges:**
- Application access managed indirectly via group membership
- Requires mature HR system integration
- Custom HR systems require Graph API integration
- Complex app role assignments may require custom extensions

---

### Option 7: IT/Helpdesk via Service Request (ITSM)

**How it works:** Users raise a service request in the ITSM platform. IT fulfils the request after approval by adding the user to the application (directly or via group membership).

| Attribute | Detail |
|-----------|--------|
| **Who manages** | IT helpdesk (fulfilment); manager/app owner (approval) |
| **Interface** | ITSM platform (ServiceNow, Jira, BMC); Entra Admin Centre for fulfilment |
| **Trigger** | Service request ticket |
| **Approval workflow** | ITSM approval workflow |
| **Automation** | Manual fulfilment; optional ITSM-to-Graph-API automation |
| **Licensing** | Base Entra ID + ITSM platform |
| **Best suited for** | Regulated environments requiring ticket-tracked access changes; organisations without Entra ID Governance |

**Advantages:**
- Formal approval via ITSM with audit trail
- Separation of duties (requester, approver, fulfiller)
- Integrates with existing ITSM processes

**Challenges:**
- Slow — users wait hours to days
- IT bottleneck for routine access requests
- No automatic removal on departure or role change (unless ITSM triggers offboarding)
- Does not scale for large application portfolios

---

### Option 8: PowerShell / Graph API Automation

**How it works:** Custom scripts programmatically manage application assignments using **Microsoft Graph API**.

| Attribute | Detail |
|-----------|--------|
| **Who manages** | IT / Identity team |
| **Interface** | PowerShell (Microsoft.Graph module), Azure Automation, Azure Functions |
| **Trigger** | Scheduled, event-driven (webhook) or on-demand |
| **Automation** | Fully automated but requires custom development |
| **Licensing** | Base Entra ID + Azure subscription for hosting |

**Common Graph API Operations:**

| Operation | Graph API Endpoint | PowerShell Cmdlet |
|-----------|-------------------|-------------------|
| List app role assignments | `GET /servicePrincipals/{id}/appRoleAssignedTo` | `Get-MgServicePrincipalAppRoleAssignedTo` |
| Assign user to app | `POST /servicePrincipals/{id}/appRoleAssignedTo` | `New-MgServicePrincipalAppRoleAssignment` |
| Remove user from app | `DELETE /servicePrincipals/{id}/appRoleAssignedTo/{id}` | `Remove-MgServicePrincipalAppRoleAssignment` |
| Assign group to app | `POST /servicePrincipals/{id}/appRoleAssignedTo` (with group principal) | `New-MgServicePrincipalAppRoleAssignment` |
| List all enterprise apps | `GET /servicePrincipals` | `Get-MgServicePrincipal` |
| Get app sign-in activity | `GET /reports/servicePrincipalSignInActivities` | `Get-MgReportServicePrincipalSignInActivity` |

**Advantages:**
- Maximum flexibility — any logic, any data source
- Bulk operations (assign 500 users from CSV)
- Integration with non-Microsoft systems
- Can handle complex app role assignment logic

**Challenges:**
- Custom development and maintenance
- Must handle throttling, pagination and error conditions
- No built-in approval workflow
- Security risk with high-privilege application permissions

---

### Option 9: Third-Party Identity Governance Platforms (IGA)

**How it works:** An enterprise IGA platform manages application assignments as part of a broader identity lifecycle.

| Platform | Integration Method | Key Capabilities |
|----------|--------------------|-----------------|
| **SailPoint IdentityNow / ISC** | Graph API connector + SCIM | Access requests, certifications, SoD, role-based provisioning, access modelling |
| **Saviynt** | Graph API connector | Application-level entitlement governance, risk-based access, fine-grained app roles |
| **Omada Identity** | Graph API connector | Business role management, policy-driven access, compliance reporting |
| **One Identity Manager** | Graph API connector | Full lifecycle management, attestation campaigns, SoD enforcement |
| **CyberArk Identity Governance** | Graph API + SCIM | Privileged application access governance, SoD, certification |

**Advantages:**
- Comprehensive governance across all applications (not just Entra ID)
- Advanced features: role mining, SoD detection, risk scoring, orphan detection
- Unified audit across all systems
- Handles complex provisioning logic

**Challenges:**
- Significant cost and implementation effort
- Requires dedicated IGA operations team
- Graph API connector maintenance as Microsoft updates APIs

---

## 3. Application Registration and Consent Governance

Beyond user assignment, the creation of application registrations and consent to API permissions must be governed.

### 3.1 Application Registration Controls

| Control | Configuration | Effect |
|---------|--------------|--------|
| **Restrict who can register applications** | Entra Admin Centre → User settings → Users can register applications: No | Only users with Application Developer, Application Administrator or Global Administrator role can register apps |
| **App registration ownership** | Each app registration has one or more designated owners | Owners can manage credentials, redirect URIs and API permissions for their application |
| **Verified publisher** | Microsoft publisher verification programme | Apps from verified publishers display a blue checkmark; enables risk-based consent decisions |

### 3.2 Consent Framework

| Consent Type | Description | Governance Risk |
|-------------|-------------|----------------|
| **User consent** | Individual users consent to an app accessing their data (e.g. read profile, read email) | Users may unwittingly grant access to malicious or overprivileged apps |
| **Admin consent** | A tenant administrator grants consent on behalf of all users | Powerful — grants access organisation-wide; must be carefully reviewed |
| **Admin consent workflow** | Users request consent; request routed to admin for review and approval | Controlled process for evaluating new app permissions before granting |

**Governance Recommendations:**

| Recommendation | Detail |
|----------------|--------|
| **Disable user consent** | Entra Admin Centre → Enterprise applications → Consent and permissions → User consent: Do not allow user consent |
| **Enable admin consent workflow** | Entra Admin Centre → Enterprise applications → Admin consent settings → Enable admin consent requests |
| **Review consent permissions** | Regularly audit existing consent grants via Entra Admin Centre → Enterprise applications → [App] → Permissions |
| **Deploy App Governance** | Microsoft Defender for Cloud Apps → App Governance: monitor OAuth app behaviour, detect overprivileged apps, automated remediation |
| **Restrict consent to verified publishers** | Allow user consent only for apps from verified publishers with low-risk permissions |

### 3.3 Application Credential Lifecycle

Application credentials (secrets and certificates) require their own lifecycle management.

| Credential Type | Lifetime | Governance Challenge | Recommendation |
|----------------|----------|---------------------|----------------|
| **Client secret** | Configurable (recommended max 12 months; max 24 months enforced by policy) | Secrets expire, breaking application authentication; long-lived secrets increase compromise risk | Rotate secrets before expiry; monitor expiry via Entra ID recommendations; migrate to certificates or managed identities |
| **Certificate** | Configurable (typically 1–2 years) | Certificate expiry breaks authentication; certificate storage must be secured | Store in Azure Key Vault; automate rotation; monitor expiry |
| **Federated identity credential** | No secret — trust relationship with external IdP | No credential to rotate; trust configuration must be accurate | Preferred for workload identity — eliminates credential management entirely |
| **Managed identity** | No credential — platform-managed | No credential to manage or rotate | Preferred for Azure workloads — most secure option |

**Monitoring:**
- **Entra ID Recommendations** flag app registrations with expiring credentials
- **Entra ID Workbook: App credential expiry** provides a dashboard of upcoming expirations
- **Microsoft Graph API** can query credential expiry dates for automated alerting

---

## 4. Application Lifecycle Stages

### 4.1 Lifecycle Overview

```
┌───────────────┐     ┌───────────────┐     ┌───────────────┐     ┌───────────────┐     ┌───────────────┐
│  Registration │────▶│   Active      │────▶│   Review      │────▶│  Deprecated   │────▶│   Removed     │
│  & Onboarding │     │   (In Use)    │     │  (Periodic)   │     │  (Phase-Out)  │     │  (Deleted)    │
└───────────────┘     └───────────────┘     └───────────────┘     └───────────────┘     └───────────────┘
```

### 4.2 Lifecycle Actions by Stage

| Stage | Actions | Responsible |
|-------|---------|-------------|
| **Registration & Onboarding** | Register app (or add from gallery); configure SSO (SAML/OIDC); configure provisioning (SCIM); set user assignment required = Yes; assign initial users/groups; configure Conditional Access; set sensitivity label; document ownership and purpose | IT / Application team |
| **Active (In Use)** | Monitor sign-in activity; rotate credentials before expiry; review and update API permissions; respond to Entra ID recommendations; manage user assignments via chosen option (manual, group, access package) | App owner / IT |
| **Review (Periodic)** | Run access reviews targeting app role assignments; verify ownership is current; review API permissions for least privilege; check sign-in activity for inactive users; review Conditional Access policy alignment | Governance team / App owner |
| **Deprecated (Phase-Out)** | Notify users of migration timeline; block new user assignments; remove from access packages and self-service catalogs; redirect users to replacement application | IT / App owner |
| **Removed (Deleted)** | Remove all user assignments; revoke API permissions; remove consent grants; delete the enterprise application (service principal); optionally delete the app registration; update documentation and CMDB | IT |

### 4.3 Inactive Application Detection

| Method | Data Source | Detail |
|--------|-----------|--------|
| **Entra ID sign-in logs** | Enterprise application sign-in activity | Filter by application → identify apps with no sign-ins in 90+ days |
| **Entra ID Recommendations** | Built-in Entra ID recommendations engine | Flags unused applications, expiring credentials and overprivileged permissions |
| **Microsoft Graph API** | `servicePrincipalSignInActivities` endpoint | Programmatically query last sign-in date for each application |
| **Entra ID Workbooks** | Pre-built workbook: Application Activity | Visual dashboard of application usage, sign-in trends and inactive apps |
| **App Governance (Defender for Cloud Apps)** | OAuth app monitoring | Detect inactive, overprivileged or suspicious OAuth applications |

---

## 5. Application Provisioning (SCIM)

Automated user provisioning to SaaS applications is a critical governance capability that ensures application-level accounts are created, updated and deprovisioned in sync with Entra ID.

### 5.1 Provisioning Modes

| Mode | Description | Governance Impact |
|------|-------------|-------------------|
| **Manual** | Admin creates accounts in the application directly | No automation; orphaned accounts when users leave; inconsistent attributes |
| **Entra ID automatic provisioning (SCIM)** | Entra ID provisions and deprovisions user accounts in the application via SCIM protocol when user/group assignments change | Automated lifecycle; accounts deprovisioned when Entra ID assignment is removed |
| **HR-driven → Entra ID → SCIM** | HR event creates/updates Entra ID account → SCIM provisions to SaaS app | End-to-end automation from HR event to application account |

### 5.2 Provisioning Scope

| Scope Option | Description |
|-------------|-------------|
| **Assigned users and groups only** | Only users/groups explicitly assigned to the enterprise application are provisioned (recommended) |
| **All users and groups** | All users in the directory are provisioned to the application (use with caution) |
| **Scoping filters** | Attribute-based filters that further restrict which assigned users are provisioned (e.g. only users in a specific department) |

### 5.3 Key Provisioning Challenges

| Challenge | Description | Resolution |
|-----------|-------------|------------|
| **Attribute mapping errors** | Incorrect mapping between Entra ID attributes and application attributes causes provisioning failures | Review and test attribute mappings in staging; use Entra ID provisioning logs to detect failures |
| **Quarantine state** | Persistent provisioning errors cause Entra ID to quarantine the provisioning job, halting all provisioning | Monitor provisioning health in Entra Admin Centre; resolve root cause and restart |
| **Orphaned accounts** | Users removed from Entra ID assignment but application account not deleted (only disabled or soft-deleted) | Configure the provisioning "on deletion" action to delete or disable; audit application accounts periodically |
| **Schema mismatch** | Application expects attributes that Entra ID does not have or cannot populate | Extend Entra ID schema with custom attributes or extension attributes; populate via HR provisioning or Graph API |
| **Non-SCIM applications** | Application does not support SCIM — no automated provisioning possible | Use manual provisioning, custom Graph API scripts, or third-party integration (via IGA platform or iPaaS) |

---

## 6. Conditional Access for Applications

Conditional Access policies control how users access applications based on identity, device, location and risk signals.

### 6.1 Common Application Conditional Access Policies

| Policy | Target | Conditions | Controls |
|--------|--------|-----------|----------|
| **Require MFA for all cloud apps** | All applications | All users | Require MFA |
| **Block legacy authentication** | All applications | Client apps: Exchange ActiveSync, other clients | Block access |
| **Require compliant device for sensitive apps** | HR system, finance system, admin portals | All users | Require compliant device (Intune) |
| **Block access from untrusted locations** | Sensitive applications | Named locations: exclude corporate network | Block access |
| **Require Terms of Use for external users** | All applications | Guest users | Require Terms of Use acceptance |
| **Risk-based access** | All applications | Sign-in risk: medium or high | Require MFA or block |
| **Session control for unmanaged devices** | All applications | Device: not compliant, not hybrid joined | Enforce app-enforced restrictions (read-only in SharePoint/Exchange) |
| **Authentication strength for privileged apps** | Admin portals (Azure, Entra, Exchange) | Admin roles | Require phishing-resistant MFA (FIDO2, WHfB) |

### 6.2 Per-Application vs. Tenant-Wide Policies

| Approach | Detail |
|----------|--------|
| **Tenant-wide baseline** | Apply to all cloud applications (require MFA, block legacy auth) — ensures minimum security posture |
| **Per-application overrides** | Additional policies targeting specific applications for enhanced controls (compliant device, trusted location, authentication strength) |
| **Exclude break-glass accounts** | Emergency access accounts excluded from all Conditional Access to prevent lockout |

---

## 7. Option Comparison Matrix

| Criteria | Manual (Admin) | Group-Based | Self-Service (MyApps) | Access Packages | Dynamic Groups | HR + Lifecycle Workflows | ITSM Ticket | Graph API / PowerShell | Third-Party IGA |
|----------|---------------|------------|----------------------|----------------|---------------|------------------------|-----------|-----------------------|----------------|
| **Automation** | None | Partial | Partial | Partial (request + auto) | Full | Full | None | Full (custom) | Full |
| **Approval workflow** | None | None (unless group has self-service approval) | Group owner or manager | Multi-stage configurable | None | Workflow-defined | ITSM ticket | Custom | Full multi-stage |
| **Time-limited access** | No | No | No | Yes | No (attribute-driven) | Yes (workflow-driven) | No | Custom | Yes |
| **Access reviews** | No | Must add separately | Must add separately | Built-in | Must add separately | Must add separately | No | Custom | Built-in |
| **SoD enforcement** | No | No | No | Yes (incompatible packages) | No | No | No | Custom | Yes |
| **External user support** | Yes (manual) | Yes (if group allows guests) | Limited | Yes (connected orgs) | No (guests excluded) | Limited | Yes (manual) | Yes | Yes |
| **App role granularity** | Full (per-user role) | One role per group | Group-based | Full (per-package role) | One role per group | Group-based | Full (manual) | Full | Full |
| **Scales to 100+ apps** | No | Yes | Yes | Yes | Yes | Yes | No | Yes | Yes |
| **Licence requirement** | Free | P1 | P1 / Governance | Governance | P1 | Governance | Free + ITSM | Free + Azure | IGA platform |
| **Implementation effort** | None | Low | Low–Medium | Medium–High | Medium | High | Low | High | Very High |

---

## 8. Recommended Approach by Organisation Size

### Small Organisation (1–50 employees)

| Recommendation | Rationale |
|----------------|-----------|
| **Option 1 (Manual)** for initial setup | Few applications; few users; admin can manage directly |
| **Option 2 (Group-Based)** as the baseline | Assign groups to applications; manage membership manually or via dynamic rules |
| Set **user assignment required = Yes** on all apps | Foundation for governance — access is explicit |
| Disable **user consent** and enable **admin consent workflow** | Prevent uncontrolled OAuth app access |

### Medium Organisation (50–500 employees)

| Recommendation | Rationale |
|----------------|-----------|
| **Option 2 (Group-Based)** with dynamic groups | Automate departmental and role-based application access |
| **Option 3 (Self-Service)** via MyApps | Empower users to discover and request application access |
| **Option 4 (Access Packages)** for sensitive and project applications | Governed access with approval, time limits and reviews |
| Enable **SCIM provisioning** for all supported SaaS applications | Automate application-level account lifecycle |
| Regular **access reviews** targeting application role assignments | Periodic recertification of who has access to what |

### Large Organisation (500+ employees)

| Recommendation | Rationale |
|----------------|-----------|
| **Option 6 (HR + Lifecycle Workflows)** as the foundation | Automate JML across all baseline applications |
| **Option 4 (Access Packages)** for governed, self-service access | Approval workflows, time limits, reviews, SoD, external users |
| **Option 5 (Dynamic Groups)** for standard departmental applications | Attribute-based automation |
| **SCIM provisioning** for all SaaS applications | End-to-end automated account lifecycle |
| **Option 9 (IGA)** if multi-platform governance is required | Unified governance across Entra ID, SAP, ServiceNow, custom apps |
| **App Governance** for OAuth application monitoring | Detect and remediate overprivileged or malicious applications |
| Comprehensive **Conditional Access** policy framework | Per-application and tenant-wide security policies |

---

## 9. Key Microsoft Technologies Reference

| Technology | Role in Application Governance |
|------------|-------------------------------|
| **Microsoft Entra ID** | Application identity platform — app registrations, service principals, SSO, Conditional Access |
| **Enterprise Applications** | Per-tenant application instances with user assignment, SSO configuration and provisioning |
| **Entra ID Entitlement Management (Governance)** | Access packages for application access with request, approval, expiry and review |
| **Entra ID Access Reviews (Governance/P2)** | Periodic recertification of application role assignments |
| **Entra ID Lifecycle Workflows (Governance)** | Automated JML workflows adjusting application access |
| **Entra ID Dynamic Groups (P1)** | Attribute-based automatic application access via group assignment |
| **SCIM Automatic Provisioning** | Automated user provisioning/deprovisioning in SaaS applications |
| **Conditional Access** | Per-application access policies based on identity, device, location and risk |
| **Admin Consent Workflow** | Governed process for reviewing and approving new application permissions |
| **App Governance (Defender for Cloud Apps)** | OAuth application monitoring, overprivilege detection, automated remediation |
| **Entra ID Recommendations** | Proactive guidance on expiring credentials, unused apps and security improvements |
| **Microsoft Graph API** | Programmatic application and assignment management |
| **Entra ID Workbooks** | Customisable reporting on application activity, provisioning and credential lifecycle |
| **Entra ID Workload Identities Premium** | Conditional Access, Identity Protection and Access Reviews for non-human identities |
