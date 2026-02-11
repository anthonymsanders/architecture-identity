# Common Microsoft Identity Issues and Challenges

**Date:** 11 February 2026

---

## Overview

This document examines the identity objects that underpin email and distribution list functionality in Microsoft environments. It clarifies which objects have an Active Directory (AD DS) account, which have a Microsoft 365 (Entra ID) representation, and what the identity management requirements are when these objects exist on-premises (Exchange Server), in the cloud (Exchange Online) or in both (hybrid).

Understanding the identity backing of each mail-enabled object is essential for provisioning, lifecycle management, access control, compliance and hybrid synchronisation.

---

## 1. Mail-Enabled Object Types and Their Identity Backing

The following table catalogues every common mail-enabled object type and identifies whether it has an underlying identity in AD DS, Entra ID or both.

| Object Type | AD DS Object | Entra ID Object | Exchange On-Premises | Exchange Online | Has a Sign-in Identity | Licence Required |
|-------------|-------------|-----------------|---------------------|----------------|----------------------|-----------------|
| **User Mailbox** | User account (`objectClass: user`) | Entra ID user | Yes | Yes | Yes — user authenticates | Yes (Exchange Online Plan or M365) |
| **Shared Mailbox** | Disabled user account (`objectClass: user`, `userAccountControl: disabled`) | Entra ID user (sign-in blocked) | Yes | Yes | No — delegates sign in with their own identity | No (up to 50 GB); Yes if > 50 GB or archive needed |
| **Room Mailbox** | Disabled user account with `msExchRecipientTypeDetails: Room` | Entra ID user (sign-in blocked) | Yes | Yes | Optionally — for Teams Rooms device sign-in | Yes if Teams Rooms licence needed |
| **Equipment Mailbox** | Disabled user account with `msExchRecipientTypeDetails: Equipment` | Entra ID user (sign-in blocked) | Yes | Yes | No | No (typically) |
| **Distribution Group (DG)** | Security or distribution group (`objectClass: group`) | Entra ID group (mail-enabled, not security) | Yes | Yes (synced or cloud) | No — no sign-in; mail routing only | No |
| **Mail-Enabled Security Group (MESG)** | Security group with `mail` attribute (`objectClass: group`, `groupType: security`) | Entra ID security group (mail-enabled) | Yes | Yes | No — no sign-in; used for permissions AND mail routing | No |
| **Dynamic Distribution Group (DDG)** | No AD DS object — exists only in Exchange | No Entra ID object — exists only in Exchange Online | Yes (Exchange on-prem only) | Yes (Exchange Online only) | No | No |
| **Microsoft 365 Group** | No native AD DS object (unless group writeback enabled) | Entra ID unified group (`groupTypes: Unified`) | No (unless hybrid writeback) | Yes — native to M365 | No — but members sign in to access resources | Yes (members need M365 licence) |
| **Mail Contact** | Contact object (`objectClass: contact`) with `mail` and `targetAddress` | Entra ID contact (mail-enabled) | Yes | Yes | No — represents an external email address | No |
| **Mail User (Mail-Enabled User)** | User account with `targetAddress` pointing externally | Entra ID user (mail-enabled, no mailbox) | Yes | Yes | Yes — user can sign in but mailbox is external | Depends on usage |
| **Public Folder Mailbox** | Disabled user account backing the public folder hierarchy | Entra ID user (sign-in blocked) | Yes | Yes | No | Yes (Exchange Online licence for hosting mailbox) |

---

## 2. Distribution Lists — Deep Dive

Distribution lists are one of the most common mail-enabled objects and one of the most poorly governed. This section examines each distribution list type in detail.

### 2.1 Distribution Group (DG)

**What it is:** A group whose sole purpose is to route email to its members. It has no security permissions — it cannot be used to grant access to resources.

**On-Premises Exchange (AD DS):**
| Attribute | Detail |
|-----------|--------|
| AD object class | `group` with `groupType` set to distribution (not security) |
| AD attributes | `mail`, `proxyAddresses`, `managedBy`, `member`, `msExchRequireAuthToSendTo` |
| Identity backing | The group is an AD DS object with a Distinguished Name (DN), `objectGUID` and `objectSID` (if universal) |
| Ownership | `managedBy` attribute defines the owner (typically a single user DN) |
| Membership management | Manual — administrators or the designated owner add/remove members via Exchange Management Console, EAC or PowerShell |
| Nesting | Can contain users, contacts, other distribution groups and mail-enabled security groups |
| Scope | Universal, Global or Domain Local (universal required for cross-forest mail flow and hybrid sync) |

**Exchange Online (M365):**
| Attribute | Detail |
|-----------|--------|
| Entra ID representation | Synced from AD DS via Entra Connect (if hybrid) or created natively in Exchange Online |
| Entra object type | Mail-enabled group (not a security group, not a unified/M365 group) |
| Management | Exchange Online Admin Centre, PowerShell (`Set-DistributionGroup`) or Entra Admin Centre (limited) |
| Ownership | `ManagedBy` property; multiple owners supported in Exchange Online |
| Self-service | Can be configured to allow members to join/leave without admin approval |

**Hybrid Identity Considerations:**
| Consideration | Detail |
|---------------|--------|
| **Sync direction** | AD DS → Entra ID (via Entra Connect). The on-premises group is the authoritative source. Changes made in Exchange Online are overwritten on the next sync cycle |
| **Attribute mastering** | `mail`, `proxyAddresses`, `member` and `managedBy` are mastered on-premises. Cloud-side edits to synced groups are blocked or reverted |
| **Membership management** | Members must be added/removed on-premises for synced groups. Adding a cloud-only user to a synced DG requires that user to have an on-premises AD object (or use group writeback v2) |
| **Cross-premises members** | A synced DG can contain on-premises mailbox users and Exchange Online mailbox users. Mail routing handles cross-premises delivery transparently |
| **GAL visibility** | The group appears in the Global Address List (GAL) in both on-premises and Exchange Online environments after sync |
| **Deletion** | Deleting the on-premises AD group removes it from Entra ID on the next sync cycle. There is no soft-delete or recycle bin for synced groups |

---

### 2.2 Mail-Enabled Security Group (MESG)

**What it is:** A security group that also has an email address. It serves a dual purpose: granting permissions to resources (SharePoint sites, file shares, applications) AND routing email to its members.

**On-Premises Exchange (AD DS):**
| Attribute | Detail |
|-----------|--------|
| AD object class | `group` with `groupType` set to security |
| AD attributes | Same as DG plus security-relevant attributes (`objectSID`, ACL membership) |
| Identity backing | Full AD DS security principal with SID — can be used in ACLs, NTFS permissions, Conditional Access group assignments and application role assignments |
| Dual purpose risk | Changing membership for email purposes inadvertently changes security permissions (and vice versa) |

**Exchange Online (M365):**
| Attribute | Detail |
|-----------|--------|
| Entra ID representation | Entra ID security group with mail enabled |
| Security use | Can be assigned to Entra ID roles, Conditional Access policies, application assignments and SharePoint Online permissions |
| Mail use | Simultaneously functions as an email distribution endpoint |

**Hybrid Identity Considerations:**
| Consideration | Detail |
|---------------|--------|
| **Sync and mastering** | Identical to DG — on-premises AD is authoritative for synced MESGs |
| **Security + mail coupling risk** | The most significant governance challenge. Adding a user for email distribution simultaneously grants them security access to all resources protected by that group. Removing them for security reasons also removes them from the distribution list |
| **Recommendation** | Decouple security and distribution where possible. Use a dedicated DG for email and a separate security group for access control. Where decoupling is not feasible, document the dual purpose and ensure both security and email owners approve membership changes |
| **Conditional Access** | MESGs synced to Entra ID can be used in Conditional Access policy assignments. Membership changes on-premises affect cloud policy enforcement within one sync cycle |

---

### 2.3 Dynamic Distribution Group (DDG)

**What it is:** A distribution group whose membership is calculated at the time of email delivery based on an LDAP filter (on-premises) or OPATH filter (Exchange Online) applied against recipient attributes.

**On-Premises Exchange:**
| Attribute | Detail |
|-----------|--------|
| AD object class | `msExchDynamicDistributionList` — a dedicated AD object class, NOT a standard group |
| Identity backing | AD object with `objectGUID` but no `objectSID` — it is not a security principal and cannot be used for permissions |
| Membership | Not stored — calculated at send time by the Exchange transport pipeline querying AD against the recipient filter |
| Filter examples | `((RecipientType -eq 'UserMailbox') -and (Department -eq 'Finance'))` |
| Management | Exchange Management Shell (`New-DynamicDistributionGroup`, `Set-DynamicDistributionGroup`) |

**Exchange Online:**
| Attribute | Detail |
|-----------|--------|
| Entra ID representation | **None** — DDGs do not sync to Entra ID via Entra Connect and have no Entra ID object |
| Management | Exchange Online PowerShell only — not visible in the Entra Admin Centre |
| Membership | Calculated at send time from Exchange Online recipient data |
| Filter scope | Queries Exchange Online recipients only — does not evaluate on-premises mailboxes |

**Hybrid Identity Considerations:**
| Consideration | Detail |
|---------------|--------|
| **No sync** | DDGs are **not synchronised** by Entra Connect. An on-premises DDG and an Exchange Online DDG are completely independent objects |
| **Split membership problem** | In a hybrid environment, an on-premises DDG only evaluates on-premises recipients and an Exchange Online DDG only evaluates cloud recipients. There is no single DDG that spans both environments |
| **Workaround** | Create matching DDGs in both environments with equivalent filters, or migrate all mailboxes to Exchange Online and use a single cloud DDG |
| **Identity gap** | Because DDGs have no Entra ID representation, they cannot be governed via Entra ID access reviews, lifecycle policies or group expiration. Governance must be handled entirely within Exchange administration |

---

### 2.4 Microsoft 365 Group (Unified Group)

**What it is:** A cloud-native group that provides a shared mailbox, calendar, SharePoint site, Planner board and OneNote notebook. It is also the identity object backing every Microsoft Teams team.

**Exchange Online (M365):**
| Attribute | Detail |
|-----------|--------|
| Entra ID representation | Entra ID unified group (`groupTypes: ["Unified"]`) — this IS the authoritative object |
| Identity backing | Entra ID group object with `objectId` — no on-premises AD object by default |
| Mailbox | Every M365 group has a group mailbox in Exchange Online for shared conversations |
| Membership | Managed in Entra ID (directly, via dynamic membership rules with Entra ID P1, or via access packages) |
| Ownership | One or more Entra ID users designated as owners |
| Governance | Subject to Entra ID group expiration, naming policies, creation restrictions, access reviews and sensitivity labels |

**On-Premises (AD DS) — Group Writeback:**
| Attribute | Detail |
|-----------|--------|
| Group writeback v1 | M365 groups can be written back to AD DS as **universal distribution groups**. They appear in the on-premises GAL but cannot be used for security permissions |
| Group writeback v2 | Writes back M365 groups as **security groups or distribution groups** (configurable). Enables on-premises applications and file shares to use M365 group membership for access control |
| AD object | Written-back groups have an AD DS group object with `mail`, `proxyAddresses` and `member` attributes populated from Entra ID |
| Mastering | Entra ID is authoritative. Membership changes made on-premises to a written-back group are overwritten on the next sync cycle |

**Hybrid Identity Considerations:**
| Consideration | Detail |
|---------------|--------|
| **Cloud-first object** | M365 groups are created and mastered in Entra ID. This is the opposite direction to DGs and MESGs which are mastered on-premises |
| **On-premises visibility** | Without group writeback, M365 groups are invisible to on-premises Exchange, file servers and applications |
| **Teams dependency** | Every Teams team is backed by an M365 group. Deleting the group deletes the team and all associated content (SharePoint site, mailbox, Planner). This coupling means group governance directly impacts Teams governance |
| **Licence requirement** | Members need an M365 licence to access group resources (mailbox, SharePoint, Teams) |
| **Migration path** | Organisations migrating from on-premises DGs to M365 groups must plan for the change in mastering direction, governance model and membership management tooling |

---

## 3. Identity Management Requirements Comparison

### 3.1 On-Premises Exchange Only

When all mailboxes and distribution lists reside on-premises Exchange Server with AD DS.

| Requirement | Detail |
|-------------|--------|
| **Identity store** | Active Directory Domain Services (AD DS) is the sole identity store |
| **Account creation** | All mail-enabled objects (users, groups, contacts) are created in AD DS and mail-enabled via Exchange Management tools |
| **Authentication** | Kerberos / NTLM for internal access; forms-based or certificate-based for OWA; AD FS or hybrid for external access |
| **Group management** | AD DS tools (ADUC, PowerShell, Exchange Management Console) — no self-service unless third-party tools are deployed |
| **Dynamic groups** | Exchange on-premises DDGs query AD DS recipients at send time |
| **Lifecycle management** | Manual — no native group expiration, no access reviews, no automated ownership management |
| **Audit and compliance** | Exchange message tracking logs, AD DS security event logs, third-party archiving solutions |
| **GAL management** | Single GAL populated from AD DS; address book policies for segmentation |
| **Delegation** | OU-based delegation in AD DS; `managedBy` for group ownership |
| **Key identity challenges** | <ul><li>No automated group lifecycle — stale groups persist indefinitely</li><li>No self-service group management without third-party tools</li><li>DDG filter maintenance requires PowerShell expertise</li><li>MESG dual-purpose coupling (security + email) is hard to govern</li><li>No native access reviews or certification campaigns</li><li>Group nesting creates complex, opaque membership hierarchies</li><li>Service accounts for Exchange transport and connectors require manual credential rotation</li></ul> |

---

### 3.2 Exchange Online (M365) Only

When all mailboxes and distribution lists reside in Exchange Online with Entra ID.

| Requirement | Detail |
|-------------|--------|
| **Identity store** | Microsoft Entra ID is the sole identity store |
| **Account creation** | Users created in Entra ID (directly, via HR-driven provisioning, or via Lifecycle Workflows); groups created in Entra Admin Centre, Exchange Online Admin Centre or Teams |
| **Authentication** | Entra ID authentication with Conditional Access, MFA, passwordless and risk-based policies |
| **Group management** | Entra Admin Centre, Exchange Online Admin Centre, Teams Admin Centre, PowerShell, Graph API — self-service available natively |
| **Dynamic groups** | Entra ID dynamic groups (attribute-based membership rules, P1 licence) for security and M365 groups; Exchange Online DDGs for distribution-only scenarios |
| **Lifecycle management** | M365 group expiration policies, Entra ID access reviews, Lifecycle Workflows, access package assignment policies with expiry |
| **Audit and compliance** | Entra ID audit logs, Exchange Online message trace, Microsoft Purview unified audit log, Microsoft Sentinel integration |
| **GAL management** | Exchange Online GAL; address book policies; M365 group visibility policies; information barriers |
| **Delegation** | Entra ID Administrative Units; group-based delegation; Entitlement Management catalogs |
| **Key identity challenges** | <ul><li>M365 group sprawl — any licensed user can create groups by default, each generating a mailbox, SharePoint site and potentially a Teams team</li><li>DGs and MESGs created in Exchange Online have limited Entra ID governance (no expiration, limited access review support)</li><li>DDGs have NO Entra ID representation — invisible to identity governance tooling</li><li>Cloud-only DGs cannot be managed via on-premises tools if any on-premises infrastructure remains</li><li>Shared mailboxes appear as disabled user accounts in Entra ID, consuming identity count but not licensing (until 50 GB exceeded)</li><li>Room and equipment mailboxes require careful identity management for Teams Rooms integration</li><li>Mail contacts represent external identities with no sign-in — cannot be governed via Conditional Access or access reviews</li></ul> |

---

### 3.3 Hybrid (On-Premises Exchange + Exchange Online)

When mailboxes and distribution lists exist in both environments, synchronised via Entra Connect.

| Requirement | Detail |
|-------------|--------|
| **Identity store** | AD DS (on-premises) + Entra ID (cloud), synchronised via Entra Connect |
| **Source of authority** | **On-premises AD DS is authoritative** for all synced objects. Cloud-side changes to synced objects are blocked or overwritten |
| **Account creation** | Users and groups created on-premises in AD DS, then synced to Entra ID. Cloud-only objects can also be created in Entra ID but are not represented on-premises (unless group writeback is enabled) |
| **Authentication** | Entra ID for cloud access (with pass-through authentication, password hash sync or federation); AD DS for on-premises access |
| **Group management** | Synced groups must be managed on-premises. Cloud-only groups managed in Entra ID. This dual management model is a major source of confusion and error |
| **Lifecycle management** | Limited — M365 group expiration applies to cloud groups only. Synced DGs and MESGs have no expiration capability. Access reviews can review membership but cannot directly modify synced group membership |
| **Sync scope and filtering** | Entra Connect sync rules determine which OUs, groups and objects are in scope. Misconfigured filtering leads to missing or unexpected objects in Entra ID |

#### Hybrid Distribution List Identity Challenges

| Challenge | Detail |
|-----------|--------|
| **Dual management surface** | Synced DGs and MESGs must be managed on-premises. Cloud DGs and M365 groups must be managed in Entra ID/Exchange Online. Administrators must know which management surface to use for each group, and using the wrong one causes errors or silent overwrites |
| **Attribute mastering conflicts** | `proxyAddresses`, `mail`, `member` and `managedBy` are mastered on-premises for synced groups. Attempts to modify these in Exchange Online are rejected with `"This object is read-only because it was synced from your on-premises environment"` |
| **Cross-premises membership** | A synced DG can contain members with on-premises mailboxes AND members with Exchange Online mailboxes. Mail routing handles this correctly, but membership management must occur on-premises regardless of where the member's mailbox resides |
| **DDG split-brain** | On-premises DDGs only evaluate on-premises recipients. Exchange Online DDGs only evaluate cloud recipients. In a hybrid environment with mailboxes in both locations, no single DDG can address all recipients matching a given filter. This is one of the most misunderstood hybrid identity issues |
| **M365 group writeback gap** | Without group writeback v2, M365 groups (including Teams-backed groups) have no on-premises AD representation. On-premises Exchange cannot route mail to them, on-premises applications cannot use their membership for access control, and they are invisible to on-premises administrators |
| **MESG security-email coupling in hybrid** | A synced MESG grants both on-premises security permissions (NTFS, SharePoint on-prem) and Entra ID security permissions (Conditional Access, SharePoint Online, application assignments). Membership changes for email purposes have security consequences in both environments |
| **Shared mailbox identity mismatch** | On-premises shared mailboxes are disabled user accounts in AD DS. When synced to Entra ID, they appear as disabled Entra ID users. If migrated to Exchange Online, the on-premises object must be converted to a remote shared mailbox (`RemoteSharedMailbox`) while retaining the Entra ID user for sync. Mishandling this creates authentication failures, GAL inconsistencies and licence assignment issues |
| **Contact vs. mail user confusion** | On-premises mail contacts (representing external recipients) sync to Entra ID as contact objects. On-premises mail users (internal users with external mailboxes) sync as Entra ID users. The distinction affects licensing, Conditional Access applicability and governance scope |
| **GAL synchronisation gaps** | If Entra Connect filtering excludes certain OUs or object types, groups or contacts may appear in the on-premises GAL but not in Exchange Online (or vice versa). Users report missing recipients and resort to manual email address entry, bypassing DLP and transport rules |
| **Group nesting and sync limits** | Deeply nested groups sync to Entra ID, but Entra ID evaluates nested membership differently to AD DS. Groups nested more than two levels deep may not resolve correctly for dynamic membership evaluation, Conditional Access or access reviews |
| **Decommission sequencing** | When migrating from hybrid to cloud-only, distribution lists must be migrated (recreated in Exchange Online) before the on-premises Exchange servers are decommissioned. Deleting on-premises AD groups before establishing cloud equivalents permanently removes the groups from Entra ID |

---

## 4. Identity Lifecycle Requirements by Object Type

This section maps each mail-enabled object type to its identity lifecycle requirements across the Joiners, Movers and Leavers (JML) process.

### 4.1 User Mailbox

| Lifecycle Event | On-Premises | Exchange Online | Hybrid |
|----------------|-------------|-----------------|--------|
| **Joiner** | Create AD user → mail-enable in Exchange | Create Entra ID user → licence assigns mailbox | Create AD user → Entra Connect syncs → licence assigns Exchange Online mailbox |
| **Mover** | Update AD attributes (department, title) → DG/DDG membership may change | Update Entra ID attributes → dynamic group membership adjusts | Update AD attributes → sync to Entra ID → cloud dynamic groups adjust; on-premises DG membership updated manually or via automation |
| **Leaver** | Disable AD account → mailbox retained per policy → eventually delete | Disable Entra ID user → convert to inactive mailbox or shared mailbox → delete after retention | Disable AD account → sync disables Entra ID user → mailbox converted → licence removed |

### 4.2 Shared Mailbox

| Lifecycle Event | On-Premises | Exchange Online | Hybrid |
|----------------|-------------|-----------------|--------|
| **Creation** | Create disabled AD user → mail-enable as shared | Create shared mailbox in Exchange Online (creates disabled Entra ID user) | Create on-premises shared mailbox → migrate to Exchange Online → convert to remote shared mailbox |
| **Delegate change** | Update `FullAccess` and `SendAs` permissions on-premises | Update permissions in Exchange Online | Permissions mastered on-premises for synced objects; Exchange Online for cloud-only shared mailboxes |
| **Decommission** | Remove mailbox → delete or retain AD object | Remove mailbox → Entra ID user object deleted or retained | Remove on-premises object → sync removes Entra ID object → mailbox purged after retention |

### 4.3 Distribution Group

| Lifecycle Event | On-Premises | Exchange Online | Hybrid (Synced) |
|----------------|-------------|-----------------|-----------------|
| **Creation** | Create AD group → mail-enable in Exchange | Create DG in Exchange Online Admin Centre | Create on-premises AD group → mail-enable → Entra Connect syncs to Exchange Online |
| **Membership change** | Add/remove members in AD DS (ADUC, PowerShell) | Add/remove members in EAC or PowerShell | **Must modify on-premises** — cloud-side changes rejected for synced groups |
| **Ownership change** | Update `managedBy` in AD DS | Update `ManagedBy` in Exchange Online | Update on-premises `managedBy` → syncs to Exchange Online |
| **Review** | Manual — no native recertification | Entra ID access reviews (limited for non-M365 groups) | Access reviews can report on membership but cannot directly modify synced group membership |
| **Decommission** | Delete AD group → Exchange mail-disables automatically | Delete DG in Exchange Online | Delete on-premises AD group → sync removes Entra ID group and Exchange Online DG |

### 4.4 Dynamic Distribution Group

| Lifecycle Event | On-Premises | Exchange Online | Hybrid |
|----------------|-------------|-----------------|--------|
| **Creation** | Create DDG in Exchange Management Shell with LDAP recipient filter | Create DDG in Exchange Online PowerShell with OPATH recipient filter | Create independently in each environment — DDGs do not sync |
| **Filter update** | Modify filter via PowerShell | Modify filter via PowerShell | Must be updated independently in each environment |
| **Membership review** | Preview membership via `Get-DynamicDistributionGroup | Get-Recipient -RecipientPreviewFilter` | Same command in Exchange Online PowerShell | Must preview independently in each environment; membership will differ because recipient populations differ |
| **Decommission** | Delete via Exchange Management Shell | Delete via Exchange Online PowerShell | Must delete independently in each environment |

### 4.5 Microsoft 365 Group

| Lifecycle Event | On-Premises | Exchange Online | Hybrid |
|----------------|-------------|-----------------|--------|
| **Creation** | N/A (cloud-native) — visible on-premises only with group writeback | Created in Entra ID, Teams, Outlook or SharePoint | Created in cloud → optionally written back to AD DS via group writeback |
| **Membership change** | Cannot modify on-premises (read-only writeback) | Managed in Entra ID, Teams or Outlook | Managed in cloud; on-premises writeback is read-only |
| **Expiration** | N/A | M365 group expiration policy (Entra ID) | Same — cloud policy applies |
| **Access review** | N/A | Entra ID access reviews fully supported | Same — cloud governance applies |
| **Decommission** | Writeback object removed automatically | Soft-delete → 30-day recoverability → permanent delete | Cloud deletion removes writeback object on next sync cycle |

---

## 5. Common Identity Issues and Anti-Patterns

### 5.1 Management Surface Confusion

| Issue | Description | Impact | Resolution |
|-------|-------------|--------|------------|
| **Editing synced groups in the cloud** | Administrators attempt to add members to a synced DG in Exchange Online Admin Centre | Changes are silently reverted on the next Entra Connect sync cycle, causing confusion and apparent "bugs" | Establish clear documentation and training: synced objects (identified by `DirSyncEnabled: True`) must be managed on-premises |
| **Creating DGs in the wrong location** | New DGs created in Exchange Online when the organisation standard is on-premises mastering | Inconsistent management surface; some groups managed on-premises, others in the cloud | Define and enforce a group creation standard: all DGs created on-premises for synced management, OR migrate fully to cloud |
| **M365 group vs. DG confusion** | Users create M365 groups when a simple DG is needed, or request DGs when an M365 group would be more appropriate | M365 groups create a mailbox, SharePoint site and Teams team — excessive resource creation for simple email distribution. DGs lack the collaboration features users expect | Provide clear guidance: DGs for email-only distribution, M365 groups for collaboration (email + files + Teams) |

### 5.2 Stale and Orphaned Groups

| Issue | Description | Impact | Resolution |
|-------|-------------|--------|------------|
| **No expiration for on-premises DGs** | AD DS groups have no native expiration mechanism | DGs persist for years after their purpose has ended, cluttering the GAL and receiving mail that nobody reads | Implement periodic reviews using scripts that identify groups with no recent mail activity (`Get-MessageTrace`) and flag them for owner review or deletion |
| **Ownerless groups** | The `managedBy` user has left the organisation and no replacement owner was assigned | No one can approve membership requests, respond to review campaigns or decide on group deletion | Enforce minimum two owners; detect ownerless groups via scheduled PowerShell scripts; use Entra ID ownerless group policy for M365 groups |
| **DGs with no members** | Groups emptied through organic attrition but never deleted | GAL pollution; users send to empty groups believing they are reaching a team | Detect and report via `Get-DistributionGroup | Where-Object { (Get-DistributionGroupMember $_).Count -eq 0 }` |

### 5.3 Security and Compliance Gaps

| Issue | Description | Impact | Resolution |
|-------|-------------|--------|------------|
| **MESG dual-purpose risk** | Adding users to an MESG for email distribution inadvertently grants access to SharePoint sites, file shares and applications secured by the same group | Unintended privilege escalation through email group management | Decouple security and distribution; audit MESGs and split into separate DG + security group where feasible |
| **DDG filter drift** | DDG filters reference attributes (department, office, title) that become inconsistent as HR data quality degrades | DDGs include wrong recipients or exclude intended ones; sensitive emails reach unintended audiences | Regular DDG filter review; align filters with HR-mastered attributes; preview membership regularly |
| **Unrestricted group send** | DGs default to allowing anyone (including external senders) to send to the group | Spam and phishing delivered to entire departments via open distribution groups | Set `RequireSenderAuthenticationEnabled: $true` on all DGs to restrict sending to authenticated (internal) users; create exceptions only where justified |
| **Hidden group membership** | Group membership is visible to all users by default, exposing organisational structure | Sensitive groups (legal, HR investigations, restructuring teams) reveal their members in the GAL | Use `HiddenGroupMembershipEnabled` for sensitive M365 groups; restrict member listing permissions on DGs |

### 5.4 Hybrid-Specific Issues

| Issue | Description | Impact | Resolution |
|-------|-------------|--------|------------|
| **Sync cycle delay** | Entra Connect default sync cycle is 30 minutes. Urgent group membership changes (e.g. removing a leaver) are not reflected in the cloud for up to 30 minutes | Former employee continues to receive email via DG membership for up to 30 minutes post-disablement | Use Entra Connect on-demand sync (`Start-ADSyncSyncCycle -PolicyType Delta`) for urgent changes |
| **Cross-premises mail routing loops** | Misconfigured hybrid mail flow causes messages sent to DGs to loop between on-premises and Exchange Online transport | Mail delivery failures; NDRs; transport queue congestion | Validate hybrid mail flow with the Microsoft Remote Connectivity Analyzer; ensure correct `targetAddress` and routing domain configuration |
| **Group writeback v2 migration** | Enabling group writeback v2 requires careful planning to avoid disrupting existing on-premises applications that use written-back groups | Applications referencing the old group object break when the group type changes during migration | Test in a staging environment; migrate groups incrementally; coordinate with application owners |
| **Decommission sequencing errors** | Removing on-premises Exchange before migrating DGs and contacts to cloud-native objects | Groups, contacts and mail-enabled objects permanently lost from Entra ID when their on-premises source is removed | Complete all group and contact migration to cloud-native objects before decommissioning on-premises Exchange; use Microsoft's hybrid decommissioning checklist |

---

## 6. Governance Recommendations by Environment

### 6.1 On-Premises Only

| Recommendation | Detail |
|----------------|--------|
| Implement a **group register** | Maintain a documented register of all DGs and MESGs with owner, purpose, creation date and review date |
| Schedule **quarterly group reviews** | Script-based reports identifying ownerless groups, empty groups, groups with no mail activity and groups with leavers as members |
| **Decouple MESGs** | Audit all MESGs and split into separate DGs and security groups where the dual purpose creates governance risk |
| Standardise **DDG filters** | Document all DDG filters in a central register; align filters to HR-mastered attributes; preview membership after HR data changes |
| Restrict **group creation** | Delegate group creation to specific admin roles; enforce naming conventions via administrative process |

### 6.2 Exchange Online Only

| Recommendation | Detail |
|----------------|--------|
| Restrict **M365 group creation** | Limit creation to a security group via Entra ID group creation policy; provide a request process for others |
| Enable **M365 group expiration** | Set 180 or 365-day expiration with owner renewal notifications |
| Deploy **Entra ID access reviews** | Review M365 group and DG membership quarterly; auto-remove unconfirmed members |
| Apply **sensitivity labels** to M365 groups | Enforce privacy, guest access and sharing controls based on classification |
| Migrate **static DGs to dynamic groups** | Where membership aligns to user attributes, replace manually managed DGs with Entra ID dynamic groups or Exchange Online DDGs |
| Monitor **DDGs separately** | DDGs are not visible in Entra ID governance tooling — establish separate Exchange Online PowerShell-based review processes |

### 6.3 Hybrid

| Recommendation | Detail |
|----------------|--------|
| **Document the source of authority** | Clearly document which objects are mastered on-premises and which are cloud-native; publish this for all administrators |
| **Tag synced objects** | Use the `DirSyncEnabled` attribute to identify synced objects in Exchange Online; build admin centre views and PowerShell scripts that filter by sync status |
| Enable **group writeback v2** | Write back M365 groups to AD DS for on-premises application integration and GAL visibility |
| Plan **DDG consolidation** | Identify on-premises DDGs that need cloud equivalents; create matching Exchange Online DDGs; plan to consolidate to a single DDG per use case once all mailboxes are migrated |
| Automate **sync-aware JML processes** | Offboarding scripts must remove the leaver from on-premises DGs (for synced groups) AND cloud-only groups, and trigger an on-demand sync for urgent changes |
| Establish **migration path to cloud-native groups** | Plan the transition from on-premises mastered DGs to cloud-native M365 groups or Entra ID security groups as part of the hybrid-to-cloud migration roadmap |

---

## 7. Summary: Identity Backing and Governance Matrix

| Object Type | AD DS Object | Entra ID Object | Can Sign In | Has Security Permissions | Syncs via Entra Connect | Entra ID Governance (Expiration / Access Review) | Exchange Governance (Transport Rules / DLP) |
|-------------|-------------|-----------------|-------------|------------------------|------------------------|------------------------------------------------|-------------------------------------------|
| **User Mailbox** | Yes (user) | Yes (user) | Yes | Yes | Yes | Yes | Yes |
| **Shared Mailbox** | Yes (disabled user) | Yes (disabled user) | No | Via delegates | Yes | Limited | Yes |
| **Room/Equipment Mailbox** | Yes (disabled user) | Yes (disabled user) | Optional | No | Yes | Limited | Yes |
| **Distribution Group** | Yes (group) | Yes (group) | No | No | Yes | Limited (no expiration) | Yes |
| **Mail-Enabled Security Group** | Yes (security group) | Yes (security group) | No | Yes | Yes | Limited (no expiration) | Yes |
| **Dynamic Distribution Group** | Yes (DDG object) | **No** | No | No | **No** | **None** | Yes |
| **Microsoft 365 Group** | Via writeback only | Yes (unified group) | No | Yes (members) | Reverse (cloud → AD) | Full (expiration + access review) | Yes |
| **Mail Contact** | Yes (contact) | Yes (contact) | No | No | Yes | **None** | Yes |
| **Mail User** | Yes (user) | Yes (user) | Yes | Yes | Yes | Yes | Yes |
