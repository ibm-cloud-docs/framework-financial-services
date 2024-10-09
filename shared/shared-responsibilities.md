---

copyright:
  years: 2020, 2024
lastupdated: "2024-10-09"

keywords: 

subcollection: framework-financial-services

---

{{site.data.keyword.attribute-definition-list}}

# Responsibilities for operating services in your deployment
{: #shared-responsibilities}

In {{site.data.keyword.cloud_notm}}, the responsibilities for deploying, operating, and securing products are shared between {{site.data.keyword.IBM_notm}} and you, the application provider. It is important for you to understand these responsibilities for every {{site.data.keyword.cloud_notm}} service you've deployed as part of the reference architectures.
{: shortdesc}

Depending on the product, the responsibility for the following types of tasks (which span activities for Day 0, Day 1, and Day 2) can be exclusive to {{site.data.keyword.IBM_notm}}, you, or shared. The tasks for each type of product are grouped in the following categories, which cut across nearly all of the {{site.data.keyword.framework-fs_notm}}'s [best practices and requirements](/docs/framework-financial-services?topic=framework-financial-services-best-practices).

Incident and operations management
:   Includes tasks such as monitoring, event management, high availability, problem determination, recovery, and full state backup and recovery.

Change management
:   Includes tasks such as deployment, configuration, upgrades, patching, configuration changes, and deletion.

Identity and access management
:   Includes tasks such as authentication, authorization, access control policies, and approving, granting, and revoking access.

Security and regulation compliance
:   Includes tasks such as security controls implementation and compliance certification.

Disaster recovery
:   Includes tasks such as providing dependencies on disaster recovery sites, provision disaster recovery environments, data and configuration backup, replicating data and configuration to the disaster recovery environment, and failover on disaster events.

For more information, see [Shared responsibilities for using {{site.data.keyword.cloud_notm}} products](/docs/overview/terms-of-use?topic=overview-shared-responsibilities).

## Responsibilities for Financial Services Validated services
{: #related-resources}

The following table provides references that describe your responsibilities for each Financial Services Validated service in the reference architecture.

| Category | VPC reference architecture | {{site.data.keyword.satelliteshort}} reference architecture | Optional for both |
|----------|-------------------|-------------------|-------------------|
| Core  | - [VPC infrastructure services](/docs/vpc?topic=vpc-responsibilities-vpc) [^tabletext] | - [{{site.data.keyword.satelliteshort}}](/docs/satellite?topic=satellite-responsibilities) |  |
| Containers  | - [{{site.data.keyword.openshiftshort}}](/docs/openshift?topic=openshift-responsibilities_iks) \n - {{site.data.keyword.registryshort}} [^tabletext-no-specific-link-container-registry] | - [{{site.data.keyword.openshiftshort}}](/docs/openshift?topic=openshift-responsibilities_iks) [^tabletext-satellite-enabled-openshift] \n - {{site.data.keyword.registryshort}} |  |
| Networking | - [VPC infrastructure services](/docs/vpc?topic=vpc-responsibilities-vpc) \n - [{{site.data.keyword.dl_short}}](/docs/dl?topic=dl-dl-responsibilities) \n - [{{site.data.keyword.tg_short}}](/docs/transit-gateway?topic=transit-gateway-tg-responsibilities) | | |
| Storage  | - [{{site.data.keyword.block_storage_is_short}}](/docs/vpc?topic=vpc-responsibilities-vpc) \n - [{{site.data.keyword.cos_short}}](/docs/cloud-object-storage?topic=cloud-object-storage-responsibilities) | - [{{site.data.keyword.cos_short}}](/docs/cloud-object-storage?topic=cloud-object-storage-responsibilities) |  |
| Security  | - [{{site.data.keyword.hscrypto}}](/docs/hs-crypto?topic=hs-crypto-shared-responsibilities) | - [{{site.data.keyword.hscrypto}}](/docs/hs-crypto?topic=hs-crypto-shared-responsibilities) | - {{site.data.keyword.appid_short_notm}} [^tabletext-no-specific-link-appid] |
| Logging and monitoring  | - [{{site.data.keyword.atracker_short}}](/docs/activity-tracker?topic=activity-tracker-shared-responsibilities) \n - {{site.data.keyword.compliance_short}} \n - [{{site.data.keyword.fl_full}}](/docs/vpc?topic=vpc-responsibilities-vpc)  | - [{{site.data.keyword.atracker_short}}](/docs/activity-tracker?topic=activity-tracker-shared-responsibilities) \n - {{site.data.keyword.compliance_short}} |  |
| Integration  | |  | - [{{site.data.keyword.messagehub}}](/docs/EventStreams?topic=EventStreams-event_streams_responsibilities) |
{: caption="Your responsibilities for {{site.data.keyword.cloud_notm}} services in the reference architectures" caption-side="top"}

[^tabletext]: {{site.data.content.vpc-infrastructure-services-content}}

[^tabletext-satellite-enabled-openshift]: {{site.data.content.satellite-enabled-openshift}}

[^tabletext-no-specific-link-appid]: There is not a specific shared responsibility article for {{site.data.keyword.appid_short_notm}}.

[^tabletext-no-specific-link-container-registry]: There is not a specific shared responsibility article for {{site.data.keyword.registryshort}}.
