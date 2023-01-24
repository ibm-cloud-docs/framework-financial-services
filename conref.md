---

copyright:
  years: 2020, 2023
lastupdated: "2023-01-24"

keywords: 

subcollection: overview

content-type: conref

---

{{site.data.keyword.attribute-definition-list}}



# Content references for framework-financial-services
{: #conref-framework-financial-services}

Generally speaking, you should strive to use only services which are Financial Services Validated in your solutions. However, depending on your circumstance there may be exceptions. See the best practice [Use only services that are {{site.data.keyword.cloud_notm}} for Financial Services Validated](/docs/framework-financial-services?topic=framework-financial-services-best-practices#best-practices-financial-services-validated-services) for more details and potential exceptions.
{: #fs-validated-disclaimer}

Generally speaking, you should strive to use only services which are Financial Services Validated in your solutions. However, depending on your circumstance there may be exceptions. See the best practice [Use only services that are {{site.data.keyword.cloud_notm}} for Financial Services Validated](/docs/framework-financial-services?topic=framework-financial-services-best-practices#best-practices-financial-services-validated-services) for more details and potential exceptions.
{: important}
{: #fs-validated-disclaimer-important}

Installing your own software is recommended when there is not yet an {{site.data.keyword.cloud_notm}} service that is Financial Services Validated. However, when installing your own software you are still responsible for the controls of the {{site.data.keyword.framework-fs_notm}} if you are seeking your own Financial Services Validated designation.
{: #byo-software-disclaimer}

Only the event routing features of {{site.data.keyword.at_short}} have been Financial Services Validated.
{: #event-routing-fs-validation}

Not yet Financial Services Validated.
{: #not-yet-validated}

Only required if using virtual servers instead of or in addition to {{site.data.keyword.openshiftshort}}.
{: #only-required-virtual-servers}

Highly recommended if using virtual servers instead of or in addition to {{site.data.keyword.openshiftshort}}.
{: #highly-recommended-with-virtual-servers}

Only required if using containers instead of or in addition to virtual servers.
{: #only-required-openshift}

Only choose one of {{site.data.keyword.dl_short}} and {{site.data.keyword.vpn_vpc_short}}.
{: #only-one-of-dl-or-vpc}

Includes VPC, Dedicated hosts for VPC, Auto Scale for VPC, {{site.data.keyword.alb_full}}, {{site.data.keyword.vpn_vpc_short}}, {{site.data.keyword.dns_short}}, and Virtual Private Endpoints for VPC.
{: #vpc-infrastructure-services-content}



## Service descriptions

### Red Hat OpenShift on IBM Cloud

{{site.data.keyword.openshiftshort}} is a managed offering to create your own {{site.data.keyword.openshiftshort}} cluster of compute hosts to deploy and manage containerized apps on {{site.data.keyword.cloud_notm}}. {{site.data.keyword.openshiftlong_notm}} provides intelligent scheduling, self-healing, horizontal scaling, service discovery and load balancing, automated rollouts and rollbacks, and secret and configuration management for your apps. Combined with an intuitive user experience, built-in security and isolation, and advanced tools to secure, manage, and monitor your cluster workloads, you can rapidly deliver highly available and secure containerized apps in the public cloud.
{: #service-description-openshift}

{{site.data.keyword.satelliteshort}}-enabled service which runs in your {{site.data.keyword.satelliteshort}} location.
{: #satellite-enabled-openshift}

### Container Registry

Use [{{site.data.keyword.registryshort}}](/docs/Registry?topic=Registry-registry_overview) to store and access private container images. {{site.data.keyword.registryshort}} provides a multi-tenant, highly available, scalable, and encrypted private image registry that is hosted and managed by {{site.data.keyword.IBM}}. You can use {{site.data.keyword.registryshort}} by setting up your own image namespace and pushing container images to your namespace. This service is required if using {{site.data.keyword.openshiftshort}}.
{: #service-description-container-registry}

### Virtual Private Endpoint

VPE is an evolution of the private connectivity to {{site.data.keyword.cloud_notm}} services. VPEs are virtual IP interfaces that are bound to an endpoint gateway created on a per service, or service instance, basis (depending on the service operation model). The endpoint gateway is a virtualized function that scales horizontally, is redundant and highly available, and spans all availability zones of your VPC. Endpoint gateways enable communications from virtual server instances within your VPC and {{site.data.keyword.cloud_notm}} service on the private backbone. VPE for VPC gives you the experience of controlling all the private addressing within your cloud.
{: #service-description-vpe}

### Direct Link

Use [{{site.data.keyword.dl_short}}](/docs/dl?topic=dl-dl-about) to seamlessly connect your on-premises resources to your cloud resources. The speed and reliability of {{site.data.keyword.dl_short}} extends your organization’s data center network and offers more consistent, higher-throughput connectivity, keeping traffic within the {{site.data.keyword.cloud_notm}} network. {{site.data.keyword.dl_short}} is the most secure way to enable connectivity from on-premises environments to {{site.data.keyword.cloud_notm}}.
{: #service-description-direct-link}

### Cloud Object Storage

[{{site.data.keyword.cos_short}}](/docs/cloud-object-storage?topic=cloud-object-storage-about-cloud-object-storage) stores encrypted and dispersed data across multiple geographic locations. {{site.data.keyword.cos_short}} is available with three types of resiliency: Cross Region, Regional, and Single Data Center. Cross Region provides higher durability and availability than using a single region at the cost of slightly higher latency. Regional service reverses those tradeoffs, and distributes objects across multiple availability zones within a single region. If a given region or availability zone is unavailable, the object store continues to function without impediment. Single Data Center distributes objects across multiple machines within the same physical location.
{: #service-description-cloud-object-storage-1}

Users of {{site.data.keyword.cos_short}} refer to their binary data, such as files, images, media, archives, or even entire databases as objects. Objects are stored in a bucket, the container for their unstructured data. Buckets contain both inherent and user-defined metadata. Finally, objects are defined by a globally unique combination of the bucket name and the object key, or name.
{: #service-description-cloud-object-storage-2}

All {{site.data.keyword.cos_short}} buckets must be encrypted with KYOK by using keys that are managed by {{site.data.keyword.hscrypto}}. For more information, see [Encryption at rest](/docs/framework-financial-services?topic=framework-financial-services-shared-encryption-at-rest). In addition, a geographically separate region should be used as an [alternative storage site](/docs/framework-financial-services?topic=framework-financial-services-shared-bcdr#your-workloads-requirements-alternate-storage-site). This means you should use [cross region resiliency](/docs/cloud-object-storage?topic=cloud-object-storage-endpoints#endpoints-geo) for all of your {{site.data.keyword.cos_short}} buckets.
{: important}
{: #service-description-cloud-object-storage-use-cross-region-kyok-important}

To start working with {{site.data.keyword.cos_short}}, see the following instructions:
{: #service-description-cloud-object-storage-reference-list-intro}

- [Getting started with {{site.data.keyword.cos_short}}](/docs/cloud-object-storage?topic=cloud-object-storage-getting-started-cloud-object-storage)
- [Connecting to {{site.data.keyword.cos_short}} from VPC](/docs/vpc?topic=vpc-connecting-vpc-cos)
- [Encrypting your data](/docs/cloud-object-storage?topic=cloud-object-storage-encryption)
{: #service-description-cloud-object-storage-reference-list}

### Hyper Protect Crypto Services

[{{site.data.keyword.hscrypto}}](/docs/hs-crypto?topic=hs-crypto-overview) is a dedicated key management service and hardware security module (HSM) based on {{site.data.keyword.cloud_notm}}. This service allows you to take the ownership of the cloud HSM to fully manage your encryption keys and to perform cryptographic operations using Keep Your Own Key (KYOK). {{site.data.keyword.hscrypto}} is also the only service in the cloud industry that is built on FIPS 140-2 Level 4-certified hardware.
{: #service-description-hpcs}

### App ID

[{{site.data.keyword.appid_short_notm}}](/docs/appid?topic=appid-about) helps developers to easily add authentication to their web and mobile apps with few lines of code, and secure their cloud-native applications and services on {{site.data.keyword.cloud_notm}}.
{: #service-description-app-id}

### Activity Tracker event routing

[{{site.data.keyword.atracker_short}}](/docs/activity-tracker?topic=activity-tracker-getting-started-routing&interface=cli) is used to collect auditable platform events that are generated by services in your {{site.data.keyword.cloud_notm}} account. These events allow you to monitor the activity of your {{site.data.keyword.cloud_notm}} account so that you can investigate abnormal activity and critical actions.
{: #service-description-activity-tracker-event-routing-1}

{{site.data.keyword.atracker_short}} provides for either event routing or [hosted event search](/docs/activity-tracker?topic=activity-tracker-getting-started-search). However, only the event routing features of {{site.data.keyword.atracker_short}} are Financial Services Validated. In regions where it's available, you must configure {{site.data.keyword.atracker_short}} event routing to send events to {{site.data.keyword.cos_short}}, where they must be encrypted with KYOK.
{: #service-description-activity-tracker-event-routing-2}

{{site.data.keyword.atracker_short}} event routing is only available in some regions (see [Locations for {{site.data.keyword.atracker_short}} event routing](/docs/activity-tracker?topic=activity-tracker-regions#regions-atracker) for more details). For regions where it's not available, you must use {{site.data.keyword.atracker_short}} hosted event search until {{site.data.keyword.atracker_short}} event routing is available. When event routing becomes available in those regions, you must switch to use event routing. For more information and possible exceptions, see [Use only services that are {{site.data.keyword.cloud_notm}} for Financial Services Validated](/docs/framework-financial-services?topic=framework-financial-services-best-practices#best-practices-financial-services-validated-services).
{: important}
{: #service-description-activity-tracker-event-routing-3-important}

### Event Streams

[{{site.data.keyword.messagehub}}](/docs/EventStreams?topic=EventStreams-about) is a high-throughput message bus built with Apache Kafka. It is optimized for event ingestion into {{site.data.keyword.cloud_notm}} and event stream distribution between your services and applications.
{: #service-description-event-streams-1}

You can use {{site.data.keyword.messagehub}} to complete the following tasks:
{: #service-description-event-streams-2}

- Offload work to back-end worker applications.
- Connect event streams to streaming analytics to realize powerful insights.
- Publish event data to multiple applications to react in real time.
{: #service-description-event-streams-3-unordered-list}

{{site.data.keyword.messagehub}} offers a fully managed Apache Kafka service, ensuring durability and high availability for our clients. By using {{site.data.keyword.messagehub}}, you have support around the clock from our team of Kafka experts.
{: #service-description-event-streams-4}

### Hyper Protect DBaaS for MongoDB

Moving confidential and mission-critical data to the cloud presents data confidentiality, security, and reliability concerns. [{{site.data.keyword.cloud}} {{site.data.keyword.ihsdbaas_mongodb_full}}](/docs/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-overview) offers highly secure database environments that have technology-enforced protection and high availability.
{: #service-description-ihsdbaas-mongodb-1}

Built on {{site.data.keyword.IBM_notm}} LinuxONE technology, {{site.data.keyword.ihsdbaas_mongodb_full}} helps you to alleviate data security and compliance concerns with built-in encryption and tamper protection for data at rest and in flight. You can deploy your workloads with sensitive data and build compliant applications without having to be a security expert.
{: #service-description-ihsdbaas-mongodb-2}

### Hyper Protect DBaaS for PostgreSQL

Moving confidential and mission-critical data to the cloud presents data confidentiality, security, and reliability concerns. [{{site.data.keyword.cloud}} {{site.data.keyword.ihsdbaas_postgresql_full}}](/docs/hyper-protect-dbaas-for-postgresql?topic=hyper-protect-dbaas-for-postgresql-overview) offers highly secure database environments that have technology-enforced protection and high availability.
{: #service-description-ihsdbaas-postgresql-1}

Built on {{site.data.keyword.IBM_notm}} LinuxONE technology, {{site.data.keyword.ihsdbaas_postgresql_full}} helps you to alleviate data security and compliance concerns with built-in encryption and tamper protection for data at rest and in flight. You can deploy your workloads with sensitive data and build compliant applications without having to be a security expert.
{: #service-description-ihsdbaas-postgresql-2}

### Security and Compliance Center

With [{{site.data.keyword.compliance_full}}](/docs/security-compliance?topic=security-compliance-getting-started) you can embed security checks into your every day workflows to help monitor for security and compliance. By monitoring for risks, you can identify security vulnerabilities and quickly work to mitigate the impact and fix the issue. By using {{site.data.keyword.compliance_short}} along with [external integrations](/security-compliance/integrations) (such as, OpenShift Compliance Operator (OSCO), Tanium, NeuVector, and so on), you can build a robust approach for monitoring for security and compliance issues.
{: #service-description-scc}

## Bastion host requirements

- Full session recording of actions executed via the bastion to perform session audits. This includes SSH, and Kubernetes exec (`kubectl exec -it`) sessions.
    - Linux-based session recordings are captured as a raw dump of stdout and stderr streams, including TAB characters (bash escape sequences).
    - Session recordings should be placed in to a storage service maintained within your environment for archiving. The storage should be configured as “immutable object storage.” Retention policies are should be applied to the storage, so that data is stored in a WORM (Write-Once-Read-Many), non-erasable and non-rewritable manner. The policy is set and enforced for a 12-month (minimum) retention period.
- Authenticating users with MFA using a physical hardware-based security key that generates a six-digit numerical code. A smart card or hardware token designed and operated to FIPS 140-2 level 2 or above or equivalent (e.g., ANSI X9.24, ISO 13491-1:2007) is recommended.
- Locking user accounts after three (3) consecutive failed logon attempts within 15 minutes.
- Locking user accounts for 30 minutes when there have been more than three unsuccessful logon attempts. After the lockout period ends, the user will be able to reset their password. Internal privileged accounts must remain locked until released by an administrator.
- Session timeout after 15 minutes of inactivity.
- Providing a system use notification banner from either the bastion or the target system. The warning banner is displayed before the system grants access to the user and the usage conditions must be approved before proceeding. The banner will provide privacy and security notices consistent with applicable customer policies, regulations, standards, and guidance. The warning banner will state that:
    - Users are accessing a financial services information system.
    - Information system usage may be monitored, recorded, and subject to audit.
    - Unauthorized use of the information system is prohibited and subject to criminal and civil penalties.
    - Use of the information system indicates consent to monitoring and recording.
{: #bastion-host-requirements}

## Encryption at rest in Satellite location

You are responsible for providing the underlying virtual and physical storage that will be accessed by {{site.data.keyword.satelliteshort}} storage in your on-premises {{site.data.keyword.satelliteshort}} location. You should ensure your underlying physical storage is encrypted using keys managed by an on-premises FIPS 140-2 level 2 or higher hardware security module (HSM).
{: important}
{: #encryption-at-rest-in-satellite-location-important}

## Related controls disclaimer

The following [{{site.data.keyword.framework-fs_notm}} controls](/docs/framework-financial-services?topic=framework-financial-services-about#framework-control-requirements) are most related to this guidance. However, in addition to following the guidance here, do your own due diligence to ensure you meet the requirements.
{: #related-controls-disclaimer}

## Beta document message

This document is part of the at the beta release level, and still undergoing some/review revision for the {{site.data.keyword.satelliteshort}} release. Feedback is welcome.
{: beta}
{: #beta-document-disclaimer}

## Satellite shared responsibilities

Responsibility is shared, except that the workload provider is solely responsible for [Disaster Recovery](/docs/satellite?topic=satellite-responsibilities#disaster-recovery).
{: #workload-provider-responsibilities-link}

Responsibility is shared, except that the workload provider is solely responsible for [Identity and access management](/docs/satellite?topic=satellite-responsibilities#iam-responsibilities).
{: #workload-provider-responsibilities-storage}

Responsibility is shared, except that the workload provider is solely responsible for [Change management](/docs/satellite?topic=satellite-responsibilities#change-management) and [Security and regulation compliance](/docs/satellite?topic=satellite-responsibilities#security-compliance).
{: #workload-provider-responsibilities-operating-system}

# List reference architectures with full names

* [{{site.data.keyword.vpc_full}} reference architecture](/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-about), with options for using one or both of:
    * [{{site.data.keyword.vsi_is_full}}](/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-detailed-vsi)
    * [{{site.data.keyword.openshiftlong}}](/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-detailed-openshift)
* [{{site.data.keyword.satellitelong}} reference architecture](/docs/framework-financial-services?topic=framework-financial-services-satellite-architecture-about) 
* [{{site.data.keyword.cloud}} for VMware® Regulated Workloads reference architecture](/docs/framework-financial-services?topic=framework-financial-services-vmware-overview)
{: #reference-architectures-list-full-names}

