# EU Policies, Regulations and Good Practice Guidance for Digital Identity

This document outlines key EU regulations, directives and good practice guidance relevant to digital identity management in the **education sector** and **critical national infrastructure (CNI)**. It covers the regulatory landscape, sector-specific identity requirements and practical compliance guidance.

---

## Part 1: EU Regulatory Landscape for Digital Identity

### 1.1 General Data Protection Regulation (GDPR)

**Regulation (EU) 2016/679 — In force since 25 May 2018**

The GDPR is the foundational regulation governing the processing of personal data across all EU member states, including identity data.

**Key Identity Requirements:**

| Requirement | Description |
|-------------|-------------|
| **Lawful basis for processing** | Organisations must establish a lawful basis (consent, contract, legal obligation, legitimate interest, etc.) for processing identity data such as names, email addresses, employee IDs and biometric data |
| **Data minimisation** | Identity systems must only collect and store the minimum personal data necessary for the stated purpose |
| **Purpose limitation** | Identity data collected for one purpose (e.g. access control) must not be repurposed without further lawful basis |
| **Right of access (Article 15)** | Individuals have the right to request a copy of all personal data held about them, including identity and access records |
| **Right to erasure (Article 17)** | Individuals can request deletion of their identity data when it is no longer necessary, subject to legal retention obligations |
| **Right to data portability (Article 20)** | Identity data provided by the individual must be exportable in a structured, machine-readable format |
| **Data Protection Impact Assessment (Article 35)** | Required for identity processing that is likely to result in high risk, including large-scale profiling, biometric processing and systematic monitoring |
| **Data breach notification (Articles 33-34)** | Identity-related breaches (e.g. credential compromise, directory exfiltration) must be reported to the supervisory authority within 72 hours |
| **Data Protection Officer (Article 37)** | Mandatory for public authorities (including educational institutions) and organisations carrying out large-scale systematic monitoring |

**Relevance to Identity Systems:**
- Identity directories (Active Directory, LDAP, Entra ID) containing employee or student personal data are subject to GDPR
- Audit logs containing sign-in data, IP addresses and device information are personal data under GDPR
- Cross-border identity synchronisation (e.g. multi-country Entra ID tenants) triggers GDPR transfer rules

**Reference:** [European Commission — Data Protection](https://commission.europa.eu/law/law-topic/data-protection_en)

---

### 1.2 eIDAS 2.0 — European Digital Identity Framework

**Regulation (EU) 2024/1183 — Entered into force 20 May 2024**

eIDAS 2.0 is a major revision of the original eIDAS regulation, establishing a harmonised framework for digital identity across the EU and mandating the European Digital Identity (EUDI) Wallet.

**Key Provisions:**

| Provision | Description |
|-----------|-------------|
| **EUDI Wallet** | By 2026, every EU member state must provide at least one European Digital Identity Wallet to citizens and businesses. The wallet is a secure mobile application for storing, managing and presenting digital credentials (ID documents, diplomas, professional certificates, health credentials) |
| **Mandatory acceptance** | Very large online platforms and specified services must accept the EUDI Wallet for strong authentication when users choose it. By 21 November 2027, businesses requiring customer identification must accept the wallet |
| **Qualified trust services** | Updated rules for electronic signatures, seals, timestamps, registered delivery and website authentication certificates |
| **Cross-border recognition** | Member states must mutually recognise digital identities issued by other member states, enabling seamless cross-border authentication |
| **User control and privacy** | Citizens control what data they share and with whom. The wallet supports selective disclosure — sharing only the minimum attributes required (e.g. proving age without revealing date of birth) |
| **Electronic attestation of attributes** | Enables verified digital credentials for education (diplomas, qualifications), professional licences and health records to be stored in the wallet |

**Relevance to Identity Systems:**
- Organisations, universities and public services will need to integrate EUDI Wallet acceptance into their authentication flows
- Identity providers must support interoperability with the EUDI Wallet's trust framework
- Education providers will be able to issue verifiable digital diplomas and qualifications via the wallet

**Key Deadlines:**
- **2026:** Member states must offer at least one EUDI Wallet
- **2027 (November):** Businesses requiring identity verification must accept the wallet

**Reference:** [eIDAS 2.0 Regulation](https://www.european-digital-identity-regulation.com/)

---

### 1.3 NIS2 Directive — Network and Information Security

**Directive (EU) 2022/2555 — Transposition deadline: 17 October 2024**

The NIS2 Directive is the EU's primary cybersecurity legislation for essential and important entities across 18 critical sectors. It significantly expands the scope of the original NIS Directive and introduces stricter security and reporting obligations.

**Sectors in Scope:**

| Essential Entities (Annex I) | Important Entities (Annex II) |
|------------------------------|-------------------------------|
| Energy (electricity, oil, gas, hydrogen, district heating) | Postal and courier services |
| Transport (air, rail, water, road) | Waste management |
| Banking and financial market infrastructure | Chemicals manufacture and distribution |
| Health (hospitals, laboratories, medical devices, pharma) | Food production, processing and distribution |
| Drinking water supply and distribution | Manufacturing (medical devices, electronics, machinery, motor vehicles) |
| Wastewater management | Digital providers (online marketplaces, search engines, social networks) |
| Digital infrastructure (DNS, TLD, cloud, data centres, CDNs, trust services) | **Research organisations** (including universities and higher education) |
| ICT service management (B2B) | |
| Public administration (central and regional government) | |
| Space | |

**Key Identity and Access Management Requirements:**

| Requirement | Description |
|-------------|-------------|
| **Access control policies (Article 21)** | Entities must implement appropriate access control policies and procedures, including role-based access, least-privilege principles and regular access reviews |
| **Multi-factor authentication** | MFA or continuous authentication must be used where appropriate, particularly for privileged access and remote access |
| **Identity and access management** | Entities must manage the identity lifecycle (joiners, movers, leavers) and maintain an inventory of all user accounts, including privileged and service accounts |
| **Supply chain security** | Identity-related risks in the supply chain (third-party access, managed service providers) must be assessed and managed |
| **Incident reporting** | Significant cybersecurity incidents (including identity-related breaches such as credential compromise or directory attacks) must be reported within 24 hours (early warning) and 72 hours (full notification) |
| **Management accountability** | Senior management is personally accountable for cybersecurity compliance, including identity security. Governance failures may result in temporary bans or disqualification |
| **Risk management measures** | Entities must adopt technical and organisational measures proportionate to their risk, including encryption, network segmentation, vulnerability management and identity security controls |

**Enforcement:**
- Fines of up to **EUR 10 million** or **2% of global annual turnover** (whichever is higher) for essential entities
- Fines of up to **EUR 7 million** or **1.4% of global annual turnover** for important entities

**Transposition Status (as of early 2026):**
Member state transposition has been staggered. Several key jurisdictions (including Germany, France, Ireland, Spain) completed transposition in 2025, while some remain in progress. Only 14 member states had completed transposition by mid-2025.

**Reference:** [EU NIS2 Directive](https://digital-strategy.ec.europa.eu/en/policies/nis2-directive)

---

### 1.4 Critical Entities Resilience (CER) Directive

**Directive (EU) 2022/2557 — Applying from 18 October 2024**

The CER Directive complements NIS2 by addressing the **physical and operational resilience** of critical entities, beyond just cybersecurity.

**Sectors in Scope:** Energy, transport, banking, financial market infrastructure, health, drinking water, wastewater, digital infrastructure, public administration, space, food.

**Key Identity Requirements:**

| Requirement | Description |
|-------------|-------------|
| **Background checks** | Critical entities must carry out background checks for persons holding sensitive roles, subject to conditions stipulated by each member state. This includes identity verification, criminal record checks and security vetting |
| **Personnel security** | Entities must establish policies for managing access by personnel to sensitive areas, systems and information, including identity-based access controls |
| **Physical access control** | Identity-based physical access systems (badge readers, biometric scanners) must be implemented for critical facilities |
| **Resilience plans** | Critical entities must maintain resilience plans that include identity and access management as part of business continuity and incident response |

**Key Deadlines:**
- **17 January 2026:** Member states must adopt a national strategy and complete risk assessments
- **17 July 2026:** Member states must identify critical entities

**Reference:** [EU Critical Entities Resilience Directive](https://home-affairs.ec.europa.eu/policies/internal-security/counter-terrorism-and-radicalisation/protection/critical-infrastructure-resilience-eu-level_en)

---

### 1.5 Digital Operational Resilience Act (DORA)

**Regulation (EU) 2022/2554 — Applying from 17 January 2025**

DORA is a sector-specific regulation for the **financial sector**, establishing uniform requirements for the security of network and information systems of financial entities and their critical ICT service providers.

**Entities in Scope:** Banks, insurers, reinsurers, investment firms, payment institutions, electronic money institutions, crypto-asset service providers, credit rating agencies, central counterparties and their ICT third-party service providers.

**Key Identity and Access Management Requirements:**

| Requirement | Description |
|-------------|-------------|
| **ICT risk management framework** | Financial entities must implement a comprehensive ICT risk management framework that includes identity and access management controls as a core component |
| **Access control and authentication** | Strong authentication mechanisms, including multi-factor authentication, must be implemented for all access to ICT systems. Access rights must be managed on a need-to-know and least-privilege basis |
| **Privileged access management** | Privileged accounts must be subject to enhanced controls including approval workflows, session monitoring and regular review |
| **ICT third-party risk** | Identity and access arrangements with ICT service providers must be contractually defined, including requirements for access controls, MFA and audit trails |
| **Incident classification and reporting** | ICT-related incidents, including identity compromise, must be classified and reported to competent authorities. The classification considers impact on authentication and access management systems |
| **Digital operational resilience testing** | Regular testing of ICT systems, including identity infrastructure, through vulnerability assessments and threat-led penetration testing (TLPT) |

**Enforcement:**
- Fines of up to **1% of average daily global turnover** for financial entities
- ICT third-party service providers designated as critical can face fines of up to **EUR 5 million** (or EUR 500,000 for individuals)

**Reference:** [EU DORA Regulation](https://digital-strategy.ec.europa.eu/en/policies/dora)

---

### 1.6 Cyber Resilience Act (CRA)

**Regulation (EU) 2024/2847 — Entered into force 10 December 2024**

The CRA establishes cybersecurity requirements for **products with digital elements** (hardware and software) placed on the EU market, including identity management systems.

**Key Provisions Relevant to Identity:**

| Provision | Description |
|-----------|-------------|
| **Identity management systems as Important Products (Class I)** | The CRA explicitly classifies identity management systems, password managers, VPN software and web browsers as Important Class I products, subject to enhanced conformity assessment |
| **Secure by default** | Products must be delivered with secure default configurations, including strong authentication defaults and no hardcoded credentials |
| **Vulnerability management** | Manufacturers must maintain vulnerability handling processes for the expected product lifetime, including timely security patches |
| **Secure authentication** | Products must implement appropriate authentication mechanisms proportionate to the risk, including protection against brute force and credential stuffing attacks |

**Key Deadlines:**
- **11 September 2026:** Reporting obligations become mandatory
- **11 December 2027:** Full application of all CRA requirements

**Reference:** [EU Cyber Resilience Act](https://digital-strategy.ec.europa.eu/en/policies/cyber-resilience-act)

---

### 1.7 EU AI Act

**Regulation (EU) 2024/1689 — Entered into force 1 August 2024**

The AI Act is the world's first comprehensive regulation of artificial intelligence, with specific provisions affecting identity verification, biometrics and education.

**Key Provisions Relevant to Identity and Education:**

| Provision | Description |
|-----------|-------------|
| **Prohibited practices (Article 5)** | Real-time remote biometric identification in publicly accessible spaces is prohibited (with narrow law enforcement exceptions). Social scoring systems and emotion recognition in workplaces and educational settings are banned. AI systems that use biometric data to infer race, political views, religion or sexual orientation are prohibited |
| **High-risk AI in education (Annex III)** | AI systems used to **determine access or admission** to educational institutions at all levels are classified as high-risk. AI systems used to **evaluate learning outcomes** (including those determining assessment results) are classified as high-risk |
| **Biometric verification vs identification** | One-to-one biometric verification (e.g. fingerprint to unlock a device, facial recognition to confirm identity) is explicitly excluded from the prohibition on remote biometric identification. This preserves the use of biometric authentication in identity systems |
| **AI literacy requirement** | Since 2 February 2025, organisations must ensure employees involved in AI use and deployment have adequate AI literacy training |
| **Transparency** | Users must be informed when they are interacting with an AI system or when AI is being used to make decisions about them (including identity verification decisions) |

**Key Deadlines:**
- **2 February 2025:** Prohibited practices and AI literacy requirements apply
- **2 August 2025:** Requirements for general-purpose AI models apply
- **2 August 2026:** Full application of all high-risk AI system requirements

**Reference:** [EU AI Act](https://digital-strategy.ec.europa.eu/en/policies/regulatory-framework-ai)

---

## Part 2: Good Practice for the Education Sector

### 2.1 Scope: Education and NIS2

The NIS2 Directive includes **research organisations** (including universities and higher education institutions) as **important entities** within its scope. Some member states have extended NIS2 scope to include primary and secondary education providers in their national transposition (e.g. Italy, Romania).

This means that universities and research institutions in the EU are subject to the full range of NIS2 cybersecurity obligations, including identity and access management requirements.

### 2.2 Key Identity Challenges in Education

| Challenge | Description |
|-----------|-------------|
| **Large, transient user populations** | Universities manage tens of thousands of students, staff, researchers and visitors, with high annual turnover as students enrol and graduate |
| **Diverse user types** | Students (undergraduate, postgraduate, doctoral), academic staff, administrative staff, visiting researchers, external examiners, contractors, alumni and partner institution users all require different access levels |
| **Federated access (eduGAIN / GEANT)** | European academic institutions participate in inter-institutional federation networks (eduGAIN, GEANT) requiring identity federation and attribute release policies |
| **Research data sensitivity** | Research projects may involve sensitive personal data, health data, classified data or commercially valuable intellectual property, requiring strict access controls |
| **BYOD and shared laboratories** | Students and researchers use personal devices and shared laboratory equipment, complicating device trust and authentication |
| **EdTech and third-party platforms** | Extensive use of third-party learning platforms (LMS, virtual labs, collaboration tools) creates identity sprawl across multiple SaaS providers |
| **Children's data (under-18 students)** | Where institutions enrol minors (e.g. dual enrolment programmes), enhanced GDPR protections for children's data apply, including parental consent requirements and age verification |

### 2.3 EU Good Practice Guidance for Education Identity

#### 2.3.1 ENISA Guidance

The European Union Agency for Cybersecurity (ENISA) provides guidance relevant to education:

- **Cybersecurity Education Maturity Assessment:** A framework for assessing cybersecurity education maturity at national level in primary and secondary schools
- **CyberEducation Platform:** Central hub for cybersecurity educational resources tailored for schools in each member state
- **Cybersecurity Higher Education Database (CyberHEAD):** The largest validated database of cybersecurity higher education programmes in the EU and EFTA

**Reference:** [ENISA Education](https://www.enisa.europa.eu/topics/education)

#### 2.3.2 GEANT and eduGAIN Federation

GEANT operates the pan-European research and education network, including the **eduGAIN** inter-federation service that enables federated identity across academic institutions.

**Good Practice for Education Identity Federation:**

| Practice | Description |
|----------|-------------|
| **SAML 2.0 federation** | Institutions should operate a SAML 2.0 Identity Provider (IdP) registered with their national research and education federation (e.g. DFN-AAI in Germany, RENATER in France, SURFconext in the Netherlands) and connected to eduGAIN |
| **Attribute release policies** | Define and publish clear attribute release policies specifying what identity attributes (eduPersonPrincipalName, mail, affiliation, entitlement) are shared with which service providers |
| **eduPerson schema** | Use the **eduPerson** LDAP object class and attribute schema as the standard for representing identity attributes in the education sector |
| **Sirtfi (Security Incident Response Trust Framework)** | Participate in the Sirtfi framework to establish trust for security incident response across federated institutions |
| **Research and Scholarship entity category** | Adopt the Research and Scholarship (R&S) entity category to streamline attribute release for legitimate research and scholarship service providers |
| **REFEDS Assurance Framework (RAF)** | Implement the REFEDS Assurance Framework to communicate the level of identity assurance (identity proofing, credential strength, attribute quality) to federated service providers |

#### 2.3.3 GDPR Compliance in Education

| Good Practice | Description |
|---------------|-------------|
| **Appoint a Data Protection Officer** | Mandatory for public educational institutions under GDPR Article 37. The DPO oversees identity data processing compliance |
| **Maintain a Record of Processing Activities (ROPA)** | Document all identity data processing activities, including directory services, authentication logs, learning management systems and student records |
| **Conduct DPIAs for identity systems** | Perform Data Protection Impact Assessments before deploying new identity systems, biometric authentication or large-scale monitoring (e.g. exam proctoring) |
| **Lawful basis for student identity processing** | Typically relies on **public task** (Article 6(1)(e)) for public universities or **contract** (Article 6(1)(b)) for enrolment-related processing. Consent should be used sparingly given the power imbalance between institution and student |
| **Data retention schedules** | Define and enforce retention periods for student identity data, authentication logs and access records. Delete or anonymise data when retention periods expire |
| **Children's data safeguards** | For under-18 students, implement enhanced protections including parental consent mechanisms, age-appropriate privacy notices and restricted profiling |
| **EdTech vendor due diligence** | Assess third-party EdTech platforms for GDPR compliance before integrating them with institutional identity systems. Ensure data processing agreements (Article 28) are in place |

#### 2.3.4 eIDAS 2.0 and Education

The EUDI Wallet will have significant implications for the education sector:

| Impact Area | Description |
|-------------|-------------|
| **Digital diplomas and qualifications** | Institutions will be able to issue verifiable digital credentials (diplomas, certificates, micro-credentials) that students store in their EUDI Wallet and present to employers or other institutions |
| **Student identity verification** | The EUDI Wallet can be used for student identity verification during enrolment, examination and online assessment, reducing reliance on institution-specific credentials |
| **Cross-border student mobility** | The wallet enables seamless identity and qualification verification across EU member states, supporting programmes such as Erasmus+ |
| **Selective disclosure** | Students can prove specific attributes (e.g. age, enrolment status, qualification level) without revealing their full identity, supporting privacy-by-design principles |

#### 2.3.5 AI Act Implications for Education

| Obligation | Description |
|------------|-------------|
| **High-risk AI classification** | AI systems used for admissions decisions or learning outcome evaluation are classified as high-risk and must comply with requirements for risk management, data governance, transparency, human oversight and accuracy |
| **Emotion recognition ban** | Emotion recognition systems are prohibited in educational settings, except where strictly necessary for medical or safety purposes (e.g. detecting distress in a student) |
| **Biometric authentication permitted** | One-to-one biometric verification (e.g. fingerprint authentication for exam access) remains permitted and is not classified as remote biometric identification |
| **AI literacy** | Educational institutions deploying AI systems must ensure relevant staff have adequate AI literacy training |
| **Transparency** | Students must be informed when AI systems are used in decisions affecting them, including identity verification and assessment |

---

## Part 3: Good Practice for Critical National Infrastructure (CNI)

### 3.1 Scope: CNI and EU Regulations

Critical national infrastructure operators in the EU are subject to a layered regulatory framework:

| Regulation | Focus | Identity Relevance |
|------------|-------|-------------------|
| **NIS2 Directive** | Cybersecurity of essential and important entities | Access control, MFA, identity lifecycle, incident reporting |
| **CER Directive** | Physical and operational resilience of critical entities | Background checks, personnel security, physical access control |
| **DORA** (financial sector) | Digital operational resilience for financial entities | Strong authentication, privileged access, third-party ICT identity controls |
| **GDPR** | Personal data protection | Identity data processing, employee monitoring, breach notification |
| **Cyber Resilience Act** | Security of products with digital elements | Secure authentication defaults in identity products deployed in CNI |

### 3.2 Key Identity Challenges in Critical Infrastructure

| Challenge | Description |
|-----------|-------------|
| **Operational Technology (OT) identity** | Industrial control systems (ICS), SCADA and OT environments often rely on shared credentials, hardcoded passwords and legacy protocols with no modern authentication support |
| **Air-gapped and isolated networks** | Some CNI environments are physically isolated, making cloud-based identity services and MFA difficult to deploy |
| **High-availability requirements** | Identity systems in CNI must be designed for extreme availability, as authentication failures can prevent access to safety-critical systems |
| **Converging IT/OT identity** | As IT and OT networks converge, organisations must manage identities across both domains while maintaining segmentation and preventing lateral movement |
| **Third-party and vendor access** | CNI organisations rely on specialist vendors for equipment maintenance, requiring secure, time-bound, auditable third-party identity management |
| **Personnel vetting** | NIS2 and CER require background checks and personnel vetting for sensitive roles, creating identity assurance requirements beyond standard IAM |
| **Nation-state threat actors** | CNI organisations face sophisticated adversaries who specifically target identity infrastructure (Active Directory, federation services, VPN credentials) as primary attack vectors |
| **Regulatory multiplicity** | CNI operators may be simultaneously subject to NIS2, CER, DORA (if in financial sector), sector-specific regulations and national security requirements |

### 3.3 NIS2 Identity Good Practice for CNI

#### 3.3.1 Access Control and Authentication

| Good Practice | Description |
|---------------|-------------|
| **Implement centralised identity management** | Deploy a centralised identity provider (e.g. Active Directory, Entra ID, or an IGA platform) as the authoritative directory for all IT user identities. Maintain a separate, dedicated identity infrastructure for OT environments where required |
| **Enforce multi-factor authentication** | MFA must be enforced for all remote access, privileged access, VPN connections and access to critical applications. Use phishing-resistant methods (FIDO2, smartcards, certificate-based authentication) where possible |
| **Implement least-privilege access** | All user accounts must operate with the minimum permissions required. Privileged access must be just-in-time, time-bound and subject to approval workflows |
| **Separate IT and OT identities** | Maintain separate identity stores and authentication infrastructure for IT and OT environments. Where access between domains is necessary, implement tightly controlled jump servers with dedicated privileged accounts |
| **Manage service and machine identities** | Inventory all service accounts, API keys and machine identities. Implement automated credential rotation and remove hardcoded credentials from OT systems where feasible |
| **Enforce session management** | Implement session timeouts, re-authentication requirements and continuous access evaluation to prevent session hijacking and stale authenticated sessions |

#### 3.3.2 Identity Lifecycle Management

| Good Practice | Description |
|---------------|-------------|
| **Automate joiner/mover/leaver processes** | Connect identity management to HR systems to automate account creation, role changes and account deprovisioning. NIS2 requires that access rights are promptly adjusted when roles change |
| **Conduct regular access reviews** | Perform access certification campaigns at least quarterly for privileged access and annually for standard access, as evidence of ongoing compliance |
| **Manage third-party identities** | All contractor and vendor identities must have designated sponsors, time-limited access, mandatory MFA and be subject to regular access reviews |
| **Implement identity-aware offboarding** | When personnel leave or contracts end, all access must be revoked across all systems within a defined SLA (NIS2 expects prompt revocation). Implement continuous access evaluation to terminate active sessions immediately |
| **Personnel vetting and background checks** | In accordance with the CER Directive, perform background checks for personnel in sensitive roles. Maintain records of vetting status as part of the identity record |

#### 3.3.3 Privileged Access Management

| Good Practice | Description |
|---------------|-------------|
| **Deploy a PAM solution** | Implement a dedicated Privileged Access Management solution to vault, rotate and audit all privileged credentials |
| **Implement just-in-time access** | Privileged access should be granted on demand with time limits, justification, approval and automatic revocation |
| **Record privileged sessions** | All privileged sessions (especially on critical infrastructure systems) should be recorded for forensic analysis and compliance evidence |
| **Protect Tier 0 identity infrastructure** | Domain controllers, identity providers, federation services and certificate authorities are Tier 0 assets. Apply the highest level of protection including dedicated admin workstations, network isolation and enhanced monitoring |
| **Monitor for identity-based attacks** | Deploy monitoring for credential theft (Pass-the-Hash, Kerberoasting, Golden Ticket), directory replication attacks (DCSync) and federation abuse (SAML token forging) |

#### 3.3.4 Incident Response and Reporting

| Good Practice | Description |
|---------------|-------------|
| **Include identity in incident response plans** | Incident response plans must include procedures for identity-related incidents: credential compromise, directory breach, federation trust abuse and mass account lockout |
| **24-hour early warning** | NIS2 requires an early warning to the national CSIRT within 24 hours of becoming aware of a significant incident. Identity-related incidents (e.g. domain admin compromise, directory exfiltration) are likely to qualify as significant |
| **72-hour full notification** | A complete incident notification must follow within 72 hours, including root cause analysis, scope of identity compromise and remediation measures |
| **Maintain forensic evidence** | Preserve authentication logs, directory change logs and privileged session recordings to support incident investigation. Ensure log integrity through tamper-proof storage |

#### 3.3.5 Supply Chain Identity Management

| Good Practice | Description |
|---------------|-------------|
| **Assess vendor identity practices** | Evaluate the identity and access management maturity of critical ICT suppliers as part of supply chain risk assessments required by NIS2 |
| **Contractual identity requirements** | Include identity security requirements in contracts with ICT third-party providers: MFA enforcement, least-privilege access, access logging and incident notification obligations |
| **Managed service provider controls** | Where MSPs manage CNI infrastructure, ensure their administrative access is governed by dedicated accounts with time-bound access, session recording and regular access reviews |
| **Software supply chain identity** | Verify the identity and integrity of software suppliers. Require software bills of materials (SBOMs) and verify code signing certificates to prevent supply chain compromise |

### 3.4 DORA Identity Good Practice for Financial CNI

For financial sector entities that are also critical infrastructure operators, DORA imposes additional identity requirements:

| Good Practice | Description |
|---------------|-------------|
| **ICT risk management framework** | Identity and access management must be a documented component of the organisation's ICT risk management framework, with defined policies, procedures, roles and responsibilities |
| **Strong authentication for all ICT access** | DORA requires strong authentication for all access to ICT systems, going beyond NIS2's risk-proportionate approach |
| **ICT third-party contractual requirements** | Contracts with ICT service providers must include specific provisions for access control, authentication, audit trails and incident notification |
| **Threat-led penetration testing (TLPT)** | Identity infrastructure must be included in regular TLPT exercises. Test scenarios should include credential theft, privilege escalation, directory compromise and federation abuse |
| **ICT concentration risk** | Assess whether the organisation's identity infrastructure creates concentration risk (e.g. single identity provider dependency). Plan for resilience and alternative authentication paths |

---

## Part 4: Regulatory Cross-Reference Matrix

### 4.1 Identity Requirements by Regulation

| Identity Requirement | GDPR | eIDAS 2.0 | NIS2 | CER | DORA | CRA | AI Act |
|---------------------|------|-----------|------|-----|------|-----|--------|
| Access control policies | | | Required | Required | Required | | |
| Multi-factor authentication | | | Required (risk-based) | | Required (strong) | Secure defaults | |
| Identity lifecycle management (JML) | Implied (data minimisation, erasure) | | Required | | Required | | |
| Privileged access management | | | Required | | Required (enhanced) | | |
| Access reviews / certification | | | Required | | Required | | |
| Background checks / personnel vetting | | | | Required | | | |
| Incident reporting (identity breaches) | 72 hours | | 24h + 72h | | 24h + 72h | | |
| Data protection impact assessment | Required (high-risk processing) | | | | | | Required (high-risk AI) |
| Cross-border identity recognition | | Mandatory | | | | | |
| Biometric controls | Data minimisation | | | | | | Prohibited (real-time remote); High-risk (post) |
| Supply chain identity controls | Data processor obligations | | Required | | Required (contractual) | SBOM requirements | |
| Digital credential issuance | | EUDI Wallet | | | | | |
| AI-based identity decisions | | | | | | | High-risk classification |

### 4.2 Sector Applicability

| Regulation | Education | Energy | Transport | Health | Financial | Digital Infrastructure | Public Administration |
|------------|-----------|--------|-----------|--------|-----------|----------------------|----------------------|
| **GDPR** | Yes | Yes | Yes | Yes | Yes | Yes | Yes |
| **eIDAS 2.0** | Yes | Yes | Yes | Yes | Yes | Yes | Yes |
| **NIS2** | Yes (research/HE) | Yes | Yes | Yes | Yes | Yes | Yes |
| **CER** | No (unless designated) | Yes | Yes | Yes | Yes | Yes | Yes |
| **DORA** | No | No | No | No | Yes | No (unless ICT provider) | No |
| **CRA** | No (unless manufacturer) | No (unless manufacturer) | No (unless manufacturer) | No (unless manufacturer) | No (unless manufacturer) | No (unless manufacturer) | No |
| **AI Act** | Yes | Yes | Yes | Yes | Yes | Yes | Yes |

---

## Part 5: Summary of Key Compliance Dates

| Date | Regulation | Milestone |
|------|-----------|-----------|
| **25 May 2018** | GDPR | Fully applicable |
| **17 October 2024** | NIS2 | Transposition deadline (many member states ongoing) |
| **18 October 2024** | CER | Directive applies |
| **10 December 2024** | CRA | Entered into force |
| **17 January 2025** | DORA | Fully applicable to financial entities |
| **2 February 2025** | AI Act | Prohibited practices and AI literacy requirements apply |
| **17 April 2025** | NIS2 | Member states must identify essential and important entities |
| **2 August 2025** | AI Act | General-purpose AI model requirements apply |
| **2026** | eIDAS 2.0 | Member states must offer at least one EUDI Wallet |
| **17 January 2026** | CER | National strategies and risk assessments due |
| **2 August 2026** | AI Act | Full application of high-risk AI requirements |
| **11 September 2026** | CRA | Reporting obligations apply |
| **17 July 2026** | CER | Member states must identify critical entities |
| **21 November 2027** | eIDAS 2.0 | Businesses must accept the EUDI Wallet |
| **11 December 2027** | CRA | Full application of all CRA requirements |
