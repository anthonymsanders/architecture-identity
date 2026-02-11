# Canadian Policies, Regulations and Good Practice Guidance for Digital Identity

This document outlines key Canadian federal and provincial regulations, government directives and good practice guidance relevant to digital identity management. It covers the regulatory landscape, sector-specific identity requirements for the **education sector** and **critical infrastructure**, and practical compliance guidance.

---

## Part 1: Canadian Regulatory Landscape for Digital Identity

### 1.1 Personal Information Protection and Electronic Documents Act (PIPEDA)

**S.C. 2000, c. 5 — In force since 1 January 2004 (full application)**

PIPEDA is Canada's federal private-sector privacy law governing the collection, use and disclosure of personal information in the course of commercial activity. It applies to all federally regulated organisations and to provincial private-sector activities in provinces that have not enacted substantially similar legislation.

**Key Identity Requirements:**

| Principle | Description |
|-----------|-------------|
| **Accountability (Principle 1)** | Organisations must designate an individual accountable for compliance, including the protection of identity data. Accountability extends to personal information transferred to third parties for processing |
| **Identifying purposes (Principle 2)** | The purposes for collecting identity data (names, email addresses, employee IDs, biometric data) must be identified and documented before or at the time of collection |
| **Consent (Principle 3)** | Meaningful consent is required for the collection, use and disclosure of personal information. The form of consent (express or implied) must be appropriate to the sensitivity of the identity data |
| **Limiting collection (Principle 4)** | Identity systems must collect only the personal information necessary for the identified purposes. Over-collection of identity attributes is a violation |
| **Limiting use, disclosure and retention (Principle 5)** | Identity data must not be used or disclosed for purposes other than those identified, and must be retained only as long as necessary. Retention schedules must be defined and enforced |
| **Accuracy (Principle 6)** | Identity data must be accurate, complete and up-to-date to the extent necessary for the purposes for which it is used. This directly impacts identity directory data quality |
| **Safeguards (Principle 7)** | Personal information must be protected by security safeguards appropriate to the sensitivity of the information. This includes access controls, authentication, encryption and audit logging for identity systems |
| **Individual access (Principle 8)** | Individuals have the right to access their personal information held by the organisation, including identity records, and to challenge its accuracy |
| **Breach reporting** | Mandatory breach reporting to the Office of the Privacy Commissioner (OPC) and affected individuals when a breach of security safeguards involving personal information creates a real risk of significant harm |

**Enforcement:**
- The OPC can investigate complaints, conduct audits and make recommendations
- The Federal Court can order compliance and award damages
- Proposed successor legislation (see Section 1.2) would introduce administrative monetary penalties of up to **CAD 25 million or 5% of global gross revenue**

**Reference:** [PIPEDA — Office of the Privacy Commissioner of Canada](https://www.priv.gc.ca/en/privacy-topics/privacy-laws-in-canada/the-personal-information-protection-and-electronic-documents-act-pipeda/)

---

### 1.2 Federal Privacy Reform: Consumer Privacy Protection Act (CPPA)

**Status: Pending — Bill C-27 died on the Order Paper in January 2025**

The federal government's proposed Bill C-27, which included the Consumer Privacy Protection Act (CPPA), the Personal Information and Data Protection Tribunal Act and the Artificial Intelligence and Data Act (AIDA), died on the Order Paper when Parliament was prorogued in January 2025.

**Expected Developments (2025-2026):**

| Development | Description |
|-------------|-------------|
| **New privacy legislation** | The federal government has signalled that a new federal private-sector privacy statute and a companion bill establishing a penalty-based enforcement tribunal are expected to be introduced in late 2025 or early 2026 |
| **Enhanced penalties** | The new legislation is expected to include administrative monetary penalties of up to **CAD 25 million or 5% of gross global revenue**, whichever is greater |
| **Data portability** | Complementary amendments to PIPEDA are expected to establish an interoperable data mobility regime, giving individuals the right to request their personal data in a structured, machine-readable format |
| **AI regulation** | AIDA (the AI component of Bill C-27) will not return in its original form. Only parts may survive in a new framework |
| **Children's privacy** | The Minister has identified children's privacy and AI-generated deepfakes as priority areas for the new legislation |

**Identity Implications:**
- Organisations should prepare for significantly higher penalties for identity data breaches
- Data portability requirements will impact identity systems that must support structured data export
- Enhanced consent requirements may affect how identity data is collected and processed

**Reference:** [Osler — Canada's 2026 Privacy Priorities](https://www.osler.com/en/insights/reports/2025-legal-outlook/canadas-2026-privacy-priorities-data-sovereignty-open-banking-and-ai/)

---

### 1.3 Provincial Privacy Legislation

Three provinces have private-sector privacy laws deemed **substantially similar** to PIPEDA, meaning organisations operating within those provinces are generally exempt from PIPEDA for intra-provincial commercial activities:

| Province | Legislation | Key Identity Provisions |
|----------|-------------|------------------------|
| **Quebec** | *Act respecting the protection of personal information in the private sector* (as amended by **Law 25**) | Mandatory Privacy Impact Assessments (PIAs) before deploying identity systems that process personal information. Mandatory designation of a privacy officer. **Private right of action** allowing individuals to sue for privacy violations. Right to data portability (effective September 2024). Enhanced protections for **sensitive personal information** including biometric data. Automatic consent requirements for cross-border transfers of identity data |
| **Alberta** | *Personal Information Protection Act (PIPA)* | Applies to all private-sector organisations in Alberta. Requires consent for collection, use and disclosure of personal information. Mandatory breach notification to the Alberta Information and Privacy Commissioner when there is a real risk of significant harm |
| **British Columbia** | *Personal Information Protection Act (PIPA)* | Applies to private-sector organisations in BC. Requires knowledge and consent for personal information processing. Organisations must implement reasonable security arrangements to protect personal information |

**Public Sector Privacy Legislation:**

Each province and territory also has public-sector privacy legislation governing government institutions, including public educational institutions:

| Legislation | Jurisdiction | Education Relevance |
|-------------|-------------|-------------------|
| **Freedom of Information and Protection of Privacy Act (FIPPA)** | Ontario, BC, Manitoba | Governs public universities, colleges, school boards and other public educational institutions. Restricts storage and processing of personal information outside Canada (BC FIPPA) |
| **Freedom of Information and Protection of Privacy Act (FOIP)** | Alberta | Applies to public educational institutions in Alberta |
| **Act respecting Access to documents held by public bodies** | Quebec | Applies to public educational institutions in Quebec, as amended by Law 25 |
| **Privacy Act** | Federal | Applies to federal government institutions, including federal educational programmes |

**Reference:** [OPC — Provincial Laws](https://www.priv.gc.ca/en/privacy-topics/privacy-laws-in-canada/the-personal-information-protection-and-electronic-documents-act-pipeda/r_o_p/prov-pipeda/)

---

### 1.4 Treasury Board Directives on Identity and Security

The Treasury Board of Canada Secretariat (TBS) sets identity, security and digital policies for the federal government. These directives apply directly to federal departments and agencies and serve as a benchmark for broader Canadian public-sector identity practices.

#### 1.4.1 Directive on Identity Management

**Effective: 1 July 2009 (amended)**

The Directive on Identity Management establishes requirements for federal departments in the establishment, use and validation of identity.

**Key Requirements:**

| Requirement | Description |
|-------------|-------------|
| **Identity establishment** | Departments must establish identity by collecting and validating identity information from authoritative sources before granting access to programmes, services or facilities |
| **Identity assurance levels** | Identity assurance must be proportionate to the risk associated with the programme or service. The Standard on Identity and Credential Assurance defines four levels of assurance (LOA 1-4) |
| **Credential management** | Credentials issued to individuals must be managed throughout their lifecycle, including issuance, renewal, suspension and revocation |
| **Identity validation** | Departments must validate identity using methods appropriate to the assurance level required, including document verification, knowledge-based verification and biometric verification |
| **Interoperability** | Identity solutions must be interoperable across the Government of Canada to support seamless service delivery |

**Reference:** [TBS — Directive on Identity Management](https://www.tbs-sct.canada.ca/pol/doc-eng.aspx?id=16577)

#### 1.4.2 Policy on Government Security

**Effective: 1 July 2019 (amended 6 January 2025)**

The Policy on Government Security establishes the framework for protecting government information, assets, services and individuals, including identity-related security controls.

**Key Identity-related Requirements:**

| Requirement | Description |
|-------------|-------------|
| **Security screening** | All individuals requiring access to government information, assets or facilities must undergo security screening proportionate to the sensitivity of the access. The Directive on Security Screening (effective 6 January 2025) requires identity verification before any security screening activities |
| **Access control** | Departments must implement access controls for information, assets and facilities based on the need-to-know and least-privilege principles |
| **Identity-based physical security** | Physical access to government facilities must be controlled through identity verification and credential-based access systems |
| **IT security** | Departments must implement IT security measures including identity and access management, authentication and authorisation controls |

**Reference:** [TBS — Policy on Government Security](https://www.tbs-sct.canada.ca/pol/doc-eng.aspx?id=16578)

#### 1.4.3 Directive on Security Screening

**Effective: 6 January 2025**

The Directive on Security Screening replaced the Standard on Security Screening and establishes updated requirements for personnel security screening across the federal government.

**Key Identity Requirements:**

| Requirement | Description |
|-------------|-------------|
| **Identity verification first** | Departments must verify the identity of an individual before undertaking any subsequent security screening activities |
| **Evidence of identity** | Identity verification must be conducted in accordance with the Standard on Identity and Credential Assurance, using authoritative identity documents |
| **Security screening levels** | Screening levels (Reliability Status, Secret, Top Secret) determine the depth of background investigation, with identity verification as the foundation |
| **Continuous monitoring** | Security screening is not a one-time event; departments must have processes for periodic review and update of screening status |

**Reference:** [TBS — Directive on Security Screening](https://www.tbs-sct.canada.ca/pol/doc-eng.aspx?id=32805)

#### 1.4.4 Policy on Service and Digital

**Effective: 1 April 2020**

The Policy on Service and Digital guides digital transformation across the federal government, including the adoption of modern identity and authentication services.

**Key Identity Requirements:**

| Requirement | Description |
|-------------|-------------|
| **Digital identity first** | Departments must use digital identity as the foundation for online service delivery, supporting secure and seamless citizen access to government services |
| **Interoperable identity** | Identity solutions must be designed for interoperability across departments and with provincial/territorial governments |
| **Privacy by design** | Identity systems must embed privacy protections at the design stage, including data minimisation, purpose limitation and consent management |
| **Open standards** | Identity solutions should be based on open standards to promote interoperability and avoid vendor lock-in |

**Reference:** [TBS — Policy on Service and Digital](https://www.tbs-sct.canada.ca/pol/doc-eng.aspx?id=32603)

---

### 1.5 Pan-Canadian Trust Framework (PCTF)

The **Pan-Canadian Trust Framework (PCTF)** is an industry-developed, standards-based, technology-neutral framework managed by the **Digital Identity and Authentication Council of Canada (DIACC)**. It establishes the criteria for trusted digital identity in Canada.

**Key Components:**

| Component | Description |
|-----------|-------------|
| **Verified Person** | Requirements for establishing and verifying that a digital identity corresponds to a real, unique individual. Defines levels of assurance for identity proofing |
| **Verified Organization** | Requirements for establishing and verifying the identity of organisations, including business registration and authorised representatives |
| **Credentials** | Standards for issuing, managing and verifying digital credentials that attest to identity attributes (e.g. name, date of birth, qualifications, professional licences) |
| **Digital Integrity** | Requirements for ensuring the integrity, confidentiality and availability of digital identity information throughout its lifecycle |
| **Consent** | Standards for obtaining, recording and managing consent for the collection, use and disclosure of personal information within digital identity ecosystems |
| **Privacy by Design** | Architectural-level privacy protections including data minimisation, purpose limitation, selective disclosure and transparent consent management |
| **Federated Architecture** | Support for multiple credential issuers whose credentials are mutually recognised across the ecosystem |

**2025 Developments:**
- FCT Client ID Verification was certified against the PCTF Verified Person Component at Level of Assurance 2 (LOA2)
- The PCTF Legal Professionals Profile V1.1 was launched as the first industry-specific profile under the PCTF

**Reference:** [DIACC — Pan-Canadian Trust Framework](https://diacc.ca/trust-framework/)

---

### 1.6 Government of Canada Identity, Credential and Access Management (ICAM)

The Government of Canada operates a centralised **Identity, Credential and Access Management (ICAM)** programme that defines how identity is managed for both citizens accessing government services and government employees accessing internal systems.

**Key Components:**

| Component | Description |
|-----------|-------------|
| **GCKey** | A government-issued credential for citizens and businesses to access Government of Canada online services. Provides a username/password-based authentication mechanism |
| **Sign-In Partners** | Citizens can use their existing online banking credentials (via participating financial institutions) to authenticate to government services, leveraging the bank's identity verification processes |
| **Interoperability** | The ICAM framework promotes interoperability with provincial and territorial digital identity programmes, working towards a pan-Canadian digital identity ecosystem |
| **Trusted Digital Identity** | The Government of Canada's Trusted Digital Identity vision aims to provide citizens with a single, secure, interoperable digital identity that can be used across all levels of government and the private sector |

**Reference:** [Canada.ca — Identity, Credential and Access Management](https://www.canada.ca/en/government/system/digital-government/online-security-privacy/identity-credential-access-management.html)

---

### 1.7 Canadian Centre for Cyber Security (CCCS) Guidance

The Canadian Centre for Cyber Security (part of the Communications Security Establishment) provides cybersecurity guidance applicable to identity and access management across all sectors.

#### 1.7.1 ITSG-33: IT Security Risk Management

ITSG-33 is the Government of Canada's primary framework for IT security risk management. It contains a **Security Control Catalogue** structured into three classes:

| Class | Description | Identity-relevant Controls |
|-------|-------------|--------------------------|
| **Technical** | Controls implemented using technology | Access control (AC), identification and authentication (IA), audit and accountability (AU), system and communications protection (SC) |
| **Operational** | Controls implemented using human processes | Personnel security (PS), media protection (MP), physical and environmental protection (PE), configuration management (CM) |
| **Management** | Controls for managing IT security and risk | Security assessment and authorisation (CA), planning (PL), risk assessment (RA), programme management (PM) |

ITSG-33 defines **security control profiles** (pre-selected baseline control sets) for different risk environments, from low to very high sensitivity.

**Reference:** [CCCS — ITSG-33](https://www.cyber.gc.ca/en/guidance/it-security-risk-management-lifecycle-approach-itsg-33)

#### 1.7.2 ITSP.30.031 v3: User Authentication Guidance

ITSP.30.031 v3 provides guidance specifically on user authentication in IT systems.

**Key Guidance:**

| Topic | Description |
|-------|-------------|
| **Authentication methods** | Guidance on selecting authentication methods (passwords, tokens, biometrics, certificates) based on the sensitivity of the system and data |
| **Password guidance** | Annex B provides practical guidance for system designers, operators and end users on designing, implementing and using password-based authentication, including password complexity, rotation and storage |
| **Multi-factor authentication** | Recommends MFA for access to sensitive systems, privileged accounts and remote access |
| **Assurance levels** | Maps authentication mechanisms to assurance levels, helping organisations select appropriate authentication strength for their risk profile |

**Reference:** [CCCS — User Authentication Guidance ITSP.30.031 v3](https://www.cyber.gc.ca/en/guidance/user-authentication-guidance-information-technology-systems-itsp30031-v3)

#### 1.7.3 Baseline Cyber Security Controls for Small and Medium Organisations

The CCCS provides a set of **baseline cybersecurity controls** specifically designed for small and medium organisations, including identity-related controls:

| Control Area | Identity Guidance |
|-------------|-------------------|
| **Develop an incident response plan** | Include identity-related incidents (credential compromise, account takeover) in incident response procedures |
| **Automatically patch operating systems and applications** | Keep identity infrastructure (directory services, authentication systems) patched and up to date |
| **Enable security software** | Implement endpoint protection on systems that host identity services |
| **Configure devices securely** | Apply secure baseline configurations to identity infrastructure (domain controllers, federation servers, MFA systems) |
| **Use strong user authentication** | Enforce MFA for all users, especially for administrative and remote access. Use unique, complex passwords and consider password managers |
| **Back up and encrypt data** | Back up identity directories and configuration data. Encrypt identity data at rest and in transit |

**Reference:** [CCCS — Baseline Cyber Security Controls](https://www.cyber.gc.ca/en/guidance/baseline-cyber-security-controls-small-and-medium-organizations)

#### 1.7.4 Critical Infrastructure Cyber Security Goals

In October 2024, the CCCS released **Critical Infrastructure Cyber Security Goals (CRGs)** featuring six pillars and 36 goals:

| Pillar | Description | Identity Relevance |
|--------|-------------|-------------------|
| **Govern** | Establish cybersecurity governance, risk management and accountability | Define identity governance policies, roles and responsibilities |
| **Identify** | Identify and manage assets, risks and vulnerabilities | Inventory all identity assets (directories, accounts, credentials, certificates) |
| **Protect** | Implement protective controls | Access control, authentication, identity lifecycle management, privileged access management |
| **Detect** | Detect cybersecurity events | Monitor identity-related events (sign-ins, privilege escalation, anomalous access) |
| **Respond** | Respond to cybersecurity incidents | Identity-specific incident response (credential compromise, account takeover, directory breach) |
| **Recover** | Recover from cybersecurity incidents | Identity infrastructure recovery, credential reset, trust re-establishment |

**Reference:** [Canada.ca — Critical Infrastructure Cyber Security Goals](https://www.canada.ca/en/communications-security/news/2024/10/top-canadian-cyber-security-body-releases-flagship-guidance-for-critical-infrastructure.html)

---

### 1.8 Proceeds of Crime (Money Laundering) and Terrorist Financing Act (PCMLTFA) / FINTRAC

**S.C. 2000, c. 17 — Regulated by FINTRAC**

The PCMLTFA and associated regulations administered by the Financial Transactions and Reports Analysis Centre of Canada (FINTRAC) impose **identity verification obligations** on reporting entities in the financial sector.

**Key Identity Verification Requirements:**

| Requirement | Description |
|-------------|-------------|
| **Know Your Client (KYC)** | Reporting entities must verify the identity of clients before or during prescribed transactions and business relationships |
| **Verification methods** | Identity can be verified using government-issued photo identification, credit file, dual-process method (two independent reliable sources), or the digital identity authentication method |
| **Cash transactions** | Identity verification required for cash transactions of **CAD 10,000** or more |
| **Electronic funds transfers** | Identity verification required for international electronic funds transfers of **CAD 1,000** or more |
| **Ongoing monitoring** | Reporting entities must conduct ongoing monitoring of business relationships, including keeping client identification information up to date |
| **Record keeping** | Identity verification records must be retained for **at least five years** after the last business transaction or account closure |
| **Agent verification (October 2025)** | From 1 October 2025, businesses can use agents or mandataries to verify the identity of corporations and other entities |
| **Discrepancy reporting (October 2025)** | From October 2025, regulated entities must report material mismatches between internal records and government registries |

**Reference:** [FINTRAC — Methods to Verify Identity](https://fintrac-canafe.canada.ca/guidance-directives/client-clientele/guide11/11-eng)

---

## Part 2: Good Practice for the Education Sector

### 2.1 Regulatory Landscape for Canadian Education

Canadian educational institutions are subject to a layered privacy and security framework depending on their type and province:

| Institution Type | Applicable Privacy Law | Jurisdiction |
|-----------------|----------------------|-------------|
| **Public universities and colleges** | Provincial public-sector privacy law (FIPPA, FOIP, or equivalent) | Province where located |
| **Private universities and colleges** | PIPEDA or provincial private-sector privacy law (PIPA, Law 25) | Province where located |
| **Public school boards (K-12)** | Provincial public-sector privacy law (FIPPA, FOIP, or equivalent) | Province where located |
| **Private schools (K-12)** | PIPEDA or provincial private-sector privacy law | Province where located |
| **Federal educational programmes** | Privacy Act | Federal |

### 2.2 Key Identity Challenges in Canadian Education

| Challenge | Description |
|-----------|-------------|
| **Large, transient populations** | Universities manage tens of thousands of students, faculty, researchers and staff with high annual turnover as students enrol, graduate and transfer between institutions |
| **Cross-provincial data flows** | Students transferring between provinces trigger different privacy laws. Data that is lawful to process in Ontario under FIPPA may have different requirements under Quebec's Law 25 |
| **Cross-border data restrictions (BC FIPPA)** | BC's FIPPA restricts the storage and processing of personal information outside Canada for public institutions. This directly impacts cloud-based identity systems (e.g. Entra ID tenants hosted outside Canada) |
| **Research data sensitivity** | Research involving personal information, health data or Indigenous community data requires ethics board approval and strict access controls |
| **EdTech vendor proliferation** | Widespread adoption of third-party learning management systems, collaboration tools and assessment platforms creates identity sprawl across unmanaged SaaS providers |
| **Federated identity (CAF / eduGAIN)** | Canadian academic institutions participate in the **Canadian Access Federation (CAF)** operated by CANARIE and connect to the global **eduGAIN** inter-federation for cross-institutional access |
| **Minor students** | K-12 schools and dual-enrolment programmes must implement enhanced protections for children's personal information, including parental consent mechanisms |
| **Indigenous data sovereignty** | Research and programmes involving Indigenous communities must respect the **OCAP principles** (Ownership, Control, Access, Possession) and First Nations, Inuit and Metis data governance frameworks |

### 2.3 Good Practice Guidance for Education Identity

#### 2.3.1 Canadian Access Federation (CAF)

The **Canadian Access Federation (CAF)**, operated by **CANARIE**, provides the federated identity infrastructure for Canadian research and education institutions.

**Good Practice for Education Identity Federation:**

| Practice | Description |
|----------|-------------|
| **SAML 2.0 federation** | Institutions should operate a SAML 2.0 Identity Provider (IdP) registered with CAF and connected to the global eduGAIN inter-federation for cross-institutional and international access |
| **Attribute release policies** | Define and publish clear attribute release policies specifying what identity attributes (eduPersonPrincipalName, mail, affiliation, entitlement) are shared with which service providers |
| **eduPerson schema** | Use the **eduPerson** LDAP object class and attribute schema as the standard for representing identity attributes in the education sector |
| **Sirtfi compliance** | Participate in the **Security Incident Response Trust Framework for Federated Identity (Sirtfi)** to establish trust for security incident response across federated institutions |
| **REFEDS Assurance Framework** | Implement the REFEDS Assurance Framework to communicate the level of identity assurance to federated service providers |
| **Research and Scholarship entity category** | Adopt the R&S entity category to streamline attribute release for legitimate research and scholarship service providers |

**Reference:** [CANARIE — Canadian Access Federation](https://www.canarie.ca/identity/)

#### 2.3.2 Privacy Compliance in Education

| Good Practice | Description |
|---------------|-------------|
| **Conduct Privacy Impact Assessments (PIAs)** | Required under Quebec Law 25 and recommended across all provinces before deploying new identity systems, biometric authentication or large-scale monitoring (e.g. exam proctoring). BC public institutions must complete PIAs under FIPPA |
| **Designate a privacy officer** | Mandatory under Quebec Law 25. Recommended for all educational institutions to oversee identity data processing compliance |
| **Maintain a personal information inventory** | Document all identity data processing activities, including directory services, authentication logs, learning management systems, student records and research data repositories |
| **EdTech vendor due diligence** | Assess third-party EdTech platforms for privacy compliance before integration. Ensure data processing agreements are in place. Verify data residency (critical for BC FIPPA compliance) |
| **Enforce data residency where required** | BC public institutions must ensure personal information is stored and accessed within Canada. This impacts cloud identity provider selection and configuration |
| **Define lawful authority for collection** | Public institutions typically rely on statutory authority for collecting student identity data. Consent should be used where statutory authority does not apply |
| **Implement retention schedules** | Define retention periods for student identity data, authentication logs and access records aligned with provincial records management requirements. Delete or anonymise data when retention periods expire |
| **Support student access rights** | Implement processes for students to exercise their rights to access, correct and (where applicable) request deletion of their personal information |

#### 2.3.3 Authentication Good Practice for Education

| Good Practice | Description |
|---------------|-------------|
| **Enforce MFA for all users** | Both PIPEDA and provincial privacy laws effectively require strong authentication to protect personal data. MFA should be enforced for all students, faculty and staff, particularly for access to personal information and administrative systems |
| **Federate authentication** | Use the institutional IdP (federated via CAF) as the primary authentication point for all integrated applications, reducing credential sprawl |
| **Support passwordless authentication** | Offer FIDO2 security keys, passkeys and institutional smartcards as passwordless authentication options, particularly for staff and researchers with access to sensitive data |
| **Implement risk-based authentication** | Apply adaptive authentication policies that step up requirements (e.g. require MFA, block access) based on contextual signals such as location, device, time and sign-in risk |
| **Manage privileged access** | IT administrators, system administrators and researchers with elevated access should be subject to privileged access management controls including just-in-time access, session recording and regular access reviews |

#### 2.3.4 OCAP Principles and Indigenous Data Sovereignty

When educational institutions manage identity data related to Indigenous peoples or communities, the following principles apply:

| Principle | Description |
|-----------|-------------|
| **Ownership** | Indigenous communities own their collective and individual data. Institutions must respect that Indigenous peoples have the right to own information about their communities |
| **Control** | Indigenous peoples must have control over all aspects of data management, including collection, use, disclosure and destruction |
| **Access** | Indigenous peoples must have access to data about themselves and their communities, regardless of where it is held |
| **Possession** | Data should be physically held by Indigenous communities or by institutions with explicit community consent and governance agreements |

These principles impact how identity systems are designed for programmes serving Indigenous students and communities, including consent processes, data storage locations and governance structures.

---

## Part 3: Good Practice for Critical Infrastructure

### 3.1 Canadian Critical Infrastructure Sectors

Public Safety Canada identifies **ten critical infrastructure sectors**:

| Sector | Examples |
|--------|----------|
| **Energy and utilities** | Electricity generation and distribution, oil and gas, pipelines, nuclear |
| **Finance** | Banks, credit unions, securities, insurance, payment systems |
| **Food** | Agriculture, food processing, distribution |
| **Government** | Federal, provincial and municipal government operations |
| **Health** | Hospitals, public health, pharmaceuticals, medical devices |
| **Information and communication technology** | Telecommunications, internet, broadcasting, satellite |
| **Manufacturing** | Defence industrial base, chemicals, advanced manufacturing |
| **Safety** | Law enforcement, fire services, emergency management, search and rescue |
| **Transportation** | Aviation, maritime, rail, road, transit |
| **Water** | Drinking water, wastewater, dams |

**Reference:** [Public Safety Canada — Critical Infrastructure](https://www.publicsafety.gc.ca/cnt/ntnl-scrt/crtcl-nfrstrctr/index-en.aspx)

### 3.2 Key Identity Challenges in Canadian Critical Infrastructure

| Challenge | Description |
|-----------|-------------|
| **Operational Technology (OT) identity** | Industrial control systems (ICS), SCADA and OT environments often use shared credentials, hardcoded passwords and legacy protocols without modern authentication |
| **Geographic dispersion** | Canadian critical infrastructure spans vast geography, including remote and northern locations with limited connectivity, complicating cloud-based identity services |
| **High-availability requirements** | Identity systems must be designed for extreme availability; authentication failures can prevent access to safety-critical systems |
| **IT/OT convergence** | As IT and OT networks converge, organisations must manage identities across both domains while maintaining segmentation |
| **Federal-provincial regulatory complexity** | Critical infrastructure operators may be subject to federal and provincial regulations simultaneously, with varying identity and security requirements |
| **Supply chain dependencies** | Heavy reliance on US and international vendors and service providers for critical infrastructure components, requiring secure third-party identity management |
| **Personnel vetting** | Federal government contractors and critical infrastructure personnel may require security screening under the Directive on Security Screening |
| **Nation-state threat actors** | Canada's critical infrastructure faces sophisticated adversaries, as documented in the CCCS National Cyber Threat Assessments |

### 3.3 CCCS-Aligned Identity Good Practice for Critical Infrastructure

#### 3.3.1 Access Control and Authentication (Protect Pillar)

| Good Practice | Description |
|---------------|-------------|
| **Implement centralised identity management** | Deploy a centralised identity provider as the authoritative directory for all IT user identities. Maintain separate, dedicated identity infrastructure for OT environments where air-gapping or network isolation is required |
| **Enforce multi-factor authentication** | MFA must be enforced for all remote access, privileged access, VPN connections and access to critical systems. Use phishing-resistant methods (FIDO2, smartcards, certificate-based authentication) aligned with ITSP.30.031 v3 guidance |
| **Implement least-privilege access** | All user accounts must operate with minimum required permissions. Privileged access must be just-in-time, time-bound and subject to approval workflows |
| **Separate IT and OT identities** | Maintain separate identity stores for IT and OT environments. Where cross-domain access is necessary, implement jump servers with dedicated privileged accounts, session recording and time-limited access |
| **Apply ITSG-33 security controls** | Implement the identification and authentication (IA) and access control (AC) security control families from ITSG-33 at the profile level appropriate to the organisation's risk classification |
| **Manage service and machine identities** | Inventory all service accounts, API keys and machine identities. Implement automated credential rotation. Remove hardcoded credentials from OT systems where feasible |

#### 3.3.2 Identity Lifecycle Management

| Good Practice | Description |
|---------------|-------------|
| **Automate joiner/mover/leaver processes** | Connect identity management to HR systems to automate account creation, role changes and deprovisioning. Access rights must be promptly adjusted when personnel roles change |
| **Conduct regular access reviews** | Perform access certification at least quarterly for privileged access and annually for standard access |
| **Manage contractor and vendor identities** | All third-party identities must have designated sponsors, time-limited access, mandatory MFA and regular access reviews |
| **Personnel security screening** | For federal government contractors and designated critical infrastructure roles, ensure security screening is conducted in accordance with the TBS Directive on Security Screening, with identity verification as the first step |
| **Implement identity-aware offboarding** | When personnel depart, revoke all access across all systems within a defined SLA. Implement session termination mechanisms to end active sessions immediately upon account disablement |

#### 3.3.3 Privileged Access Management

| Good Practice | Description |
|---------------|-------------|
| **Deploy a PAM solution** | Implement dedicated Privileged Access Management to vault, rotate and audit all privileged credentials across IT and OT environments |
| **Implement just-in-time access** | Privileged access should be on-demand with time limits, justification, approval and automatic revocation |
| **Record privileged sessions** | All privileged sessions, especially on critical infrastructure systems, should be recorded for forensic analysis |
| **Protect Tier 0 identity infrastructure** | Domain controllers, identity providers, federation services and certificate authorities are Tier 0 assets requiring the highest level of protection |
| **Monitor for identity-based attacks** | Monitor for credential theft, privilege escalation, directory attacks and federation abuse using SIEM integration and identity threat detection |

#### 3.3.4 Incident Response and Reporting

| Good Practice | Description |
|---------------|-------------|
| **Include identity in incident response plans** | Plans must address credential compromise, directory breach, federation trust abuse, mass account lockout and identity infrastructure failure |
| **Report to CCCS** | The CCCS encourages critical infrastructure operators to report cybersecurity incidents, including identity-related breaches. While mandatory reporting is not yet universally legislated at the federal level, sector-specific regulations (e.g. OSFI for financial institutions) may require it |
| **Maintain forensic evidence** | Preserve authentication logs, directory change logs and privileged session recordings to support incident investigation |
| **Coordinate with Public Safety Canada** | Critical infrastructure operators should participate in sector-specific information sharing through Public Safety Canada's critical infrastructure programmes |

### 3.4 Financial Sector Identity Requirements (OSFI)

The **Office of the Superintendent of Financial Institutions (OSFI)** regulates federally regulated financial institutions (banks, insurers, trust companies) and imposes specific technology and cybersecurity requirements.

**OSFI Guideline B-13: Technology and Cyber Risk Management (Effective 1 January 2024)**

| Requirement | Description |
|-------------|-------------|
| **Identity and access management** | Financial institutions must implement robust IAM controls including role-based access, least-privilege, MFA and regular access reviews |
| **Privileged access** | Privileged accounts must be subject to enhanced controls including separate credentials, MFA, session monitoring and regular recertification |
| **Third-party access** | Identity and access controls for third-party service providers must be contractually defined and regularly assessed |
| **Authentication** | Strong authentication, including MFA, must be implemented for all access to systems processing financial data |
| **Incident management** | Technology incidents, including identity compromise, must be reported to OSFI in accordance with the Technology and Cyber Security Incident Reporting advisory |

**FINTRAC identity verification requirements** (Section 1.8 above) also apply to all financial sector reporting entities.

---

## Part 4: Regulatory Cross-Reference Matrix

### 4.1 Identity Requirements by Regulation

| Identity Requirement | PIPEDA | Quebec Law 25 | BC FIPPA | TBS Directives | ITSG-33 | PCMLTFA / FINTRAC | OSFI B-13 |
|---------------------|--------|---------------|----------|-----------------|---------|-------------------|-----------|
| Access control policies | Safeguards principle | Required | Required | Required | AC family | | Required |
| Multi-factor authentication | Implied (safeguards) | Implied (safeguards) | Implied (safeguards) | Required (ITSP.30.031) | IA family | | Required |
| Identity lifecycle management | Retention principle | Required | Required | Required (Directive on Identity Management) | PS family | | Required |
| Privileged access management | Implied (safeguards) | Implied (safeguards) | Implied (safeguards) | Required | AC family | | Required (enhanced) |
| Access reviews / certification | | | | Required | CA family | | Required |
| Identity verification | | | | Required (LOA 1-4) | IA family | Required (KYC) | Required |
| Security screening / vetting | | | | Required (Directive on Security Screening) | PS family | | |
| Privacy Impact Assessment | Recommended | **Mandatory** | Required | Required | RA family | | |
| Data residency | | Cross-border consent | **Canada only** | | | | |
| Breach notification | **Mandatory** | **Mandatory** | Required | Required | | | Required (to OSFI) |
| Data portability | Proposed (CPPA) | **Right enacted** | | | | | |
| Record retention | Required | Required | Required | Required | | **5 years minimum** | Required |

### 4.2 Sector Applicability

| Regulation | Education (Public) | Education (Private) | Energy | Finance | Health | Government | Telecommunications |
|------------|-------------------|--------------------|---------|---------|---------|-----------|--------------------|
| **PIPEDA** | No (provincial law applies) | Yes (unless provincial law) | Yes | Yes | Yes | No (Privacy Act) | Yes |
| **Quebec Law 25** | Yes (QC public) | Yes (QC private) | Yes (QC) | Yes (QC) | Yes (QC) | Yes (QC public) | Yes (QC) |
| **BC FIPPA** | Yes (BC public) | No | No | No | Yes (BC public) | Yes (BC public) | No |
| **Ontario FIPPA** | Yes (ON public) | No | No | No | No | Yes (ON public) | No |
| **TBS Directives** | No (unless federal) | No | No | No | No | Yes (federal) | No |
| **ITSG-33** | Recommended | Recommended | Recommended | Recommended | Recommended | **Required (federal)** | Recommended |
| **PCMLTFA / FINTRAC** | No | No | No | **Yes** | No | No | No (unless MSB) |
| **OSFI B-13** | No | No | No | **Yes (FRFI)** | No | No | No |
| **PCTF** | Voluntary | Voluntary | Voluntary | Voluntary | Voluntary | Aligned | Voluntary |

---

## Part 5: Summary of Key Compliance Dates and Milestones

| Date | Regulation / Event | Milestone |
|------|-------------------|-----------|
| **1 January 2004** | PIPEDA | Full application across Canada |
| **1 November 2018** | PIPEDA (DORS/2018-64) | Mandatory breach reporting and record-keeping in force |
| **22 September 2023** | Quebec Law 25 (Phase 2) | Privacy officer designation, PIAs, consent and transparency requirements |
| **22 September 2024** | Quebec Law 25 (Phase 3) | Right to data portability in force |
| **1 January 2024** | OSFI Guideline B-13 | Technology and Cyber Risk Management requirements effective |
| **6 January 2025** | TBS Directive on Security Screening | New security screening and identity verification requirements effective |
| **1 October 2025** | FINTRAC | Agent/mandatary entity verification and discrepancy reporting requirements |
| **Late 2025 / Early 2026** | Federal privacy reform | New federal privacy legislation expected to be introduced (successor to Bill C-27/CPPA) |
| **2025** | PCTF | First industry-specific profile (Legal Professionals) certified |
| **Ongoing** | CCCS CRGs | Critical infrastructure organisations implementing 36 cyber security goals across six pillars |
