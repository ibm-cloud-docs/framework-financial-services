---

copyright:
  years: 2020, 2025
lastupdated: "2025-02-20"

keywords:

subcollection: framework-financial-services

---

{{site.data.keyword.attribute-definition-list}}

# {{site.data.keyword.cloud_notm}} account setup
{: #shared-account-setup}

An [{{site.data.keyword.cloud_notm}} account](/docs/account?topic=account-overview) is needed to provision and manage {{site.data.keyword.cloud_notm}} services that make up the reference architectures of the {{site.data.keyword.cloud_notm}} for Financial Services. Along with the high-level steps to follow, we describe some of the best practices for account setup that will help you satisfy the requirements of the {{site.data.keyword.framework-fs_notm}}. In addition, the most relevant [control requirements](/docs/framework-financial-services?topic=framework-financial-services-about#framework-control-requirements) are provided.
{: shortdesc}

1. Create an {{site.data.keyword.cloud_notm}} account. For more information, see [Create your account](/docs/account?topic=account-account-getting-started#account-gs-create).

   It is highly recommended that you use a functional ID that is owned by your company rather than an employee's personal ID. A functional ID is a company-owned email address (such as `ibm-cloud-admin@domain.com`) used to represent a functional user. This allows for uninterrupted administrative access by the account owner as employees leave the company or are reassigned to other projects.
   {: tip}

   The following table shows the controls that are most related to this step.

   | Family              | Control                                           |
   |---------------------|---------------------------------------------------|
   | Access Control (AC) | [AC-2 Account Management](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-ac-2)  |
   {: caption="Related controls in {{site.data.keyword.framework-fs_notm}} for account creation [FSv2.0]" caption-side="top"}
   {: #related-controls-fsv2.0}
   {: tab-title="FSv2.0"}
   {: tab-group="RelatedControls-1"}
   {: class="simple-tab-table"}


   | Family              | Control                                           |
   |---------------------|---------------------------------------------------|
   | Access Control (AC) | [AC-2 Account Management](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-ac-2)  |
   {: caption="Related controls in {{site.data.keyword.framework-fs_notm}} for account creation [FSv1.1]" caption-side="top"}
   {: #related-controls-fsv1.1}
   {: tab-title="FSv1.1"}
   {: tab-group="RelatedControls-1"}
   {: class="simple-tab-table"}


1. Set up the {{site.data.keyword.atracker_short}} service as described in [Audit logging for {{site.data.keyword.cloud_notm}} events](/docs/framework-financial-services?topic=framework-financial-services-shared-logging-audit). This enables {{site.data.keyword.cloud_notm}} platform events to be recorded for auditing purposes. Setting this up early in the process is important so that all platform events that occur during the rest of these steps are available in the audit logs.

   The following table shows the controls that are most related to this step.

   | Family              | Control                                           |
   |---------------------|---------------------------------------------------|
   | Access Control (AC) | [AC-2 Account Management](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-ac-2) \n [AC-2 (1) Account Management &#124; Automated System Account Management](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-ac-2.1) \n [AC-2 (4) Account Management &#124; Automated Audit Actions](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-ac-2.4) \n [AC-2 (7) Account Management &#124; Privileged User Accounts](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-ac-2.7) |
   | Audit and Accountability (AU) | [AU-3 Content of Audit Records](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-au-3) \n [AU-4 Audit Log Storage Capacity](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-au-4) \n [AU-5 Response to Audit Processing Failures](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-au-5) \n [AU-6 Audit Record Review, Analysis. and Reporting](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-au-6) \n [AU-6 (1) Audit Record Review, Analysis. and Reporting &#124; Automated Process Integration](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-au-6.1) \n [AU-7 Audit Record Reduction and Report Generation](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-au-7) \n [AU-10 Non-repudiation](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-au-10) \n [AU-11 Audit Record Retention](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-au-11) |
   {: caption="Related controls in {{site.data.keyword.framework-fs_notm}} for audit logging [FSv2.0]" caption-side="top"}
   {: #related-controls-fsv2.0}
   {: tab-title="FSv2.0"}
   {: tab-group="RelatedControls-2"}
   {: class="simple-tab-table"}


   | Family              | Control                                           |
   |---------------------|---------------------------------------------------|
   | Access Control (AC) | [AC-2 Account Management](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-ac-2) \n [AC-2 (1) Account Management &#124; Automated System Account Management](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-ac-2.1) \n [AC-2 (4) Account Management &#124; Automated Audit Actions](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-ac-2.4) \n [AC-2 (7) Account Management &#124; Privileged User Accounts](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-ac-2.7) |
   | Audit and Accountability (AU) | [AU-3 Content of Audit Records](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-au-3) \n [AU-4 Audit Log Storage Capacity](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-au-4) \n [AU-5 Response to Audit Processing Failures](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-au-5) \n [AU-6 Audit Record Review, Analysis. and Reporting](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-au-6) \n [AU-6 (1) Audit Record Review, Analysis. and Reporting &#124; Automated Process Integration](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-au-6.1) \n [AU-7 Audit Record Reduction and Report Generation](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-au-7) \n [AU-10 Non-repudiation](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-au-10) \n [AU-11 Audit Record Retention](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-au-11) |
   {: caption="Related controls in {{site.data.keyword.framework-fs_notm}} for audit logging [FSv1.1]" caption-side="top"}
   {: #related-controls-fsv1.1}
   {: tab-title="FSv1.1"}
   {: tab-group="RelatedControls-2"}
   {: class="simple-tab-table"}


1. Upgrade your account to either Pay-As-You-Go or Subscription. For more information, see [Upgrading your account](/docs/account?topic=account-upgrading-account).

   It is highly recommended to upgrade to a Subscription account so that you can [set up an enterprise](/docs/framework-financial-services?topic=framework-financial-services-shared-account-access-management#enterprise). Enterprises offer significant advantages in your ability to scale your environment over time as described in [Enterprise account architecture](/docs/enterprise-account-architecture?topic=enterprise-account-architecture-about).
   {: tip}

1. Enable [multi-factor authentication (MFA)](/docs/account?topic=account-enablemfa) by using the **U2F MFA** type for all users in your account. Users authenticate by using a physical hardware-based security key that generates a six-digit numerical code. Based on the FIDO U2F standard, this method offers the highest level of security. This security is needed because the {{site.data.keyword.framework-fs_notm}} requires a smart card or hardware token that is designed and operated to FIPS 140-2 level 2 or higher or equivalent (for example, ANSI X9.24 or ISO 13491-1:2007).

   The following table shows the controls that are most related to this step.

   | Family              | Control                                           |
   |---------------------|---------------------------------------------------|
   | Identification and Authentication (IA) | [IA-2 (1) Identification and Authentication (Organizational Users) &#124; Multi-factor Authentication To Privileged Accounts](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-ia-2.1) \n [IA-2 (11) Identification and Authentication (Organizational Users) &#124; Remote Access - Separate Device](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-ia-2.11) |
   {: caption="Related controls in {{site.data.keyword.framework-fs_notm}} for multi-factor authentication [FSv2.0]" caption-side="top"}
   {: #related-controls-fsv2.0}
   {: tab-title="FSv2.0"}
   {: tab-group="RelatedControls-3"}
   {: class="simple-tab-table"}


      | Family              | Control                                           |
   |---------------------|---------------------------------------------------|
   | Identification and Authentication (IA) | [IA-2 (1) Identification and Authentication (Organizational Users) &#124; Multi-factor Authentication To Privileged Accounts](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-ia-2.1) \n [IA-2 (11) Identification and Authentication (Organizational Users) &#124; Remote Access - Separate Device](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-ia-2.11) |
   {: caption="Related controls in {{site.data.keyword.framework-fs_notm}} for multi-factor authentication [FSv1.1]" caption-side="top"}
   {: #related-controls-fsv1.1}
   {: tab-title="FSv1.1"}
   {: tab-group="RelatedControls-3"}
   {: class="simple-tab-table"}



1. Restrict IP addresses from which a user can access the {{site.data.keyword.cloud_notm}} account. For more information, see [Allowing specific IP addresses for an account](/docs/account?topic=account-ips#ips_account) for more information.

   The following table shows the controls that are most related to this step.

   | Family              | Control                                           |
   |---------------------|---------------------------------------------------|
   | Access Control (AC) | [AC-4 Information Flow Enforcement](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-ac-4) |
   | System and Communications Protection (SC)  | [SC-7 Boundary Protection](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-7) \n [SC-7 (5) Boundary Protection &#124; Deny By Default - Allow By Exception](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-7.5) |
   {: caption="Related controls in {{site.data.keyword.framework-fs_notm}} for restricting IP addresses [FSv2.0]" caption-side="top"}
   {: #related-controls-fsv2.0}
   {: tab-title="FSv2.0"}
   {: tab-group="RelatedControls-4"}
   {: class="simple-tab-table"}


   | Family              | Control                                           |
   |---------------------|---------------------------------------------------|
   | Access Control (AC) | [AC-4 Information Flow Enforcement](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-ac-4) |
   | System and Communications Protection (SC)  | [SC-7 Boundary Protection](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-sc-7) \n [SC-7 (5) Boundary Protection &#124; Deny By Default - Allow By Exception](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-sc-7.5) |
   {: caption="Related controls in {{site.data.keyword.framework-fs_notm}} for restricting IP addresses [FSv1.1]" caption-side="top"}
   {: #related-controls-fsv1.1}
   {: tab-title="FSv1.1"}
   {: tab-group="RelatedControls-4"}
   {: class="simple-tab-table"}


1. While optional, it is recommended that you [enable authentication from an external identity provider (IdP)](/docs/account?topic=account-idp-integration) to securely authenticate external users to your {{site.data.keyword.cloud_notm}} account. This provides a way for your employees to use your company's single sign-on (SSO) solution.

1. Enable the {{site.data.keyword.cloud_notm}} for Financial Services Validated setting in your account. With this setting, you can [filter the catalog](https://cloud.ibm.com/catalog?search=label%3Afs_ready){: external} for services that are designated as Financial Services Validated and indicates that your account stores regulated financial services information. If you enable Financial Services Validated, your account still has access to the full public catalog. For more information, see [Enabling your account to use Financial Services Validated products](/docs/account?topic=account-enabling-fs-validated).

   The following table shows the controls that are most related to this step.

   | Family              | Control                                           |
   |---------------------|---------------------------------------------------|
   | Access Control (AC) | [AC-20 Use of External Information Systems](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-ac-20) |
   | System and Services Acquisition (SA) | [SA-4 Acquisitions Process](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sa-4) \n [SA-9 External Information System Services](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sa-9) |
   | Enterprise System and Services Acquisition (ESA) | [ESA-5 Subcontractor Risk Management](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-esa-5) |
   | Security Assessment and Authorization (CA) | [CA-3 System Interconnections](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-ca-3) |
   {: caption="Related controls  in {{site.data.keyword.framework-fs_notm}} for using only Financial Services Validated services [FSv2.0]" caption-side="top"}
   {: #related-controls-fsv2.0}
   {: tab-title="FSv2.0"}
   {: tab-group="RelatedControls-5"}
   {: class="simple-tab-table"}


   | Family              | Control                                           |
   |---------------------|---------------------------------------------------|
   | Access Control (AC) | [AC-20 Use of External Information Systems](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-ac-20) |
   | System and Services Acquisition (SA) | [SA-4 Acquisitions Process](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-sa-4) \n [SA-9 External Information System Services](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-sa-9) |
   | Enterprise System and Services Acquisition (ESA) | [ESA-5 Subcontractor Risk Management](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-esa-5) |
   | Security Assessment and Authorization (CA) | [CA-3 System Interconnections](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-ca-3) |
   {: caption="Related controls  in {{site.data.keyword.framework-fs_notm}} for using only Financial Services Validated services [FSv1.1]" caption-side="top"}
   {: #related-controls-fsv1.1}
   {: tab-title="FSv1.1"}
   {: tab-group="RelatedControls-5"}
   {: class="simple-tab-table"}


1. Set the session inactivity timeout to 15 minutes. For more information, see [Setting the sign-out due to inactivity duration](/docs/account?topic=account-iam-work-sessions#sessions-inactivity).

   The following table shows the controls that are most related to this step.

   | Family              | Control                                           |
   |---------------------|---------------------------------------------------|
   | Access Control (AC) | [AC-11 Session Lock](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-ac-11) |
   {: caption="Related controls in {{site.data.keyword.framework-fs_notm}} for using only session inactivity timeout [FSv2.0]" caption-side="top"}
   {: #related-controls-fsv2.0}
   {: tab-title="FSv2.0"}
   {: tab-group="RelatedControls-6"}
   {: class="simple-tab-table"}


   | Family              | Control                                           |
   |---------------------|---------------------------------------------------|
   | Access Control (AC) | [AC-11 Session Lock](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-ac-11) |
   {: caption="Related controls in {{site.data.keyword.framework-fs_notm}} for using only session inactivity timeout [FSv1.1]" caption-side="top"}
   {: #related-controls-fsv1.1}
   {: tab-title="FSv1.1"}
   {: tab-group="RelatedControls-6"}
   {: class="simple-tab-table"}


1. [Update company profile details](/docs/account?topic=account-contact-info).

   The following table shows the controls that are most related to this step.

   | Family              | Control                                           |
   |---------------------|---------------------------------------------------|
   | Configuration Management (CM) | [CM-8 (4) Information System Component Inventory &#124; Accountability Information](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-cm-8.4) |
   {: caption="Related {{site.data.keyword.framework-fs_notm}} controls for updating company profile details [FSv2.0]" caption-side="top"}
   {: #related-controls-fsv2.0}
   {: tab-title="FSv2.0"}
   {: tab-group="RelatedControls-7"}
   {: class="simple-tab-table"}


   | Family              | Control                                           |
   |---------------------|---------------------------------------------------|
   | Configuration Management (CM) | [CM-8 (4) Information System Component Inventory &#124; Accountability Information](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-cm-8.4) |
   {: caption="Related {{site.data.keyword.framework-fs_notm}} controls for updating company profile details [FSv1.1]" caption-side="top"}
   {: #related-controls-fsv1.1}
   {: tab-title="FSv1.1"}
   {: tab-group="RelatedControls-7"}
   {: class="simple-tab-table"}


1. [Set email preferences for notifications](/docs/account?topic=account-email-prefs). You can receive email notifications about {{site.data.keyword.cloud_notm}} platform-related items, such as announcements, critical events, security notices, billing and usage, and ordering.

   The following table shows the controls that are most related to this step.

   | Family              | Control                                           |
   |---------------------|---------------------------------------------------|
   | System and Information Integrity (SI) |  [SI-2 Flaw Remediation](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-si-2) \n [SI-5 Security Alerts & Advisories](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-si-5) |
   {: caption="Related {{site.data.keyword.framework-fs_notm}} controls for configuring notifications [FSv2.0]" caption-side="top"}
   {: #related-controls-fsv2.0}
   {: tab-title="FSv2.0"}
   {: tab-group="RelatedControls-8"}
   {: class="simple-tab-table"}


   | Family              | Control                                           |
   |---------------------|---------------------------------------------------|
   | System and Information Integrity (SI) |  [SI-2 Flaw Remediation](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-si-2) \n [SI-5 Security Alerts & Advisories](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-si-5) |
   {: caption="Related {{site.data.keyword.framework-fs_notm}} controls for configuring notifications [FSv1.1]" caption-side="top"}
   {: #related-controls-fsv1.1}
   {: tab-title="FSv1.1"}
   {: tab-group="RelatedControls-8"}
   {: class="simple-tab-table"}


1. Choose a support plan. For more information, see [Basic, Advanced, and Premium Support plans](/docs/get-support?topic=get-support-support-plans).

## Next steps
{: #next-steps}

* [Organizing your {{site.data.keyword.cloud_notm}} accounts and resources](/docs/framework-financial-services?topic=framework-financial-services-shared-account-organization)
