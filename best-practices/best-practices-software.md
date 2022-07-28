---

copyright:
  years: 2020, 2022
lastupdated: "2022-07-28"

keywords: 

subcollection: framework-financial-services

---

{{site.data.keyword.attribute-definition-list}}

# Best practices and requirements for software
{: #best-practices-software}

These best practices summarize some of the most important technical principles for software providers based on the [controls](/docs/allowlist/framework-financial-services?topic=framework-financial-services-about#framework-control-requirements) and implementation guidance of the {{site.data.keyword.framework-fs_notm}}. These best practices are _not_ a replacement for the information provided in the [Control Implementation Overview templates](/docs/allowlist/framework-financial-services?topic=framework-financial-services-about#control-implementation-overview-templates), which are still the definitive guides to the controls and required evidence for application providers.
{: shortdesc}

If you're a SaaS provider, see [Best practices for software as a service](/docs/allowlist/framework-financial-services?topic=framework-financial-services-best-practices).
{: tip}

## 1. Enable operators to properly configure your application to meet the controls
{: #best-practices-reference-architecture}

**Requirement:** Enable and organization operating your software to meet the controls of the {{site.data.keyword.framework-fs_notm}}.

**Purpose & value:** An operator is responsible for the {{site.data.keyword.framework-fs_notm}} controls for every piece of software they run in their management or workload VPCs, whether running in a virtual server instance or in {{site.data.keyword.openshiftshort}} containers. So, the overarching principle for a software provider is to enable their software to be deployed, configured, and managed in a way that could be Financial Services Validated.

**Implementation guidance:** Provide clear written instructions for an operator to follow in order to deploy, configure, and manage your software on the {{site.data.keyword.cloud_notm}} for Financial Services.

## 2. Provide account and access management to enable a zero-trust environment
{: #best-practices-zero-trust}

**Requirement:** Enable operators to implement a system of zero-trust where account, identity, and access management is based on the principles of separation of duties and least privilege.

**Purpose & value:** The design of the {{site.data.keyword.cloud_notm}} for Financial Services is intended to enable a zero-trust model for deployments of the reference architectures. The scope of any one individual operator or administrator is strictly limited to those actions necessary and appropriate to perform their assigned roles. Overlap of duties is minimized or eliminated to prevent the need for privilege escalation that may result in undesired flows of information into or out of the operator's deployment. It is guided by two major principles:

* [Separation of duties](https://csrc.nist.gov/glossary/term/separation_of_duty){: external} - No user should be given enough privileges to misuse the system on their own. For example, no one user should have the authority/ability to develop, compile, and/or move object code from non-production environments into production environments.
* [Least privilege](https://csrc.nist.gov/glossary/term/least_privilege){: external} - Restrict the access privileges of authorized personnel (e.g., program execution privileges, file modification privileges) to the minimum necessary to perform their jobs. Access must be granted based only on a need to know. Access that has not been explicitly permitted is denied by default. Role-based access controls (RBAC) are used to allocate logical access to a specific job function or area of responsibility, rather than to an individual. Administrative access is only granted to individuals whose job roles have been approved for administration responsibilities.

**Implementation guidance:** As a software provider, you must provide facilities to accomplish these goals as appropriate. As a simple example, if your software provides a database, you should provide the ability to create database users with different roles (e.g., viewer, editor, administrator).

Ideally, you should support integration with an external (to the application) identity and access management provider for validating users and their associated access group membership.

**Most Relevant Control Groups or Controls:**


| Family              | Control                                           |
|---------------------|---------------------------------------------------|
| Access Control (AC) | AC-2 Account Management \n AC-3 Access Enforcement \n AC-5 Separation of Duties \n AC-6 Least Privilege \n AC-14 Permitted Actions without Identification or Authentication |
{: caption="Table 1. Related controls for zero trust" caption-side="top"}

## 3. Enable ability to capture audit events
{: #best-practices-audit-logs}

**Requirement:** Provide technical capability within the application to capture audit events.

**Purpose & value:** It is important that the operator has access to audit logs to be able to investigate abnormal activity as well to comply with regulatory audit requirements. With a proper audit logging system, an operator can be automatically alerted to potential problems. This allows the operator to be proactive and check into potential security issues shortly after they've occurred.

**Implementation guidance:** You must log critical events related to administering your software that can be accessed by the operator. Many examples of auditable events are listed in the AU-2 control from the {{site.data.keyword.framework-fs_notm}}, including:

* Access, downloading, revisions to confidential information
* Access to or update to any financial transaction data
* Any changes to consumer accounts
* Login/logoff event successes and failures
* etc.

The audit events that are generated should comply with the Cloud Auditing Data Federation (CADF) standard using the system time for timestamps.

The software should buffer audit logs locally in the event that the central audit facility is unavailable, and if the capacity for buffering the audit records is exceeded and audit events are discarded, the developer should send an audit event indicating the loss of audit records once the central audit facility is accessible.

You must provide the technical capability to ensure only permitted users have access to audit records.

**Most Relevant Control Groups or Controls:**

| Family              | Control                                           |
|---------------------|---------------------------------------------------|
| Audit and Accountability (AU) | AU-2 Audit Events \n AU-3 Content of Audit Records \n AU-5 Response to Audit Processing Failures \n AU-8 Time Stamps \n AU-9 Protection of Audit Information \n AU-10 Non-repudiation \n AU-11 Audit Record Retention \n AU-12 Audit Generation |
{: caption="Table 2. Related controls for audit logs" caption-side="top"}

## 4. Follow secure development processes and ensure software integrity 
{: #best-practices-development-processes}

**Requirement:** Ensure that security engineering principles are applied in the design, development, implementation, and modification of your software. Maintain software integrity by using signed images, applying security patches, doing vulnerability scans, etc.

**Purpose & value:** It is critical to follow security engineering principles and manage changes to your software using a system development life cycle (SDLC) that incorporates information security considerations. Doing so minimizes the risk of introducing code or configuration changes that undermine your software's security.

Lack of software integrity leaves your operator vulnerable to security problems. So, it is critical for you to be able to assert the origin of all software components in your application and to keep software components up to date.

**Most Relevant Control Groups or Controls:**

| Family              | Control                                           |
|---------------------|---------------------------------------------------|
| System and Services Acquisition (SA) | SA-8 Security Engineering Principles \n SA-11 Developer Security Testing and Evaluation \n SA-12 Supply Chain Protection \n SA-14 Criticality Analysis \n SA-15 Development Process, Standards, and Tools
{: caption="Table 3. Related controls for development processes" caption-side="top"}

## 5. Ensure consumer data can be encrypted at rest and in transit
{: #best-practices-encryption}

**Requirement:** Ensure operator can configure your software so that consumer data can be encrypted properly at rest and in transit.

**Purpose & value:** Consumers do not want their data to be accessible to bad actors. Protecting consumer data against unauthorized disclosure, modification, or destruction throughout the data lifecycle is of paramount importance in the {{site.data.keyword.cloud_notm}} for Financial Services. Encryption is a major component of this protection, and operators must be able to configure your software to meet the data encryption requirements of the {{site.data.keyword.framework-fs_notm}}.

**Implementation guidance:**

* You must implement and document application capabilities and configuration requirements in accordance with the cryptographic specifications provided in the Supplemental Guidance for Cryptography section of the CIO template.
* You must implement and document application capabilities and configuration requirements related to using cryptographic keys provided via [{{site.data.keyword.hscrypto}}](/docs/hs-crypto?topic=hs-crypto-overview) to protect data in transit and at rest.

**Most Relevant Control Groups or Controls:**

| Family              | Control                                           |
|---------------------|---------------------------------------------------|
| System and Communications Protection (SC) | SC-8 Transmission Confidentiality and Integrity \n SC-8 (1) Transmission Confidentiality and Integrity &#124; Cryptographic Protection \n SC-12 Cryptographic Key Establishment and Management \n SC-12 (2) Cryptographic Key Establishment and Management &#124; Symmetric Keys \n SC-12 (3) Cryptographic Key Establishment and Management &#124; Asymmetric Keys \n SC-13 Cryptographic Protection \n SC-28 Protection of Information At Rest \n SC-28 (1) Protection of Information at Rest &#124; Cryptographic Protection  |
{: caption="Table 4. Related controls for data encryption" caption-side="top"}

## 6. Enable software to be run in a highly available manner
{: #best-practices-high-availability}

**Requirement:** Design your software so that operators can meet the {{site.data.keyword.framework-fs_notm}} requirements for high availability.

**Purpose & value:** Operators SaaS offerings are required to be highly available so that consumers do not face downtime of critical applications. Typically, high availability is addressed mostly by the operator, but you will need to provide documentation if your software or software deployment pattern includes enhanced availability capabilities (such as replication, active-active, active-passive,  etc.).

**Most Relevant Control Groups or Controls:**

| Family              | Control                                           |
|---------------------|---------------------------------------------------|
| System and Communications Protection (SC) | SC-6 Resource Availability |
{: caption="Table 6. Related controls for high-availability" caption-side="top"}



## Next steps
{: #best-practices-next-steps}

If you haven't already, you should learn about the three reference architectures for the {{site.data.keyword.cloud_notm}} for Financial Services where your software would be deployed:

{{site.data.content.reference-architectures-list-full-names}}


