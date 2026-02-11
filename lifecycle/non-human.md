# Governance and Control of Non-Human Identities

## Overview
Non-human identities (NHIs) include service accounts, application identities, bots, and machine identities used for automation, integration, and cloud operations. Effective governance is critical to reduce risk, ensure compliance, and maintain operational integrity.

---

## Microsoft Entra (Azure AD)
- **Best Practices:**
  - Use Managed Identities for Azure resources instead of traditional service accounts.
  - Assign least privilege via Azure AD roles and custom roles.
  - Use Conditional Access policies for service principals.
  - Monitor and review permissions regularly with Access Reviews.
  - Enable logging and alerting for suspicious activity.
- **Controls:**
  - Managed Identities (System-assigned and User-assigned)
  - Azure AD Privileged Identity Management (PIM)
  - Access Reviews and Conditional Access
  - Azure Policy for resource governance
- **Guidance:**
  - [Microsoft: Secure application registration and permissions](https://learn.microsoft.com/en-us/azure/active-directory/develop/secure-app-model)
  - [Microsoft: Managed identities for Azure resources](https://learn.microsoft.com/en-us/azure/active-directory/managed-identities-azure-resources/overview)
  - [Microsoft: Best practices for managing non-human identities](https://learn.microsoft.com/en-us/azure/active-directory/fundamentals/security-operations-applications)

---

## AWS (Amazon Web Services)
- **Best Practices:**
  - Use IAM Roles for EC2, Lambda, and other AWS services instead of static credentials.
  - Apply least privilege to IAM roles and policies.
  - Rotate access keys and secrets regularly.
  - Enable CloudTrail and Config for monitoring and auditing.
  - Use AWS Organizations SCPs for governance.
- **Controls:**
  - IAM Roles and Policies
  - Resource-based policies
  - AWS Secrets Manager and Parameter Store
  - AWS Config and CloudTrail
- **Guidance:**
  - [AWS: Best practices for managing AWS access keys](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)
  - [AWS: Service-linked roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html)
  - [AWS: Identity governance in AWS](https://aws.amazon.com/identity/governance/)

---

## Legal and Regulatory Guidance
- **Europe (EU):**
  - NIS2 Directive and GDPR require strong controls and auditability for all identities, including non-human.
  - Machine identities must be managed to prevent unauthorized access and data breaches.
- **UK:**
  - UK GDPR and NIS Regulations apply to non-human identities in regulated sectors.
  - Principle of least privilege and auditability is required.
- **NCSC (UK):**
  - [NCSC: Managing machine identities](https://www.ncsc.gov.uk/collection/identity-and-access-management/mitigations/machine-identities)
  - [NCSC: Identity and access management collection](https://www.ncsc.gov.uk/collection/identity-and-access-management)

---

*This document summarizes best practices, controls, and legal guidance for governing non-human identities in cloud and enterprise environments.*
