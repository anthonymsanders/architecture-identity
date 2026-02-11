# Common Joiners, Movers and Leavers (JML) Challenges

The Joiners, Movers and Leavers (JML) process is the identity lifecycle that governs how user accounts and access rights are created, modified and revoked as people join an organisation, change roles within it and eventually leave. A poorly managed JML process is one of the most common root causes of security incidents, compliance failures and operational inefficiency in identity management.

This document outlines the common JML challenges organisations face, the risks they create and practical guidance for addressing them.

---

## Common User Lifecycle Events

Before examining the challenges, it is important to understand the range of lifecycle events that a JML process must handle:

### Joiner Events

| Event | Description | Typical Trigger |
|-------|-------------|-----------------|
| **New permanent employee** | Full-time or part-time hire starting employment | HR system new hire record |
| **New contractor / temporary worker** | Fixed-term or agency staff engaged for a project or period | Procurement or hiring manager request |
| **New intern / apprentice** | Short-term placement, often with limited access requirements | HR or programme coordinator request |
| **Re-hire / returning employee** | Former employee returning to the organisation | HR system rehire record |
| **Merger / acquisition joiner** | Employee acquired through M&A activity whose identity must be integrated | M&A integration programme |
| **Guest / visitor** | External individual requiring short-term, limited access (e.g. interviewee, auditor, consultant) | Sponsor or host request |
| **Student enrolment** | Student joining an educational institution | Student records system |
| **Partner / B2B user** | External organisation user requiring access to shared resources | Business relationship agreement |

### Mover Events

| Event | Description | Typical Trigger |
|-------|-------------|-----------------|
| **Department transfer** | Employee moves to a different department or business unit | HR system attribute change |
| **Promotion** | Employee moves to a higher-level role, often requiring additional access | HR system job title or grade change |
| **Role change (lateral)** | Employee moves to a different role at the same level | HR system job title change |
| **Location change** | Employee relocates to a different office, region or country | HR system location attribute change |
| **Manager change** | Employee's reporting line changes | HR system manager attribute change |
| **Contract extension** | Contractor's engagement is extended beyond the original end date | Procurement or hiring manager request |
| **Contract scope change** | Contractor's responsibilities change, requiring different access | Hiring manager request |
| **Leave of absence** | Employee goes on extended leave (parental, medical, sabbatical) | HR system leave record |
| **Return from leave** | Employee returns from extended leave and requires access restoration | HR system return date |
| **Entity transfer (M&A)** | Employee moves between legal entities within the same organisation | HR or integration programme |
| **Student programme change** | Student changes course, faculty or enrolment status | Student records system |

### Leaver Events

| Event | Description | Typical Trigger |
|-------|-------------|-----------------|
| **Voluntary resignation** | Employee leaves of their own accord with a notice period | HR system termination record |
| **Involuntary termination** | Employee is dismissed, potentially requiring immediate access revocation | HR system termination record (urgent) |
| **Retirement** | Employee retires, often with a planned transition period | HR system termination record |
| **Contract end** | Contractor's engagement reaches its agreed end date | Contract end date in procurement or identity system |
| **Death in service** | Employee passes away, requiring sensitive account handling | HR notification |
| **Redundancy / restructure** | Multiple employees leave as part of an organisational change | HR programme |
| **Student graduation / withdrawal** | Student completes studies or withdraws from the institution | Student records system |
| **Guest access expiry** | Visitor or guest account reaches its time limit | Access expiry date |
| **Partner relationship end** | B2B collaboration ceases | Business relationship termination |

---

## Part 1: Joiner Challenges

### 1.1 Day-One Access Delay

**Challenge:** New employees arrive on their first day without the accounts, access and equipment they need to be productive. IT may not receive notification of the new hire until the start date, or provisioning is a manual process that takes days.

**Risks:**
- Lost productivity during the first days or weeks of employment
- New hires sharing colleagues' credentials to work while waiting for their own accounts
- Frustration and poor first impression for the new employee
- Hiring managers bypassing IT processes to get access set up informally

**Good Practice:**
- Connect the identity management system to the **HR system** as the authoritative source. New hire records should trigger automated provisioning workflows **before** the start date (pre-hire provisioning)
- Define **starter access packages** (role-based bundles of accounts, group memberships and application assignments) that are automatically provisioned based on HR attributes (department, job title, location)
- Implement **pre-hire workflows** that create accounts, assign licences, configure email and provision application access in advance so everything is ready on day one
- Issue **temporary access passes** or onboarding credentials that allow new hires to complete MFA registration and device enrolment on their first day
- Establish **SLAs for provisioning** with clear escalation paths when automated provisioning is not available

---

### 1.2 Inconsistent Baseline Access

**Challenge:** Different new hires in the same role receive different levels of access depending on who requested it, which IT staff member processed it and what was copied from a previous employee's account.

**Risks:**
- Over-provisioning (new hires receive more access than they need)
- Under-provisioning (new hires cannot perform their duties)
- Inconsistent security posture across the organisation
- Compliance failures when access cannot be justified by role requirements

**Good Practice:**
- Define **role-based access control (RBAC)** with documented business roles mapped to specific technical entitlements (group memberships, application assignments, mailbox configurations)
- Use **dynamic groups** or **automated role assignment** based on HR attributes to ensure that all users with the same job title, department and location receive the same baseline access
- Prohibit the practice of **cloning access from another user** (often called "copy from" provisioning), as this perpetuates privilege creep and grants access based on an individual's accumulated entitlements rather than the role's actual requirements
- Regularly review and update role definitions to ensure they reflect current business requirements

---

### 1.3 Contractor and External User Onboarding

**Challenge:** Contractors, consultants and external partners are onboarded through ad-hoc processes outside the standard HR workflow. There is no single system of record for external identities, and contractor accounts often lack defined start dates, end dates and sponsors.

**Risks:**
- No visibility into how many external users have access to the organisation's systems
- Contractor accounts without expiry dates remain active indefinitely
- No clear ownership or sponsorship, so nobody is accountable for reviewing or revoking access
- External users receiving the same level of access as permanent employees

**Good Practice:**
- Establish a **dedicated onboarding process for external users** with mandatory fields: sponsor name, business justification, start date, end date, access scope
- Use **B2B collaboration** or **guest accounts** that allow external users to authenticate with their own organisation's credentials, rather than creating local accounts
- Configure **automatic account expiry** aligned to the contract end date
- Require the sponsor to complete an **access review** at defined intervals (e.g. every 90 days) to re-confirm that the external user still requires access
- Apply **differentiated access policies** (e.g. stricter MFA, device compliance requirements, restricted application access) for external user accounts

---

### 1.4 Re-hire and Return Identity Conflicts

**Challenge:** When a former employee is re-hired, the organisation must decide whether to reactivate the old account or create a new one. Re-activating old accounts may restore access that is no longer appropriate. Creating new accounts creates duplicate identities.

**Risks:**
- Re-activated accounts carry stale entitlements from the previous employment period
- Duplicate identities create confusion, audit issues and potential compliance violations
- Audit trails are broken if a new account is created without linking to the previous identity record

**Good Practice:**
- Define a clear **re-hire policy** specifying whether accounts are reactivated or recreated based on the time elapsed since departure and the nature of the new role
- If reactivating an account, **strip all previous entitlements** first and provision fresh access based on the new role's requirements
- Maintain a **link between old and new identity records** in the identity governance system for audit continuity
- Ensure that the HR system correctly identifies re-hires (rather than creating a duplicate employee record) and triggers the appropriate identity workflow

---

### 1.5 M&A Identity Integration

**Challenge:** When organisations merge or acquire other entities, thousands of new identities must be integrated from the acquired organisation's identity infrastructure. The acquired organisation may use different directories, naming conventions, email domains and access management tools.

**Risks:**
- Duplicate identities across the acquiring and acquired organisations
- Access gaps preventing acquired employees from working
- Security exposure from overly broad temporary access granted during integration
- Extended integration timelines leaving shadow identity infrastructure in place

**Good Practice:**
- Establish a **dedicated identity integration workstream** within the M&A programme
- Conduct an **identity discovery** exercise to inventory all accounts, directories, applications and entitlements in the acquired organisation
- Define **interim access arrangements** (e.g. federation, cross-tenant synchronisation, B2B guest access) to provide Day 1 access without rushing full integration
- Plan and execute **identity consolidation** including directory migration, account correlation, naming convention alignment and entitlement rationalisation
- Define the **target state** (single directory, single tenant, unified governance) and work towards it with a phased plan

---

## Part 2: Mover Challenges

### 2.1 Privilege Creep (Entitlement Accumulation)

**Challenge:** When employees change roles, they receive new access for their new role but their old access is rarely revoked. Over time, employees accumulate entitlements far beyond what their current role requires.

**Risks:**
- Violation of the **least-privilege principle**
- Increased blast radius if the account is compromised
- **Separation of duties** violations (employee holds conflicting permissions)
- Compliance audit findings and regulatory penalties
- Insider threat risk from excessive access

**Good Practice:**
- Implement **attribute-based access** using dynamic groups or automated role recalculation. When HR attributes change, the identity system automatically removes old group memberships and adds new ones
- Deploy **mover workflows** that trigger on HR attribute changes, executing a defined sequence: notify the new manager, remove old role-based access, grant new role-based access, flag any non-standard entitlements for review
- Run **access certification campaigns** specifically triggered by role changes, requiring the new manager to certify all access within 30 days of the move
- Track **entitlement count per user** as a metric and flag users with significantly more entitlements than peers in the same role

---

### 2.2 Delayed Access for New Role

**Challenge:** When an employee moves to a new role, there is often a gap between when they start the new role and when they receive the necessary access. During this gap, the employee cannot perform their new duties or relies on colleagues sharing access.

**Risks:**
- Lost productivity during the transition period
- Credential sharing to work around access delays
- Business process delays when key roles are not operational

**Good Practice:**
- Implement **pre-mover provisioning** that grants new role access **before or on** the effective date of the role change, not after
- Ensure the HR system publishes the **future effective date** of role changes so that identity workflows can be scheduled in advance
- Define **transition periods** where both old and new access coexist for a short, defined window (e.g. 5-10 business days), after which old access is automatically removed
- Assign a **mover buddy or handover contact** in the new team who can flag any access gaps quickly

---

### 2.3 Manager Changes Breaking Approval Chains

**Challenge:** When an employee's manager changes (due to restructure, manager departure or lateral move), existing approval workflows, access reviews and delegation chains break. The new manager may not know what access their new direct reports hold or whether it is appropriate.

**Risks:**
- Access review campaigns are sent to the wrong manager (the old one) or to nobody
- Approval requests for access changes are routed incorrectly
- The new manager has no visibility into their team's access posture

**Good Practice:**
- Ensure the **manager attribute** is updated in the HR system **before** or **on** the effective date of the change, and that this propagates to the identity directory
- Configure access review and approval workflows to **dynamically resolve** the current manager from the directory at the time of the review, rather than caching a static manager value
- When a new manager takes over, trigger a **baseline access review** requiring them to certify all direct reports' access within 30 days
- Implement **delegation chains** so that if a manager is unavailable, a designated alternate can approve requests and complete reviews

---

### 2.4 Leave of Absence and Return

**Challenge:** When employees go on extended leave (parental, medical, sabbatical), their accounts and access must be handled appropriately. Leaving accounts fully active during extended absence creates risk, but fully disabling them can cause problems when the employee returns.

**Risks:**
- Active accounts during long absences are targets for compromise (no legitimate user monitoring the account)
- Fully disabled accounts may cause data loss (email delivery failure, OneDrive access, Teams membership)
- Returning employees face delays while access is restored

**Good Practice:**
- Define a **leave of absence policy** that specifies what happens to accounts at defined absence thresholds (e.g. after 30 days: disable interactive sign-in but retain mailbox; after 90 days: disable all access; after 12 months: archive and deprovision)
- Implement **leave workflows** triggered by the HR system's leave record that automatically adjust account status (e.g. block sign-in, revoke active sessions, retain mailbox and data)
- Implement **return workflows** triggered by the HR system's return date that restore access and notify the employee and their manager
- Preserve the employee's **data and group memberships** during the leave period so that restoration is seamless

---

### 2.5 Cross-Entity and Cross-Border Moves

**Challenge:** When employees move between legal entities (e.g. subsidiaries, divisions in different countries) or across national borders, they may move between different identity domains, directories, tenants or regulatory jurisdictions.

**Risks:**
- Employee loses access during the transition between entities
- Identity data is duplicated across tenants or directories
- Regulatory requirements change (e.g. GDPR applies in the EU entity but not in the Canadian entity, or vice versa)
- Employment contract changes trigger different identity policies (e.g. different data residency requirements)

**Good Practice:**
- Map all **cross-entity and cross-border move scenarios** and define identity transition procedures for each
- Determine whether the identity **migrates** (account moves from one directory to another) or **bridges** (account exists in both with federation) during the transition
- Implement a **transition window** with access in both the old and new entity to prevent productivity gaps
- Ensure that **data residency and privacy requirements** for the new jurisdiction are applied to the identity record (e.g. identity data storage location, consent requirements, retention policies)
- Coordinate with HR, Legal and IT in both the source and target entities

---

## Part 3: Leaver Challenges

### 3.1 Delayed Account Disablement

**Challenge:** When an employee leaves, their accounts are not disabled promptly. The most common cause is that IT is not notified in time, or the offboarding process relies on a manual ticket that can be missed or delayed.

**Risks:**
- Former employees retain access to corporate systems, email and data
- If the departure is involuntary (dismissal), the former employee may exfiltrate data or cause damage during the delay
- Compliance violation — many regulations require prompt access revocation upon termination
- Active accounts for former employees are targets for credential-based attacks

**Good Practice:**
- Connect the identity management system to the **HR system** so that termination records automatically trigger account disablement across all connected systems
- Define **offboarding SLAs**: for involuntary terminations, accounts should be disabled within **minutes** (not hours or days); for voluntary departures, accounts should be disabled on or before the last working day
- Implement **Continuous Access Evaluation (CAE)** or equivalent session revocation to terminate active sessions immediately when an account is disabled, rather than waiting for token expiry
- Use **on-demand synchronisation** for hybrid environments so that disablement propagates between on-premises AD and cloud identity providers within minutes

---

### 3.2 Incomplete Access Revocation

**Challenge:** Even when the primary account is disabled, the leaver retains access to systems that are not connected to the central identity platform. This includes SaaS applications with local accounts, shared drives with direct permissions, API keys, SSH keys, service accounts and personal devices with cached credentials.

**Risks:**
- Former employees retain access through unmanaged channels
- Data exposure through orphaned access that nobody is monitoring
- Compliance failures when auditors discover active access for terminated employees

**Good Practice:**
- Maintain a **complete entitlement inventory** for each identity, covering all connected and unconnected systems
- Automate deprovisioning via **SCIM provisioning** or **API-based connectors** to as many target systems as possible, so that disabling the central identity triggers automatic revocation in downstream applications
- Maintain a **manual offboarding checklist** for systems that cannot be automated (shared drives, third-party portals with local accounts, physical access badges, mobile device management)
- Conduct **post-offboarding verification** audits: after the leaver's account is disabled, run an automated check across all connected systems to confirm that access has been fully revoked
- Revoke **personal device access** by wiping corporate data via MDM or revoking device trust

---

### 3.3 Data Retention and Mailbox Handling

**Challenge:** When an employee leaves, their email, files and data must be handled appropriately. Deleting everything immediately may lose business-critical information. Retaining everything indefinitely creates data protection and storage cost issues.

**Risks:**
- Loss of business-critical emails, files and project data
- Regulatory non-compliance if data is deleted before retention periods expire
- Privacy non-compliance if personal data is retained beyond necessary periods
- Storage and licensing costs for indefinitely retained leaver data

**Good Practice:**
- Define a **data retention policy for leavers** specifying retention periods for email, files and application data, aligned with regulatory requirements (e.g. 7 years for financial records, specific periods for HR records)
- Convert the leaver's **mailbox to a shared mailbox** (no licence required) or apply a **litigation hold** or **retention policy** before disabling the account
- Transfer ownership of critical files and **shared resources** (SharePoint sites, Teams, shared drives) to the leaver's manager or a designated successor
- Set up **auto-reply** on the leaver's mailbox for a defined period to redirect enquiries
- After the retention period expires, **archive and then delete** the leaver's data in accordance with the retention policy

---

### 3.4 Involuntary Termination (Urgent Offboarding)

**Challenge:** When an employee is terminated involuntarily (dismissal, disciplinary action, security incident), access must be revoked immediately — often before the employee is informed, and certainly before they leave the building.

**Risks:**
- Data exfiltration in the window between notification and access revocation
- Sabotage or destruction of data and systems
- Reputational damage if confidential information is leaked

**Good Practice:**
- Establish an **urgent offboarding process** with a defined, rehearsed runbook that can be executed within minutes
- The urgent offboarding runbook should include: disable all accounts (AD, Entra ID, VPN, SaaS), revoke all active sessions and tokens (CAE), block mobile device access (MDM wipe), disable physical access (badge revocation), disable remote access (VPN, RDP), notify IT security for monitoring
- Coordinate with **HR and Legal** so that IT is notified **before** the employee is informed of the termination, allowing access to be revoked simultaneously with or before the notification
- Maintain **pre-authorised emergency access revocation** procedures that do not require standard approval workflows (to avoid delays)
- Review **audit logs** for the period immediately before and after the termination for any signs of data exfiltration or unusual activity

---

### 3.5 Orphaned and Unattributed Accounts

**Challenge:** Over time, organisations accumulate accounts that do not correspond to any current employee, contractor or partner. These orphaned accounts may belong to former employees whose offboarding was incomplete, test accounts that were never cleaned up, service accounts with no documented owner or accounts created for temporary purposes.

**Risks:**
- Orphaned accounts are prime targets for attackers (no legitimate user to notice suspicious activity)
- Compliance findings for accounts that cannot be attributed to a current individual
- Licence waste for orphaned cloud accounts consuming paid licences

**Good Practice:**
- Run **regular reconciliation** between the identity directory and the authoritative HR source to identify accounts with no matching active employee record
- Implement **automated dormancy detection** that flags accounts with no sign-in activity beyond a defined threshold (e.g. 90 days) for review or automatic disablement
- Require **all accounts to have a designated owner** (for service accounts) or a matching HR record (for user accounts). Accounts without an owner or HR match should be flagged and investigated
- Conduct **quarterly orphaned account reviews** and report the findings to identity governance stakeholders
- Maintain a **clean-up SLA**: orphaned accounts must be investigated within 5 business days and disabled or re-attributed within 10 business days

---

### 3.6 Service Account and Shared Account Offboarding

**Challenge:** When an employee leaves, their personal accounts are disabled, but service accounts, shared accounts, API keys and automation credentials that they created or managed may be overlooked. These non-personal accounts often have no documented relationship to the departing employee.

**Risks:**
- Service accounts managed by the leaver continue to operate with no oversight
- API keys and secrets known to the leaver remain valid
- Shared accounts whose password was known to the leaver are not rotated
- The leaver retains the ability to access systems via non-personal credentials

**Good Practice:**
- Maintain a **service account ownership register** that links every non-personal account to a responsible individual. When that individual leaves, ownership must be transferred as part of the offboarding process
- Include **credential rotation** in the offboarding checklist: rotate passwords, regenerate API keys and rotate secrets for all service accounts, shared accounts and automation credentials that the leaver had access to
- Audit **application registrations, API keys and OAuth tokens** created by the leaver and transfer ownership or revoke as appropriate
- Include a question in the **exit interview or offboarding form**: "Do you manage or have access to any service accounts, shared accounts, API keys or automation credentials?"

---

## Part 4: Cross-Cutting JML Challenges

### 4.1 HR System as the Authoritative Source

**Challenge:** The HR system is the natural authoritative source for JML events, but in many organisations the HR system is not connected to the identity platform, or the HR data is incomplete, delayed or inaccurate.

**Risks:**
- Identity events are triggered late or not at all
- Provisioning and deprovisioning are based on stale or incorrect data
- Manual processes fill the gap, introducing errors and delays

**Good Practice:**
- Establish the HR system as the **single authoritative source** for employment status, job title, department, location, manager and start/end dates
- Implement a **real-time or near-real-time integration** between the HR system and the identity management platform (via API, SCIM, or connector)
- Define **data quality standards** for the HR system: mandatory fields, validation rules, timeliness requirements
- Establish **SLAs for HR data entry**: new hires must be entered X days before start date; terminations must be entered by close of business on the last working day
- Implement **exception handling** for scenarios where the HR system is not the source (e.g. contractors managed outside HR, guest users sponsored by business)

---

### 4.2 Lack of a Single Identity Record

**Challenge:** Many organisations do not have a single, unified identity record for each person. Instead, identity information is fragmented across the HR system, Active Directory, Entra ID, SaaS applications, email systems and physical access systems. There is no single place to see all of a person's accounts, entitlements and lifecycle status.

**Risks:**
- Offboarding misses accounts in systems that are not visible from the central identity view
- Movers receive new access but old access in disconnected systems is not revoked
- Auditors cannot produce a complete access report for a single individual
- Duplicate identities across systems cause confusion and compliance issues

**Good Practice:**
- Deploy an **Identity Governance and Administration (IGA)** platform that aggregates identity data from all connected systems into a unified identity record
- Establish **correlation rules** that link accounts across systems to a single person (e.g. matching on employee ID, email address or unique identifier)
- Use the unified identity record as the basis for **access reviews, offboarding and compliance reporting**
- Conduct periodic **identity reconciliation** across all systems to identify unlinked or orphaned accounts

---

### 4.3 Manual Processes and Tribal Knowledge

**Challenge:** JML processes rely on manual steps, institutional knowledge and individual heroics rather than documented, automated workflows. The process works when the experienced IT staff member is available but breaks down during holidays, sick leave or staff turnover.

**Risks:**
- Process failures when key individuals are unavailable
- Inconsistent execution depending on who processes the request
- No audit trail for manual actions
- Inability to scale as the organisation grows

**Good Practice:**
- **Document all JML processes** as formal procedures with defined steps, responsibilities, SLAs and escalation paths
- **Automate** as many steps as possible through the identity management platform: automated provisioning, automated deprovisioning, automated access reviews, automated notifications
- Implement **workflow orchestration** so that each step triggers the next, with built-in escalation if a step is not completed within the SLA
- Maintain a **manual offboarding checklist** for the steps that cannot yet be automated, and assign clear ownership for each step
- **Test JML processes regularly** (e.g. quarterly tabletop exercises) to verify that they work correctly under normal and urgent conditions

---

### 4.4 Audit and Compliance Evidence

**Challenge:** Organisations cannot demonstrate to auditors that their JML processes are working effectively. There is no evidence that accounts were provisioned correctly, that access was reviewed when roles changed, or that accounts were disabled promptly when employees left.

**Risks:**
- Audit findings and remediation costs
- Regulatory penalties for non-compliance (GDPR, NIS2, SOX, PIPEDA, HIPAA)
- Inability to respond to data subject access requests (who had access to this data, when and why?)
- Reputational damage from publicised compliance failures

**Good Practice:**
- Ensure all JML events generate **auditable log entries** with timestamps, actor identification, action taken and outcome
- Maintain **provisioning and deprovisioning logs** showing when accounts were created, modified and disabled, and by what trigger (HR event, manual request, automated workflow)
- Retain **access review evidence** showing who reviewed what, when they reviewed it and what decision they made
- Generate **regular JML compliance reports** showing: average time to provision (joiner), average time to revoke old access (mover), average time to disable accounts (leaver), number of orphaned accounts, access review completion rates
- Present JML metrics to **identity governance stakeholders** on a regular cadence (monthly or quarterly)

---

### 4.5 Handling Exceptions and Edge Cases

**Challenge:** Standard JML processes handle the common scenarios (permanent employee joins, changes role, leaves) but break down for edge cases such as: employees with multiple roles, employees who are also contractors, board members who are not in the HR system, interns who transition to permanent employees, employees on secondment to another organisation.

**Risks:**
- Edge cases fall through the cracks and are handled ad-hoc
- Accounts are duplicated or misconfigured
- Access is granted without proper authorisation or lifecycle management

**Good Practice:**
- **Document all known edge cases** and define specific procedures for each
- Establish a **JML exceptions register** to track non-standard scenarios, decisions and outcomes
- Assign a **JML process owner** who is responsible for identifying new edge cases, defining procedures and ensuring they are handled consistently
- Implement **flexible identity lifecycle workflows** that can accommodate non-standard scenarios (e.g. multiple active roles, dual identity types, manual override with documented justification)
- Review the exceptions register **quarterly** to identify patterns and incorporate recurring edge cases into the standard process

---

## Part 5: JML Maturity Model

| Dimension | Level 1: Ad-hoc | Level 2: Defined | Level 3: Managed | Level 4: Optimised |
|-----------|-----------------|------------------|------------------|--------------------|
| **Joiner provisioning** | Manual, ticket-based, days to complete | Documented process, partially automated, hours to complete | Fully automated from HR trigger, minutes to complete with pre-hire provisioning | Predictive provisioning, self-service fine-tuning, real-time readiness verification |
| **Mover access adjustment** | Not addressed; access only added, never removed | Documented process; old access removed on request | Automated entitlement recalculation on HR attribute change with transition window | Continuous entitlement optimisation with peer comparison analytics |
| **Leaver deprovisioning** | Manual, often delayed by days or weeks | Documented process, same-day SLA for planned departures | Automated from HR trigger, minutes for planned; immediate for involuntary | Automated with post-offboarding verification, data handling and credential rotation |
| **HR integration** | No integration; IT notified by email or ticket | Batch file or scheduled sync (daily) | Real-time or near-real-time API/SCIM integration | Bi-directional integration with data quality monitoring and exception handling |
| **Access reviews** | None or annual paper-based review | Scheduled campaigns for high-risk access | Recurring automated campaigns for all access with auto-remediation | Continuous access intelligence with ML-assisted recommendations |
| **Audit evidence** | No systematic evidence; relies on email trails | Basic logging; manual report generation | Comprehensive audit logs with automated compliance reporting | Real-time governance dashboards with predictive compliance analytics |
| **External user lifecycle** | Unmanaged; accounts persist indefinitely | Defined process with manual expiry management | Automated expiry, sponsor reviews and differentiated access policies | Full lifecycle automation with risk-based continuous assessment |
| **Service account management** | Unmanaged; no ownership records | Ownership register maintained; manual rotation | Automated rotation; included in access reviews | Credential-free workload identity where possible; continuous monitoring |

---

## Part 6: Key JML Metrics

| Metric | Description | Target |
|--------|-------------|--------|
| **Time to provision (joiner)** | Elapsed time from HR start date to all baseline access being active | < 4 hours (pre-hire provisioning: ready on Day 1) |
| **Time to adjust (mover)** | Elapsed time from HR role change to old access removed and new access granted | < 24 hours for automated; < 5 business days for manual |
| **Time to disable (leaver — planned)** | Elapsed time from last working day to all accounts disabled | < 1 hour |
| **Time to disable (leaver — involuntary)** | Elapsed time from termination decision to all accounts disabled and sessions terminated | < 15 minutes |
| **Orphaned account rate** | Percentage of active accounts with no matching active HR record | < 1% |
| **Privilege creep index** | Average entitlement count for movers vs new joiners in the same role | Ratio < 1.2 (movers should have no more than 20% more entitlements than fresh joiners) |
| **Access review completion rate** | Percentage of assigned access reviews completed within the review period | > 95% |
| **Post-offboarding verification rate** | Percentage of leavers with confirmed full access revocation across all systems | > 99% |
| **External user expiry compliance** | Percentage of external accounts with a defined and enforced expiry date | 100% |
