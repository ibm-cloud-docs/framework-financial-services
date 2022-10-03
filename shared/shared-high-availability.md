---

copyright:
  years: 2020, 2022
lastupdated: "2022-09-30"

keywords: 

subcollection: framework-financial-services

---

{{site.data.keyword.attribute-definition-list}}

# High availability overview
{: #shared-high-availability}

Ensuring high availability (HA) is important for all cloud-based applications, and it is all the more critical for regulated workloads that the {{site.data.keyword.cloud_notm}} for Financial Services is intended to host. In short, consumers depend on your application and cannot afford downtime. Using the VPC reference architecture and following the controls of the {{site.data.keyword.framework-fs_notm}} help you achieve that goal. When building an HA solution in {{site.data.keyword.cloud_notm}}, you need to consider your workloads that run in virtual servers or {{site.data.keyword.openshiftshort}} and the HA characteristics of all of the other {{site.data.keyword.cloud_notm}} services your solution depends on.
{: shortdesc}

## HA for the reference architectures

See the following resources depending on which reference architecture you are using:

- [High availability for VPC reference architecture](/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-high-availability)
- [High availability for {{site.data.keyword.satelliteshort}} reference architecture](/docs/framework-financial-services?topic=framework-financial-services-satellite-architecture-high-availability)

## HA in {{site.data.keyword.cloud_notm}} services
{: #ibm-cloud-services}

Your HA strategy needs to consider all of the {{site.data.keyword.cloud_notm}} services in your deployment. The following table provides references to additional information on HA for each service in the reference architecture.



| Category | VPC reference architecture | {{site.data.keyword.satelliteshort}} reference architecture | Optional for both |
|----------|-------------------|-------------------|-------------------|
| Core  | - [VPC infrastructure services](/vpc?topic=vpc-ha-dr-vpc) [^tabletext] | - [{{site.data.keyword.satelliteshort}}](/docs/satellite?topic=satellite-ha) |  |
| Containers  | - [{{site.data.keyword.openshiftshort}}](/docs/openshift?topic=openshift-ha) \n - [{{site.data.keyword.registryshort}}](/docs/Registry?topic=Registry-ha-dr) | - [{{site.data.keyword.openshiftshort}}](/docs/openshift?topic=openshift-ha) [^tabletext-satellite-enabled-openshift] \n - [{{site.data.keyword.registryshort}}](/docs/Registry?topic=Registry-ha-dr) |  |
| Networking | - [VPC infrastructure services](/vpc?topic=vpc-ha-dr-vpc) \n - [{{site.data.keyword.dl_short}}](/docs/dl?topic=dl-ha-dr) \n - [{{site.data.keyword.tg_short}}](/docs/transit-gateway?topic=transit-gateway-ha-dr#high-availability) |  |  |
| Storage  | - [{{site.data.keyword.block_storage_is_short}}](/vpc?topic=vpc-ha-dr-vpc) \n - [{{site.data.keyword.cos_short}}](/docs/cloud-object-storage?topic=cloud-object-storage-endpoints#endpoints-geo) | - [{{site.data.keyword.cos_short}}](/docs/cloud-object-storage?topic=cloud-object-storage-endpoints#endpoints-geo) |  |
| Security  | - [{{site.data.keyword.hscrypto}}](/docs/hs-crypto?topic=hs-crypto-ha-dr) | - [{{site.data.keyword.hscrypto}}](/docs/hs-crypto?topic=hs-crypto-ha-dr) | - [{{site.data.keyword.appid_short_notm}}](/docs/appid?topic=appid-ha-dr) |
| Logging and monitoring  | - [{{site.data.keyword.atracker_short}}](/docs/activity-tracker?topic=activity-tracker-ha-dr) \n - [{{site.data.keyword.compliance_short}}](/docs/security-compliance?topic=security-compliance-ha-dr) [^tabletext-not-yet-validated] \n - [Flow Logs for VPC](/vpc?topic=vpc-ha-dr-vpc)  | - [{{site.data.keyword.atracker_short}}](/docs/activity-tracker?topic=activity-tracker-ha-dr) \n - [{{site.data.keyword.compliance_short}}](/docs/security-compliance?topic=security-compliance-ha-dr) |  |
| Integration  |  |  | - [{{site.data.keyword.messagehub}}](/docs/EventStreams?topic=EventStreams-sla) |
| Databases  |  |  | - [{{site.data.keyword.cloud}} {{site.data.keyword.ihsdbaas_mongodb_full}}](/docs/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-high-availability-disaster-recovery) \n - [{{site.data.keyword.cloud}} {{site.data.keyword.ihsdbaas_postgresql_full}}](/docs/hyper-protect-dbaas-for-postgresql?topic=hyper-protect-dbaas-for-postgresql-high-availability-disaster-recovery) |
{: caption="Table 1. High availability information for {{site.data.keyword.cloud_notm}} services in the reference architectures" caption-side="top"}

[^tabletext]: {{site.data.content.vpc-infrastructure-services-content}}

[^tabletext-satellite-enabled-openshift]: {{site.data.content.satellite-enabled-openshift}}

[^tabletext-not-yet-validated]: {{site.data.content.not-yet-validated}}

The references cover the steps that the service has taken to achieve high availability as well as the steps you must take. For example, consider {{site.data.keyword.dl_short}}. The {{site.data.keyword.dl_short}} service itself makes use of multiple zones within a region to help achieve HA. However, you {{site.data.keyword.dl_short}} is not a redundant service at the cross-connect router (XCR), so you have the responsibility for creating redundancy through your border gateway protocol (BGP) schemas. See [High availability and disaster recovery for {{site.data.keyword.dl_short}}](/docs/dl?topic=dl-ha-dr) and [Models for diversity and redundancy in {{site.data.keyword.dl_short}}](/docs/dl?topic=dl-models-for-diversity-and-redundancy-in-direct-link) for more details.

In addition to the Financial Services Validated services in the reference architecture, see the following references for other important information:

* [How {{site.data.keyword.cloud_notm}} ensures high availability and disaster recovery](/docs/overview?topic=overview-zero-downtime)
* [Responsibilities for operating services in your deployment](/docs/framework-financial-services?topic=framework-financial-services-shared-responsibilities)

## Related controls in {{site.data.keyword.framework-fs_notm}}
{: #related-controls}

The following {{site.data.keyword.framework-fs_notm}} controls are most related to this guidance. However, in addition to following the guidance here, ydo your own due diligence to ensure you meet the requirements.

| Family              | Control                                           |
|---------------------|---------------------------------------------------|
| Contingency Planning (CP) | [CP-2 Contingency Plan](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-cp-2) \n [CP-7 Alternate Processing Site](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-cp-7) |
| System and Communications Protection (SC) | [SC-6 Resource Availability](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-6) |
{: caption="Table 2. Related controls in {{site.data.keyword.framework-fs_notm}}" caption-side="top"}

## Next steps
{: #next-steps}

* [Development processes and software integrity](/docs/framework-financial-services?topic=framework-financial-services-shared-development-processes)
