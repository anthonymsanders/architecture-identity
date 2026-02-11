# Governance and Control of Application Identities

## Overview
Application identities are used by software, services, and APIs to access resources and perform automated tasks. Proper governance ensures security, compliance, and operational integrity.

---

## AWS (Amazon Web Services)
- **Guidance:**
  - Use IAM Roles for applications instead of static credentials.
  - Apply least privilege to application roles and policies.
  - Rotate secrets using AWS Secrets Manager.
  - Monitor application activity with CloudTrail.
  - [AWS: Best practices for IAM roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)
  - [AWS: Application authentication](https://aws.amazon.com/identity/application-authentication/)

---

## Microsoft (Azure/Entra)
- **Guidance:**
  - Register applications in Azure AD and use service principals.
  - Assign least privilege permissions to app registrations.
  - Use Managed Identities for Azure resources.
  - Monitor and review app permissions regularly.
  - [Microsoft: Secure application registration](https://learn.microsoft.com/en-us/azure/active-directory/develop/secure-app-model)
  - [Microsoft: Managed identities](https://learn.microsoft.com/en-us/azure/active-directory/managed-identities-azure-resources/overview)

---

## Google Cloud
- **Guidance:**
  - Use Service Accounts for applications.
  - Grant only required IAM roles to service accounts.
  - Rotate and manage keys with Secret Manager.
  - Monitor access with Cloud Audit Logs.
  - [Google: Service accounts](https://cloud.google.com/iam/docs/service-accounts)
  - [Google: Best practices for IAM](https://cloud.google.com/iam/docs/best-practices)

---

## Oracle Cloud
- **Guidance:**
  - Use Dynamic Groups and Policies for application access.
  - Assign least privilege to application identities.
  - Use Vault for secret management.
  - Monitor with Audit service.
  - [Oracle: IAM for applications](https://docs.oracle.com/en-us/iaas/Content/Identity/Tasks/managingdynamicgroups.htm)
  - [Oracle: Security best practices](https://docs.oracle.com/en-us/iaas/Content/Security/Concepts/security_best_practices.htm)

---

## IBM Cloud
- **Guidance:**
  - Use Service IDs for applications.
  - Assign only necessary access policies.
  - Use IBM Secrets Manager for credentials.
  - Monitor with Activity Tracker.
  - [IBM: Service IDs](https://cloud.ibm.com/docs/account?topic=account-serviceids)
  - [IBM: IAM best practices](https://cloud.ibm.com/docs/account?topic=account-iamoverview)

---

## SailPoint
- **Guidance:**
  - Integrate application identities into identity governance workflows.
  - Automate provisioning and access reviews for applications.
  - Monitor application access and enforce policies.
  - [SailPoint: Application onboarding](https://community.sailpoint.com/t5/IdentityNow-Articles/Application-Onboarding-Overview/ta-p/80344)
  - [SailPoint: Best practices](https://community.sailpoint.com/t5/IdentityNow-Articles/Best-Practices-for-Application-Onboarding/ta-p/80345)

---

## Okta
- **Guidance:**
  - Register applications in Okta and assign appropriate permissions.
  - Use OAuth and OpenID Connect for secure app authentication.
  - Automate app provisioning and deprovisioning.
  - [Okta: Application integration](https://developer.okta.com/docs/guides/)
  - [Okta: Best practices](https://help.okta.com/oie/en-us/Content/Topics/Apps/Apps_Best_Practices.htm)

---

## Ping Identity
- **Guidance:**
  - Use PingFederate or PingOne for application SSO and federation.
  - Apply least privilege to application connectors.
  - Monitor and audit application access.
  - [Ping: Application integration](https://docs.pingidentity.com/bundle/pingfederate-112/page/ubk1564002972642.html)
  - [Ping: Best practices](https://docs.pingidentity.com/bundle/pingfederate-112/page/ubk1564002972642.html)

---

## One Identity
- **Guidance:**
  - Onboard applications into identity governance workflows.
  - Use connectors for automated provisioning.
  - Enforce access reviews and policy compliance.
  - [One Identity: Application onboarding](https://support.oneidentity.com/technical-documents/identity-manager/9.1/administration-guide/22#TOPIC-1802457)
  - [One Identity: Best practices](https://support.oneidentity.com/technical-documents/identity-manager/9.1/administration-guide/22#TOPIC-1802457)

---

## OpenText
- **Guidance:**
  - Register and manage application identities in OpenText IAM.
  - Assign least privilege and monitor app access.
  - Use connectors for integration with enterprise applications.
  - [OpenText: Application identity management](https://www.opentext.com/products/identity-access-management)
  - [OpenText: Support resources](https://www.opentext.com/support/resources)

---

*This document summarizes best practices and vendor guidance for governing application identities in cloud and enterprise environments.*
