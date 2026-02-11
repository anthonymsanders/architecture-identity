# Common Identity Challenges by Organisation Size

## Common User Types in a Business

Before exploring identity challenges, it is important to understand the typical user types that organisations must manage. These user types exist across organisations of all sizes, though the volume and complexity varies.

### Internal Users

| User Type | Description |
|-----------|-------------|
| **Permanent Employees** | Full-time and part-time staff on payroll with standard employment contracts |
| **Contractors / Temporary Workers** | Fixed-term or agency staff engaged for specific projects or periods |
| **Executives / Senior Leadership** | C-suite and board members, often requiring elevated access and additional security controls |
| **IT Administrators** | Technical staff with privileged access to systems, infrastructure and identity platforms |
| **Developers** | Engineering staff requiring access to source code, CI/CD pipelines, cloud environments and APIs |
| **Service Accounts** | Non-human identities used by applications, scripts and automated processes |

### External Users

| User Type | Description |
|-----------|-------------|
| **Customers (B2C)** | End consumers accessing customer-facing applications and services |
| **Business Partners (B2B)** | Suppliers, vendors and partners requiring access to shared platforms or data |
| **Guest Users** | Visitors, interviewees or short-term collaborators needing temporary, limited access |
| **Regulatory / Auditors** | External auditors or regulators requiring read-only or time-bound access for compliance reviews |
| **Managed Service Providers (MSPs)** | Third-party IT providers managing infrastructure or applications on behalf of the organisation |

### Emerging Identity Types

| User Type | Description |
|-----------|-------------|
| **Machine / Workload Identities** | Cloud workloads, containers, microservices and IoT devices that authenticate without human interaction |
| **API Consumers** | External or internal systems consuming APIs that require authentication and authorisation |
| **Bots / RPA Identities** | Robotic process automation accounts performing repetitive tasks across systems |

---

## Small Organisations (1 - 50 employees)

### Typical Characteristics
- Limited or no dedicated IT staff
- Heavy reliance on SaaS and cloud-native tools
- Informal processes and ad-hoc access management
- Flat organisational structure

### Common Identity Challenges

| Challenge | Description |
|-----------|-------------|
| **No centralised identity provider** | User accounts are created independently in each SaaS application, leading to identity sprawl and inconsistent credentials |
| **Shared accounts** | Teams share logins for cost reasons or convenience, making it impossible to attribute actions to individuals |
| **Weak or no MFA adoption** | Multi-factor authentication is often not enforced, leaving accounts vulnerable to credential attacks |
| **No joiners/movers/leavers process** | When staff leave, accounts are often not disabled promptly, creating orphaned accounts and security risk |
| **Password reuse** | Without a password manager or SSO, users reuse passwords across business and personal accounts |
| **Shadow IT** | Staff adopt tools and services without IT oversight, creating unmanaged identities outside organisational control |
| **No access reviews** | There is no formal process to review who has access to what, leading to privilege creep over time |
| **Owner dependency** | The business owner or a single individual holds all administrative credentials, creating a single point of failure |

---

## Medium Organisations (50 - 500 employees)

### Typical Characteristics
- Dedicated IT team but limited identity-specific expertise
- Mix of on-premises and cloud systems
- Growing regulatory and compliance obligations
- Multiple departments with different access needs

### Common Identity Challenges

| Challenge | Description |
|-----------|-------------|
| **Identity silos** | Different departments or business units manage their own user directories, leading to fragmented identity data and duplicate accounts |
| **Inconsistent provisioning** | No standardised process for granting access; some teams use tickets, others use email requests, resulting in delays and errors |
| **Privilege creep** | As employees change roles, old access is rarely revoked while new access is added, accumulating excessive permissions |
| **Contractor and third-party access** | Growing use of external contractors and partners without a clear identity lifecycle, leading to unmanaged external accounts |
| **Compliance gaps** | Regulations such as GDPR, ISO 27001 or Cyber Essentials require demonstrable access controls that informal processes cannot satisfy |
| **Lack of SSO across all applications** | SSO may be implemented for some applications but not others, leaving gaps where users must manage separate credentials |
| **Weak privileged access management** | Admin and service accounts are not adequately secured, monitored or rotated |
| **Incomplete offboarding** | Leavers retain access to some systems because there is no single source of truth for all user entitlements |
| **Limited visibility and reporting** | No consolidated view of who has access to what, making audits time-consuming and unreliable |
| **Growing machine identity footprint** | Increasing use of APIs, automation and cloud services creates non-human identities that are not governed |

---

## Large Organisations (500+ employees)

### Typical Characteristics
- Dedicated Identity and Access Management (IAM) team or function
- Complex hybrid and multi-cloud environments
- Multiple business units, geographies and regulatory jurisdictions
- Mergers, acquisitions and divestitures creating identity integration challenges

### Common Identity Challenges

| Challenge | Description |
|-----------|-------------|
| **Identity governance at scale** | Managing hundreds of thousands of identities across multiple systems, directories and clouds requires mature tooling and processes |
| **Merger and acquisition integration** | Each acquired entity brings its own identity infrastructure, directories and policies that must be rationalised and integrated |
| **Regulatory complexity** | Operating across jurisdictions means complying with multiple overlapping regulations (GDPR, HIPAA, SOX, NIS2) simultaneously |
| **Orphaned and dormant accounts** | The sheer volume of accounts makes it difficult to identify and remove accounts that are no longer needed |
| **Excessive standing privileges** | Users and service accounts retain persistent elevated access instead of using just-in-time or time-bound privilege escalation |
| **Decentralised application ownership** | Business units own applications independently, resulting in inconsistent identity integration and access policies |
| **Machine identity explosion** | Thousands of service accounts, API keys, certificates and workload identities that are poorly tracked and rarely rotated |
| **Complex access certification** | Periodic access reviews become unmanageable when reviewers must certify thousands of entitlements they do not fully understand |
| **Identity as an attack surface** | Sophisticated threat actors target identity infrastructure (Active Directory, Entra ID, federation services) as a primary attack vector |
| **Legacy system integration** | Older systems may not support modern authentication protocols (SAML, OIDC, SCIM), requiring custom connectors or exceptions |
| **Zero Trust adoption** | Transitioning from perimeter-based security to identity-centric Zero Trust architecture is a multi-year transformation |
| **Cross-domain identity federation** | Establishing and maintaining trust relationships with partners, suppliers and customers across different identity domains |
| **Identity data quality** | Inconsistent, incomplete or outdated identity attributes across HR, IT and business systems undermine automation and policy enforcement |
| **Separation of duties conflicts** | Ensuring that no single identity holds conflicting permissions (e.g. creating and approving payments) becomes increasingly difficult at scale |

---

## Summary: Challenge Progression by Organisation Size

| Dimension | Small | Medium | Large |
|-----------|-------|--------|-------|
| **Identity store** | Scattered SaaS accounts | Partial directory consolidation | Multiple directories, hybrid/multi-cloud |
| **Provisioning** | Manual / ad-hoc | Semi-automated with gaps | Automated with IGA tooling |
| **MFA** | Often absent | Partially enforced | Enforced with adaptive policies |
| **Privileged access** | Owner-managed | Basic PAM controls | Enterprise PAM with JIT access |
| **Compliance** | Minimal awareness | Emerging requirements | Multi-regulation governance |
| **Machine identities** | Few, unmanaged | Growing, partially tracked | Thousands, requiring dedicated management |
| **Biggest risk** | Account takeover via weak credentials | Privilege creep and compliance gaps | Identity infrastructure compromise at scale |
