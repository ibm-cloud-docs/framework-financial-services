---

copyright:
  years: 2020, 2023
lastupdated: "2023-01-24"

keywords: 

subcollection: framework-financial-services

---

{{site.data.keyword.attribute-definition-list}}

# Best practices and requirements for software as a service
{: #best-practices}

These best practices summarize some of the most important technical principles for SaaS providers based on the [controls requirements](](/docs/framework-financial-services-controls) and implementation guidance of the {{site.data.keyword.framework-fs_notm}}. These best practices are _not_ a replacement for the information provided in the [Control Implementation Overview templates](/docs/framework-financial-services?topic=framework-financial-services-about#control-implementation-overview-templates), which are still the definitive guides to the controls and required evidence for application providers.
{: shortdesc}

If you're a software provider, see [Best practices for software](/docs/framework-financial-services?topic=framework-financial-services-best-practices-software).
{: tip}

## 1. Use an approved reference architecture
{: #best-practices-reference-architecture}

**Requirement:** Deploy and manage {{site.data.keyword.cloud_notm}} resources as defined by an approved reference architecture.

**Purpose & value:** The quickest path toward meeting the controls of the {{site.data.keyword.framework-fs_notm}} is to use one of the pre-defined, single-tenant reference architectures. These architectures are designed to facilitate implementing the other requirements and best practices such as network isolation, security function isolation, high availability, backup and recovery, encryption, audit logging, compliance monitoring, etc.

A key aspect of the framework is to separate user workloads from system management functionality and isolate security functions from non-security functions. One of the most important features of the reference architectures is the definition of physical and logical separation between the edge, management, and workload planes. It is your responsibility to ensure that separation stays in place.

**Implementation guidance:** Explore and choose one of the supported reference architectures for the {{site.data.keyword.cloud_notm}} for Financial Services:

{{site.data.content.reference-architectures-list-full-names}}

**Most Relevant Controls:**

| Family              | Control                                           |
|---------------------|---------------------------------------------------|
| System and Communications Protection (SC) | [SC-2 Application Partitioning](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-2) \n [SC-3 Security Function Isolation](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-3) |
{: caption="Table 1. Related controls for use of reference architecture" caption-side="top"}

## 2. Use only services that are {{site.data.keyword.cloud_notm}} for Financial Services Validated
{: #best-practices-financial-services-validated-services}

**Requirement:** Only use {{site.data.keyword.cloud_notm}} and third-party managed services which are {{site.data.keyword.cloud_notm}} for Financial Services Validated.

**Purpose & value:** {{site.data.keyword.cloud_notm}} for Financial Services Validated designates that an [{{site.data.keyword.cloud_notm}} service](https://cloud.ibm.com/catalog?search=label%3Afs_ready){: external} or ecosystem partner service has evidenced compliance to the controls of the {{site.data.keyword.framework-fs_notm}}. Using services which are Financial Services Validated makes it much easier to meet all of the control requirements and provide evidence of compliance.

An external service is an {{site.data.keyword.cloud_notm}} or third-party service deployed outside of the authorization boundaries of your offering and typically not under direct control of your offering. External services used by an offering should be considered extensions of the offering itself. For an offering to be validated against the requirements of the {{site.data.keyword.framework-fs_notm}}, all of its external services must meet the same requirements.

For cases where you need additional functionality not covered by any Financial Services Validated service, you are advised to install your own software within your deployment. However, you are then responsible for [ensuring the software](#best-practices-self-installed-software) meets all of the {{site.data.keyword.cloud_notm}} controls.

If you represent a technology vendor seeking the Financial Services Validated designation, then you must request special approval for a temporary deviation if you wish to use services that are are not themselves Financial Services Validated. If you represent a financial institution or if the Financial Services Validated designation is not required by your consumers, then you have freedom to choose based on the level of risk you are willing to accept. Regardless of designation, all {{site.data.keyword.cloud_notm}} services are designed with security in mind, and many are certified with other [compliance programs](https://www.ibm.com/cloud/compliance){: external}, such as ISO, SOC 2, etc.
{: important}

**Implementation guidance:**

For technology vendors:

1. For services which are not Financial Services Validated, consult with your {{site.data.keyword.IBM_notm}} customer success representative to determine if it is appropriate for use.
2. If you add services to your solution which are not Financial Services Validated, be sure to update the appropriate parts of the System Environment and Inventory section of your Control Implementation Overview template.

**Most Relevant Controls:**

| Family              | Control                                           |
|---------------------|---------------------------------------------------|
| Access Control (AC) | [AC-20 Use of External Information Systems](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-ac-20) |
| System and Services Acquisition (SA) | [SA-4 Acquisitions Process](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sa-4) \n [SA-9 External Information System Services](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sa-9) |
| Enterprise System and Services Acquisition (ESA) | [ESA-5 Subcontractor Risk Management](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-esa-5) |
| Security Assessment and Authorization (CA) | [CA-3 System Interconnections](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-ca-3) |
{: caption="Table 2. Related controls for using only Financial Services Validated services" caption-side="top"}

## 3. Ensure all deployed software meets all controls
{: #best-practices-self-installed-software}

**Requirement:** Ensure all software used by your service is developed, installed, configured, and managed in accordance with the {{site.data.keyword.framework-fs_notm}} control requirements. It is recommended that you use software that is Financial Services Validated, when available.

**Purpose & value:** Your service will need software to function. This may be software you implement, or it may be software from third-parties, such as databases, logging stacks, bastion hosts, web application firewalls, etc. These are all perfectly acceptable if you adhere to all {{site.data.keyword.framework-fs_notm}} requirements related to development, deployment, configuration, and management of the software.

You will be expected to provide appropriate evidence for all software as part of the validation process. Using software that is already Financial Services Validated will make it easier for you to provide this evidence.

VMware Regulated Workloads reference architecture](/docs/framework-financial-services?topic=framework-financial-services-vmware-day0-account-setup)

-->



## 4. Implement a system of account, identity, and access management to enable a zero-trust environment
{: #best-practices-zero-trust}

**Requirement:** Implement a system of zero-trust where account, identity, and access management are based on the principles of separation of duties and least privilege. This system must include {{site.data.keyword.cloud_notm}} accounts and resources under the control of {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM) as well as supporting accounts and resources outside of {{site.data.keyword.cloud_notm}}.

**Purpose & value:** The design of the {{site.data.keyword.cloud_notm}} for Financial Services is intended to enable a zero-trust model for deployments of the reference architectures. The scope of any one individual operator or administrator is strictly limited to those actions necessary and appropriate to perform their assigned roles. Overlap of duties is minimized or eliminated to prevent the need for privilege escalation that may result in undesired flows of information into or out of your deployment. It is guided by two major principles:

* [Separation of duties](https://csrc.nist.gov/glossary/term/separation_of_duty){: external} - No user should be given enough privileges to misuse the system on their own. For example, no one user should have the authority/ability to develop, compile, and/or move object code from non-production environments into production environments.
* [Least privilege](https://csrc.nist.gov/glossary/term/least_privilege){: external} - Restrict the access privileges of authorized personnel (e.g., program execution privileges, file modification privileges) to the minimum necessary to perform their jobs. Access must be granted based only on a need to know. Access that has not been explicitly permitted is denied by default. Role-based access controls (RBAC) are used to allocate logical access to a specific job function or area of responsibility, rather than to an individual. Administrative access is only granted to individuals whose job roles have been approved for administration responsibilities.

Not only do these principles apply to {{site.data.keyword.cloud_notm}} accounts and resources, but they also apply to accounts and resources that are not managed by {{site.data.keyword.cloud_notm}}. For example, your operators will need credentials and proper authorizations to resources such as (but not limited to):

* Virtual servers in your VPCs
* Self-installed software (such as databases, firewalls, etc.)
* Source code control systems
* External identity providers and user directories
* etc.

In addition, you must provide RBAC for consumer access to the deployed application in the workload plane.

Individuals should be able to request access to resources, and they should be granted access only if it is absolutely required for the them to do their jobs. As individuals leave the organization, change job roles, etc., there needs to be a system to revalidate that the previously granted access continues to be needed for the individuals to perform their duties.

**Implementation guidance:**


* [VPC reference architecture](/docs/framework-financial-services?topic=framework-financial-services-shared-account-setup)
* [{{site.data.keyword.satelliteshort}} reference architecture](/docs/framework-financial-services?topic=framework-financial-services-shared-account-setup)


**Most Relevant Controls:**

| Family              | Control                                           |
|---------------------|---------------------------------------------------|
| Access Control (AC) | [AC-2 Account Management](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-ac-2) \n [AC-3 Access Enforcement](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-ac-3) \n [AC-5 Separation of Duties](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-ac-5) \n [AC-6 Least Privilege](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-ac-6) \n [AC-14 Permitted Actions without Identification or Authentication](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-ac-14) |
| Identification and Authentication (IA) | [IA-5 Authenticator Management](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-ia-5) \n [IA-5 (1) Authenticator Management &#124; Password-Based Authentication](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-ia-5.1) |
{: caption="Table 3. Related controls for zero trust" caption-side="top"}

## 5. Use and maintain non-production environments for development and testing
{: #best-practices-non-production-environments}

**Requirement:** Do not put anything into production that has not been tested in an equivalent non-production environment. Put non-production environments in a separate {{site.data.keyword.cloud_notm}} account. Treat all non-production environments as if they were for production environments by following all of the best practices and {{site.data.keyword.framework-fs_notm}} controls for each environment you create.

**Purpose & value:** Non-production environments (all environments that your service uses for development, testing, or demonstration) are as critical to the security of your service as production environments. Security weaknesses must be detected and mitigated in development environments by enforcing all the same controls and security testing as production. As security weaknesses in development environments may be more likely than production, separation of these environments from production is required.

It is permissible to run a "pre-production" environment in your production account to be used only for final verification and sniff testing before promotion to production -- but, only if the set of operators is the same for both environments. However, all changes still need to be developed and tested first in a non-production environment in a separate account.
{: note}

**Implementation guidance:**

1. Ensure all controls in the {{site.data.keyword.framework-fs_notm}} are followed for each new environment you create, whether for production or not.
2. Create and manage production environments using an {{site.data.keyword.cloud_notm}} account separate from the one you are using for non-production environments.

**Most Relevant Controls:**

| Family              | Control                                           |
|---------------------|---------------------------------------------------|
| Configuration Management (CM) | [CM-3 (2) Configuration Change Control &#124; Testing, Validation, and Documentation Of Changes](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-cm-3.2) \n [CM-4 (1) Impact Analyses &#124; Separate Test Environments](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-cm-4.1) |
| System and Services Acquisition (SA) | [SA-10 Developer Configuration Management](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sa-10) \n [SA-15 (9) Development Process, Standards, and Tools &#124; Use of Live Data](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sa-15.9) |
| System and Communications Protection (SC) | [SC-2 Application Partitioning](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-2) \n [SC-3 Security Function Isolation](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-3) |
{: caption="Table 4. Related controls for non-production environments" caption-side="top"}

## 6. Deploy your {{site.data.keyword.cloud_notm}} resources only to approved multizone regions
{: #best-practices-financial-services-regions}

**Requirement:** Deploy your {{site.data.keyword.cloud_notm}} resources only to multizone regions which are Financial Services Validated.

**Purpose & value:** For all consumer data to stay within the boundary of {{site.data.keyword.cloud_notm}} for Financial Services, you should only create {{site.data.keyword.cloud_notm}} resources in the Financial Services Validated [multizone regions (MZRs)](/docs/overview?topic=overview-locations#mzr-table) listed in [Enabling your account to use Financial Services Validated products](/docs/account?topic=account-enabling-fs-validated):

* Dallas (`us-south`)
* Washington, D.C. (`us-east`)
* Frankfort (`eu-de`)
* London (`eu-gb`)

**Most Relevant Controls:**

| Family              | Control                                           |
|---------------------|---------------------------------------------------|
| Access Control (AC) | [AC-20 Use of External Information Systems](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-ac-20) |
| Security Assessment and Authorization (CA) | [CA-3 System Interconnections](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-ca-3) |
{: caption="Table 5. Related controls for using only approved geographic regions" caption-side="top"}

## 7. Enforce information flow policies and protect the boundaries of your application
{: #best-practices-boundary-protection}

**Requirement:** Leverage the components of the reference architecture to enforce policies for information flows and to protect the boundaries of your application.

**Purpose & value:** Information flow control determines where information is allowed to travel within the components of your service offering as well as to/from other interconnected systems. Information flow control is intended to reduce the risk of sensitive information flowing from one system to another that has less stringent security policies. Flow control is based on the characteristics of the information (e.g., is the content of an information payload valid?) and/or the information path (e.g., is the information coming from a trusted IP address?).  

Boundary protection increases security by monitoring and restricting communications at the external network boundary and key internal boundaries of your service. Boundary protection devices (e.g., gateways, encrypted tunnels, hardware and virtual firewalls, etc.) are commonly employed.

**Implementation guidance:**

* [VPC reference architecture](/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-connectivity-overview)
* [{{site.data.keyword.satelliteshort}} reference architecture](/docs/framework-financial-services?topic=framework-financial-services-satellite-architecture-connectivity-overview)


**Most Relevant Controls:**

| Family              | Control                                           |
|---------------------|---------------------------------------------------|
| Access Control (AC) | [AC-4 Information Flow Enforcement](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-ac-4) \n [AC-4 (5) Information Flow Enforcement &#124; Embedded Data Types](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-ac-4.5) \n [AC-4 (6) Information Flow Enforcement &#124; Metadata](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-ac-4.6) \n [AC-4 (14) Information Flow Enforcement &#124; Security Policy Filter Constraints](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-ac-4.14) \n [AC-4 (21) Information Flow Enforcement &#124; Physical / Logical Separation of Information Flows](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-ac-4.21) \n [AC-20 Use of External Information Systems](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-ac-20) |
| Security Assessment and Authorization (CA) | [CA-3 System Interconnections](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-ca-3) |
| System and Communications Protection (SC)  | [SC-5 Denial of Service Protection](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-5) \n [SC-7 Boundary Protection](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-7) \n [SC-7(4) Boundary Protection &#124; External Telecommunications Services](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-7.4) \n [SC-7 (5) Boundary Protection &#124; Deny By Default - Allow By Exception](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-7.5) \n [SC-7 (10) Boundary Protection &#124; Prevent Exfiltration](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-7.10) \n [SC-8 Transmission Confidentiality and Integrity](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-8) \n [SC-8 (1) Transmission Confidentiality and Integrity &#124; Cryptographic Protection](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-8.1) \n [SC-11 Trusted Path](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-11)  |
{: caption="Table 6. Related controls for boundary protection" caption-side="top"}

## 8. Ensure all operator actions are executed through a bastion host
{: #best-practices-bastion-host}

**Requirement:** Ensure all interactive operator actions can only be run through a bastion host in your dedicated edge or management plane which is properly isolated from your workload plane. Enable recording of bastion sessions for auditing.

**Purpose & value:** A bastion host is a server in your edge or management plane that allows a secure connection to other resources in your edge, management, and workload planes. Using a bastion host for executing operator actions minimizes the chances of penetration from an external source. Session logging allows tracking of all activities that are performed through the bastion host in the event a potential security issue must be investigated.

**Implementation guidance:**

* [VPC reference architecture](/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-connectivity-bastion)
* [{{site.data.keyword.satelliteshort}} reference architecture](/docs/framework-financial-services?topic=framework-financial-services-satellite-architecture-connectivity-bastion)


**Most Relevant Controls:**

| Family              | Control                                           |
|---------------------|---------------------------------------------------|
| Access Control (AC) | [AC-6 (9) Least Privilege &#124; Auditing Use of Privileged Functions](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-ac-6.9) \n [AC-17 Remote Access](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-ac-17) |
| Audit and Accountability (AU) | [AU-14 Session Audit](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-au-14) |
| Identification and Authentication (IA) | [IA-2 (1) Identification and Authentication (Organizational Users) &#124; Network Access to Privileged Accounts](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-ia-2.1) |
{: caption="Table 7. Related controls for bastion host" caption-side="top"}

## 9. Capture audit events and forward to a SIEM
{: #best-practices-audit-logs}

**Requirement:** Capture audit events for actions performed on the components of your service, including {{site.data.keyword.cloud_notm}} services as well as any software components you install within your deployment. Audit logs should be securely stored, and they should also be forwarded to a security information and event management (SIEM) system.

**Purpose & value:** It is important to have audit logs to be able to investigate abnormal activity as well to comply with regulatory audit requirements. With a proper audit logging system, you can be automatically alerted to potential problems. This allows you to be proactive and check into potential security issues shortly after they've occurred.

Your software components (whether written by you or a third-party) should be enabled to emit critical management and data events. Many examples are listed in the AU-2 control from the {{site.data.keyword.framework-fs_notm}}, including:

* Access, downloading, revisions to confidential information
* Access to or update to any financial transaction data
* Any changes to consumer accounts
* Login/logoff event successes and failures
* etc.

**Implementation guidance:**

* [VPC reference architecture](/docs/framework-financial-services?topic=framework-financial-services-shared-logging-audit)
* [{{site.data.keyword.satelliteshort}} reference architecture](/docs/framework-financial-services?topic=framework-financial-services-shared-logging-audit)


**Most Relevant Controls:**

| Family              | Control                                           |
|---------------------|---------------------------------------------------|
| Audit and Accountability (AU) | [AU-1 Audit and Accountability Policy and Procedures) &#124; Network Access to Privileged Accounts](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-au-1) \n [AU-2 Audit Events](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-au-2) \n [AU-3 Content of Audit Records](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-au-3) \n [AU-11 Audit Record Retention](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-au-11) |
| System and Information Integrity (SI) | [SI-4 Information System Monitoring](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-si-4) |
{: caption="Table 8. Related controls for audit logs" caption-side="top"}

## 10. Ensure operational logging and monitoring is implemented
{: #best-practices-operational-logging-and-monitoring}

**Requirement:** Ensure operational logging and monitoring is implemented.

**Purpose & value:** Operational logging and operational monitoring are a very important for ensuring your application runs smoothly. Proper operational logging and monitoring can help you determine if you need to failover to an alternate storage or processing site. In addition, operational logging and monitoring can help you determine if operations have returned to normal after a system disruption.

Operational logging is a complement to [audit logs](#best-practices-audit-logs). Audit logs contain auditable events, while operational logs contain everything else that might be logged. For example, an operational log might contain entries that reflect what's happening as a program executes, such as functions getting called, stack traces getting thrown, etc. While this kind of log data is key to keeping a system running smoothly, it's typically not sent to a SIEM.

Operational logs can be split into two categories:

* Application - Log data generated by software components that you deploy and manage within your deployment. This may be from your own code, or other software components like databases, message queues, etc.
* Platform - Log data from {{site.data.keyword.cloud_notm}} service instances that is not contained in the [audit data sent to {{site.data.keyword.atracker_short}}](/docs/framework-financial-services?topic=framework-financial-services-shared-logging-audit).

Operational monitoring for gauging system health is a very important complement to [monitoring for security and compliance](#best-practices-security-compliance-monitoring). Operational metrics include measurements for CPU usage, memory usage, API response times, etc.



**Most Relevant Controls:**

| Family              | Control                                           |
|---------------------|---------------------------------------------------|
| Contingency Planning (CP) | [CP-2 (3) Contingency Plan &#124; Resume Essential Missions / Business Functions](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-cp-2.3) \n [CP-6 Alternate Storage Site](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-cp-6) \n [CP-7 Alternate Processing Site](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-cp-7) \n [CP-10 Information System Recovery and Reconstitution](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-cp-10)  |
| System and Information Integrity (SI) | [SI-11 Error Handling](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-si-11)    |
{: caption="Table 9. Related controls in {{site.data.keyword.framework-fs_notm}}" caption-side="top"}

## 11. Follow secure development processes and ensure software integrity 
{: #best-practices-development-processes}

**Requirement:** Ensure that security engineering principles are applied in the design, development, implementation, and modification of the system. Maintain software integrity by using signed images, applying security patches, doing vulnerability scans, etc.

**Purpose & value:** It is critical to follow security engineering principles and manage changes to your service using a system development life cycle (SDLC) that incorporates information security considerations. Doing so minimizes the risk of introducing code or configuration changes that undermine your system security.

Lack of software integrity leaves you vulnerable to security problems. So, it is critical for you to be able to assert the origin of all software components in the system and to keep software up to date. In addition, running regular scans on software to detect vulnerabilities allows you to more quickly take corrective action.

**Implementation guidance:**

* [VPC reference architecture](/docs/framework-financial-services?topic=framework-financial-services-shared-development-processes)
* [{{site.data.keyword.satelliteshort}} reference architecture](/docs/framework-financial-services?topic=framework-financial-services-shared-development-processes)


**Most Relevant Controls:**

| Family              | Control                                           |
|---------------------|---------------------------------------------------|
| System and Information Integrity (SI) | [SI-7 Software & Information Integrity](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-si-7)   |
| System and Services Acquisition (SA) | [SA-3 System Development Life Cycle](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sa-3) \n [SA-8 Security Engineering Principles](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sa-8) \n [SA-10 Developer Configuration Management](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sa-10) \n [SA-10 (1) Developer Configuration Management &#124; Software and Firmware Integrity Verification](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sa-10.1) \n [SA-11 Developer Security Testing and Evaluation](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sa-11) \n [SA-15 Development Process, Standards, and Tools](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sa-15) \n [SA-15 (9) Development Process, Standards, and Tools &#124; Use of Live Data](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sa-15.9) |
| Risk Assessment (RA) | [RA-5 Vulnerability Scanning](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-ra-5)  |
{: caption="Table 10. Related controls for development processes" caption-side="top"}

## 12. Encrypt consumer data at rest and in transit
{: #best-practices-encryption}

**Requirement:** Encrypt all consumer data whether at rest or in transit.

**Purpose & value:** Consumers do not want their data to be accessible to bad actors. Protecting consumer data against unauthorized disclosure, modification, or destruction throughout the data lifecycle is of paramount importance in the {{site.data.keyword.cloud_notm}} for Financial Services. Cryptographic controls must be in place in all regions and availability zones to protect the confidentiality and integrity of data.

Data at rest is to always be encrypted, and you must use [{{site.data.keyword.hscrypto}}](/docs/hs-crypto?topic=hs-crypto-overview) to manage encryption keys. {{site.data.keyword.hscrypto}} allows you to take the ownership of the cloud HSM to fully manage your encryption keys and to perform cryptographic operations using [Keep Your Own Key (KYOK)](/docs/hs-crypto?topic=hs-crypto-understand-concepts#kyok-concept). This means you have full control and authority over encryption keys. No one except you (not even {{site.data.keyword.IBM_notm}}) has access to your encryption keys. You should ensure that all Financial Services Validates services which support integration with {{site.data.keyword.hscrypto}} are properly configured.

Data in transit should always be encrypted using TLS 1.2 or higher. This includes _all_ traffic in your deployment such as between virtual server instances and between pods in {{site.data.keyword.openshiftshort}}.

Use {{site.data.keyword.hscrypto}} for TLS offload for all data that is that is requested from outside of {{site.data.keyword.cloud_notm}} (inbound traffic to {{site.data.keyword.cloud_notm}}) and protected by a certificate which is signed by a public Certificate Authority. Configure any web servers to use TLS offload to set up the session, so that the private key never leaves {{site.data.keyword.hscrypto}}.

More detailed guidance can be found in the Cryptographic Requirements appendix of the Control Implementation Overview Template for whichever reference architecture you are using.

**Implementation guidance:**

* [VPC reference architecture](/docs/framework-financial-services?topic=framework-financial-services-shared-encryption-at-rest)
* [{{site.data.keyword.satelliteshort}} reference architecture](/docs/framework-financial-services?topic=framework-financial-services-shared-encryption-at-rest)


**Most Relevant Controls:**

| Family              | Control                                           |
|---------------------|---------------------------------------------------|
| System and Communications Protection (SC) | [SC-8 Transmission Confidentiality and Integrity](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-8) \n [SC-8 (1) Transmission Confidentiality and Integrity &#124; Cryptographic Protection](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-8.1) \n [SC-12 Cryptographic Key Establishment and Management](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-12) \n [SC-12 (2) Cryptographic Key Establishment and Management &#124; Symmetric Keys](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-12.2) \n [SC-12 (3) Cryptographic Key Establishment and Management &#124; Asymmetric Keys](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-12.3) \n [SC-13 Cryptographic Protection](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-13) \n [SC-28 Protection of Information At Rest](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-28) \n [SC-28 (1) Protection of Information at Rest &#124; Cryptographic Protection](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-28.1)  |
{: caption="Table 11. Related controls for data encryption" caption-side="top"}

## 13. Implement business continuity and disaster recovery
{: #best-practices-bcdr}

**Requirement:** Implement a system for business continuity and disaster recovery (BCDR). Define Recovery Point Objective (RPO), Recovery Time Objective (RTO), and Maximum Tolerable Downtime (MTD) metrics for essential business functions. Implement your application so that the metrics are achieved.

**Purpose & value:** Unfortunately, disasters occur that can make some or all of the data centers in a region unavailable. To avoid data loss, you must ensure all important data is backed up to a separate Financial Services Validated multizone region that can be used to restore service. However, your consumers must be able to opt out of having their data stored in the alternate region based on their data residency requirements.

In addition, you must follow best practices for BCDR as defined by the specific managed services that you use. For example, {{site.data.keyword.dl_short}}, {{site.data.keyword.hscrypto}}, etc.

**Implementation guidance:**

* [VPC reference architecture](/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-bcdr)
* [{{site.data.keyword.satelliteshort}} reference architecture](/docs/framework-financial-services?topic=framework-financial-services-satellite-architecture-bcdr)


**Most Relevant Controls:**

| Family              | Control                                           |
|---------------------|---------------------------------------------------|
| Contingency Planning (CP) | [CP-2 Contingency Plan](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-cp-2) \n [CP-6 Alternate Storage Site](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-cp-6) \n [CP-7 Alternate Processing Site](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-cp-7) \n [CP-9 Information System Backup](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-cp-9) \n [CP-10 Information System Recovery and Reconstitution](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-cp-10) \n [CP-10 (2) System Recovery and Reconstitution &#124; Transaction Recovery](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-cp-10.2) |
{: caption="Table 12. Related controls for business continuity and disaster recovery" caption-side="top"}

## 14. Design your application for high availability (recommended)
{: #best-practices-high-availability}

**Requirement:** Design your application for high availability (HA) to minimize downtime for your consumers.

This item is unique among the other best practices in that it represents a recommendation rather than a {{site.data.keyword.framework-fs_notm}} requirement. The {{site.data.keyword.framework-fs_notm}} only requires you to meet the RPO, RTO, and MTD goals that you define (see previous best practice). However, to be best in class, you should strive to be highly available.
{: note}

**Purpose & value:** Consumers will be relying on your service for critical operations and have the potential to be deeply impacted by any downtime. To avoid downtime, you should implement redundancy to eliminate single points of failure. It is strongly recommended that you deploy your application to:

* Multiple availability zones within any Financial Services Validated multizone region you're using. This enables you to ensure if one data center in a region goes down, that your application can continue to function.
* Multiple Financial Services Validated multizone regions with failover between them. If something catastrophic happens that impacts an entire region, you can failover to another so consumers can still use your service.

In addition, it is recommended that you:

* Implement a solution for autoscaling your deployment before capacity is exceeded.
* Follow best practices for HA as defined by the specific managed services that you use. For example, {{site.data.keyword.dl_short}}, {{site.data.keyword.hscrypto}}, etc.

**Implementation guidance:**

* [VPC reference architecture](/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-high-availability)
* [{{site.data.keyword.satelliteshort}} reference architecture](/docs/framework-financial-services?topic=framework-financial-services-satellite-architecture-high-availability)


**Most Relevant Controls:**

| Family              | Control                                           |
|---------------------|---------------------------------------------------|
| Contingency Planning (CP) | [CP-2 Contingency Plan](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-cp-2) \n [CP-7 Alternate Processing Site](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-cp-7) |
| System and Communications Protection (SC) | [SC-6 Resource Availability](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-6) |
{: caption="Table 13. Related controls for high-availability" caption-side="top"}

## 15. Use endpoint detection and remediation (EDR) tooling to detect malicious code
{: #best-practices-endpoint-detection-remediation}

**Requirement:** Use endpoint detection and remediation (EDR) tooling that is effective at detecting malicious code in a cloud environment.

**Purpose & value:** Malicious code includes viruses, worms, Trojan horses, and spyware. The presence of malicious code puts the operation of your service and the customer data you process at risk. It is important to run regular, automated scans of the components of your service to detect these vulnerabilities so that they can be mitigated.

There are many EDR solutions for virtual server instances such as CrowdStrike and Carbon Black. For Kubernetes, relevant tooling includes StackRox, NeuVector, Sysdig Secure, and Twistlock.



**Most Relevant Controls:**

| Family              | Control                                           |
|---------------------|---------------------------------------------------|
| System and Information Integrity (SI) | [SI-3 Malicious Code Protection](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-si-3) |
{: caption="Table 14. Related controls for business continuity and disaster recovery" caption-side="top"}

## 16. Regularly scan for open ports / protocols
{: #best-practices-network-threat-detection}

**Requirement:** Regularly scan for open ports / protocols, and ensure only those ports / protocols needed by your service are open.

**Purpose & value:** As part of the principal of least privilege, you should only open the minimum number of ports / protocols that are needed by your service. Open ports / protocols not needed by your service enable unnecessary threat vectors. So, it is important to regularly scan to make sure the list of open ports / protocols is correct. Tools such as Nmap or Tenable can be used for this purpose.



**Most Relevant Controls:**

| Family              | Control                                           |
|---------------------|---------------------------------------------------|
| Configuration Management (CM) | [CM-7 Least Functionality](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-cm-7) \n [CM-7 (1)Least Functionality &#124; Periodic Review](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-cm-7.1) |
{: caption="Table 15. Related controls for port scanning" caption-side="top"}

## 17. Secure and manage secrets and certificates
{: #best-practices-secrets-management}

**Requirement:** Secrets must be securely protected through their entire lifecycle.

**Purpose & value:** A secret is any piece of data that is sensitive within the context of an application or service. Secrets include all of the following but are not limited to:

* Passwords of any type (database logins, OS accounts, functional IDs, etc.)
* API keys
* Long-lived authentication tokens (OAuth2, github, IAM, etc.)
* SSH keys
* Encryption keys
* Other private keys (PKI/TLS certificates, HMAC keys, signing keys, etc.)

You should should ensure:

* Secrets are generated and stored in the environment (e.g., dev, test, production) where your service is deployed.
* Secrets never leave their environments (e.g., dev, test, production) and should be secured using access control measures. Service design should minimize the number of machines and people with access to secrets using both authorization and network restrictions based on the principle of least privilege.
* Secrets are rotated in according with the requirements of the {{site.data.keyword.framework-fs_notm}} with minimal or no down time.
* Secrets are never stored in source code, configuration files, or documentation.

**Implementation guidance:**

* [VPC reference architecture](/docs/framework-financial-services?topic=framework-financial-services-shared-secrets)
* [{{site.data.keyword.satelliteshort}} reference architecture](/docs/framework-financial-services?topic=framework-financial-services-shared-secrets)

**Most Relevant Controls:**

| Family              | Control                                           |
|---------------------|---------------------------------------------------|
| Access Control (AC) | [AC-2 Account Management](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-ac-2)  |
| Identification and Authentication (IA)  | [IA-2 User Identification and Authentication](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-ia-2) \n [IA-3 Device Identification and Authentication](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-ia-3) \n [IA-5 Authenticator Management](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-ia-5)  |
{: caption="Table 16. Related controls for secrets management" caption-side="top"}

## 18. Tag all {{site.data.keyword.cloud_notm}} resources with security attributes
{: #best-practices-security-attributes}

**Requirement:** Tag all {{site.data.keyword.cloud_notm}} resources with security attributes.

**Purpose & value:** All {{site.data.keyword.cloud_notm}} resources in your account should be tagged with security attributes you define. You can apply user tags to organize your resources and easily find them later, to help you with identifying specific team usage or cost allocation, and to control access to your resources without requiring updates to your IAM policies.

**Implementation guidance:**

* [VPC reference architecture](/docs/framework-financial-services?topic=framework-financial-services-shared-tagging-resources)
* [{{site.data.keyword.satelliteshort}} reference architecture](/docs/framework-financial-services?topic=framework-financial-services-shared-tagging-resources)
VMware Regulated Workloads reference architecture](/docs/framework-financial-services?topic=framework-financial-services-shared-tagging-resources) -->

**Most Relevant Controls:**

| Family              | Control                                           |
|---------------------|---------------------------------------------------|
| Access Control (AC) | [AC-16 Security Attributes](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-ac-16)  |
| System and Communications Protection (SC) | [SC-16 Transmission of Security Attributes](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-16) |
{: caption="Table 17. Related controls for non-production environments" caption-side="top"}

## 19. Monitor for security and compliance against a baseline configuration
{: #best-practices-security-compliance-monitoring}

**Requirement:** Deploy and use tooling for monitoring and reporting of security and compliance. Maintain a baseline configuration for your service that consists of automated mechanisms to facilitate information system baseline management.

{{site.data.content.service-description-scc}}

**Purpose & value:** Continuous monitoring and reporting helps detect malicious activity and security vulnerabilities as early as possible. Automated baseline management makes it much more likely that any compliance-breaking changes to your system configurations are caught early.

**Implementation guidance:**

* [VPC reference architecture](/docs/framework-financial-services?topic=framework-financial-services-shared-monitoring-compliance)
* [{{site.data.keyword.satelliteshort}} reference architecture](/docs/framework-financial-services?topic=framework-financial-services-shared-monitoring-compliance)


**Most Relevant Controls:**

| Family              | Control                                           |
|---------------------|---------------------------------------------------|
| Change Management (CM) | [CM-2 (2) Baseline Configuration &#124; Automation Support for Accuracy and Currency](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-cm-2.2) \n [CM-6 Configuration Settings](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-cm-6) \n [CM-6 (1) Configuration Settings &#124; Automated Management, Application, and Verification](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-cm-6.1)   |
{: caption="Table 18. Related controls for monitoring security and compliance" caption-side="top"}

## Next steps
{: #best-practices-next-steps}

Learn about the three reference architectures for the {{site.data.keyword.cloud_notm}} for Financial Services:

* [VPC reference architecture](/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-about)
* [{{site.data.keyword.satelliteshort}} reference architecture](/docs/framework-financial-services?topic=framework-financial-services-satellite-architecture-about)
* [VMware Regulated Workloads reference architecture](/docs/framework-financial-services?topic=framework-financial-services-vmware-overview)

Or, if you're intending to be a software provider, see also:

* [Best practices for software](/docs/framework-financial-services?topic=framework-financial-services-best-practices-software)


