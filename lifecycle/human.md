# Governance and Control of Human Identities

## Overview
Human identities include staff, students, contractors, and partners who require access to systems and data. Effective governance ensures security, compliance, and a positive user experience.

---

## Microsoft Entra (Azure AD)
- **Best Practices:**
  - Use Azure AD for centralized identity management.
  - Enforce Multi-Factor Authentication (MFA) for all users.
  - Apply Conditional Access policies for risk-based access control.
  - Use Privileged Identity Management (PIM) for admin roles.
  - Regularly review and certify user access with Access Reviews.
  - Automate onboarding and offboarding processes.
- **Controls:**
  - Azure AD MFA and Conditional Access
  - Privileged Identity Management (PIM)
  - Access Reviews and dynamic groups
  - Self-service password reset
- **Guidance:**
  - [Microsoft: Identity and access management best practices](https://learn.microsoft.com/en-us/azure/active-directory/fundamentals/security-operations-identity)
  - [Microsoft: Conditional Access](https://learn.microsoft.com/en-us/azure/active-directory/conditional-access/overview)
  - [Microsoft: Privileged Identity Management](https://learn.microsoft.com/en-us/azure/active-directory/privileged-identity-management/pim-configure)

---

## AWS (Amazon Web Services)
- **Best Practices:**
  - Use IAM users and groups for human access.
  - Enforce MFA for all users.
  - Apply least privilege to IAM policies.
  - Rotate passwords and access keys regularly.
  - Use AWS SSO for centralized access management.
  - Monitor activity with CloudTrail and Config.
- **Controls:**
  - IAM users, groups, and policies
  - AWS SSO and MFA
  - Password policies and rotation
  - CloudTrail and Config for auditing
- **Guidance:**
  - [AWS: IAM best practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)
  - [AWS: AWS SSO](https://docs.aws.amazon.com/singlesignon/latest/userguide/what-is.html)
  - [AWS: Enabling MFA](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_mfa_enable_virtual.html)

---

## Legal and Regulatory Guidance
- **Europe (EU):**
  - GDPR requires strong authentication, access controls, and auditability for all human users.
  - Data minimization and privacy by design are mandatory.
- **UK:**
  - UK GDPR and NIS Regulations require secure management of human identities in regulated sectors.
  - Regular access reviews and least privilege are required.
- **NCSC (UK):**
  - [NCSC: Identity and access management collection](https://www.ncsc.gov.uk/collection/identity-and-access-management)
  - [NCSC: Authentication and access control](https://www.ncsc.gov.uk/collection/passwords)

---

*This document summarizes best practices, controls, and legal guidance for governing human identities in cloud and enterprise environments.*
