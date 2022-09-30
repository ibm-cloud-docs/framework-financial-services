---

copyright:
  years: 2020, 2022
lastupdated: "2022-09-30"

keywords: 

subcollection: framework-financial-services

---

{{site.data.keyword.attribute-definition-list}}

# Business continuity and disaster recovery overview
{: #shared-bcdr}

Business continuity and disaster recovery (BCDR) is important for all cloud-based applications, and it is all the more critical for the kind of regulated workloads that the {{site.data.keyword.cloud_notm}} for Financial Services is intended to host. In the event of a disaster, consumers depend on service being restored quickly without loss of data. Using the VPC reference architecture and following the controls of the {{site.data.keyword.framework-fs_notm}} help you achieve that goal. When building a BCDR solution in {{site.data.keyword.cloud_notm}}, you need to consider your workloads that run in virtual servers or {{site.data.keyword.openshiftshort}} and the BCDR characteristics of all of the other {{site.data.keyword.cloud_notm}} services your solution depends on.
{: shortdesc}

## Requirements
{: #your-workloads-requirements}

The following sections describe some of the requirements that you must follow for BCDR of your workloads.

### Alternative storage site 
{: #your-workloads-requirements-alternate-storage-site}

You must establish and configure an alternative storage site in at least one geographically separate {{site.data.keyword.cloud_notm}} region. So, if the storage in one region becomes unavailable, you can use storage from another region.

See [Deploy your {{site.data.keyword.cloud_notm}} resources only to approved multizone regions](/docs/framework-financial-services?topic=framework-financial-services-best-practices#best-practices-financial-services-regions) for the list of Financial Services Validated regions. You can choose any of them to act as an alternative storage site. However, your consumers must be able to opt out of having their data stored in the alternate region based on their data residency requirements.
{: tip}



### Information system backup
{: #your-workloads-requirements-information-system-backup}

As the provider, you must:

* Conduct backups of user-level information contained in the information system
* Conduct backups of system-level information contained in the information system
* Conduct backups of information system documentation, including security-related documentation
* Protect the confidentiality, integrity, and availability of backup information at storage locations  

The different levels of information are defined as follows:

* User-level information - Consumer/consumer-managed data
* System-level information - Data you manage that is necessary to meet your service's Recovery Point Objective (RPO). For example, system-state information, operating system and application software, licenses, and so on.
* System documentation - External documentation, internal architecture documentation, run books, and so on.

Other requirements:

* Backups must be at least incremental daily and full weekly.
* Backups must be monitored. Failed backups must be investigated and corrective action that is documented as necessary to maintain the service's RPO.
* Backups and backup logs should be accessible only to authorized individuals who need access to do their jobs.
* Transaction-based systems must implement transaction recovery. Transaction-based systems include database management systems and transaction processing systems. Mechanisms supporting transaction recovery include transaction rollback and transaction journaling.

### Information system recovery and reconstitution
{: #your-workloads-requirements-information-system-recovery}

You should enable recovery and reconstitution of the system to a known state after a disruption, compromise, or failure. Contingency plans must include procedures for validating successful recovery and reconstitution.  Recovery and reconstitution activities include resuming operational capabilities at the original location.

## Backup and disaster recovery for the reference architectures
{: #bcdr-for-reference-architectures}

See the following resources depending on which reference architecture you are using:

- [Business continuity and disaster recovery for VPC reference architecture](/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-bcdr)
- [Business continuity and disaster recovery for {{site.data.keyword.satelliteshort}} reference architecture](/docs/framework-financial-services?topic=framework-financial-services-satellite-architecture-bcdr)

## Backup and disaster recovery for {{site.data.keyword.cloud_notm}} services
{: #ibm-cloud-services}

In addition to backing up your workloads and having the capability to restore service in the face of a disaster, your BCDR strategy needs to consider all of the {{site.data.keyword.cloud_notm}} services in your deployment. The following table provides references to additional information regarding BCDR for each service in the reference architecture.

The following table provides references for more information about BCDR for each service in the reference architecture.

| Category | VPC reference architecture | {{site.data.keyword.satelliteshort}} reference architecture | Optional for both |
|----------|-------------------|-------------------|-------------------|
| Core  | - [VPC infrastructure services](/vpc?topic=vpc-ha-dr-vpc) [^tabletext] | - [{{site.data.keyword.satelliteshort}}](/docs/satellite?topic=satellite-ha) |  |
| Containers  | - [{{site.data.keyword.openshiftshort}}](/docs/openshift?topic=openshift-ha) \n - [{{site.data.keyword.registryshort}}](/docs/Registry?topic=Registry-ha-dr) | - [{{site.data.keyword.openshiftshort}}](/docs/openshift?topic=openshift-ha) [^tabletext-satellite-enabled-openshift] \n - [{{site.data.keyword.registryshort}}](/docs/Registry?topic=Registry-ha-dr) |  |
| Networking  | - [VPC infrastructure services](/vpc?topic=vpc-ha-dr-vpc) \n - [{{site.data.keyword.dl_short}}](/docs/dl?topic=dl-ha-dr) \n - [{{site.data.keyword.tg_short}}](/docs/transit-gateway?topic=transit-gateway-ha-dr#disaster-recovery)  |  |  |
| Storage  | - [{{site.data.keyword.block_storage_is_short}}](/vpc?topic=vpc-ha-dr-vpc) \n - [{{site.data.keyword.cos_short}}](/docs/cloud-object-storage?topic=cloud-object-storage-endpoints#endpoints-geo) | - [{{site.data.keyword.cos_short}}](/docs/cloud-object-storage?topic=cloud-object-storage-endpoints#endpoints-geo) |  |
| Security  | - [{{site.data.keyword.hscrypto}}](/docs/hs-crypto?topic=hs-crypto-ha-dr#cross-region-disaster-recovery)  | - [{{site.data.keyword.hscrypto}}](/docs/hs-crypto?topic=hs-crypto-ha-dr#cross-region-disaster-recovery)  | - [{{site.data.keyword.appid_short_notm}}](/docs/appid?topic=appid-ha-dr)  |
| Logging and monitoring  | - [{{site.data.keyword.atracker_short}}](/docs/activity-tracker?topic=activity-tracker-ha-dr) \n - [{{site.data.keyword.compliance_short}}](/docs/security-compliance?topic=security-compliance-ha-dr) [^tabletext-not-yet-validated] \n - [Flow Logs for VPC](/vpc?topic=vpc-ha-dr-vpc)  | - [{{site.data.keyword.atracker_short}}](/docs/activity-tracker?topic=activity-tracker-ha-dr) \n - [{{site.data.keyword.compliance_short}}](/docs/security-compliance?topic=security-compliance-ha-dr) |  |
| Integration  |  |  | - [{{site.data.keyword.messagehub}}](/docs/EventStreams?topic=EventStreams-disaster_recovery_scenario) |
| Databases  |  |  | - [{{site.data.keyword.ihsdbaas_mongodb_full}}](/docs/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-high-availability-disaster-recovery) \n - [{{site.data.keyword.ihsdbaas_postgresql_full}}](/docs/hyper-protect-dbaas-for-postgresql?topic=hyper-protect-dbaas-for-postgresql-high-availability-disaster-recovery) |
{: caption="Table 1. Backup and disaster recovery information for {{site.data.keyword.cloud_notm}} services in the reference architectures" caption-side="top"}

[^tabletext]: {{site.data.content.vpc-infrastructure-services-content}}

[^tabletext-satellite-enabled-openshift]: {{site.data.content.satellite-enabled-openshift}}

[^tabletext-not-yet-validated]: {{site.data.content.not-yet-validated}}

In addition to the Financial Services Validated services in the reference architecture, see the following references for other important information:

* [How {{site.data.keyword.cloud_notm}} ensures high availability and disaster recovery](/docs/overview?topic=overview-zero-downtime)
* [High availability](/docs/framework-financial-services?topic=framework-financial-services-shared-high-availability) in the VPC reference architecture
* [Responsibilities for operating services in your deployment](/docs/framework-financial-services?topic=framework-financial-services-shared-responsibilities) of the VPC reference architecture

## Related controls in {{site.data.keyword.framework-fs_notm}} 
{: #related-controls}

{{site.data.content.related-controls-disclaimer}}


| Family              | Control                                           |
|---------------------|---------------------------------------------------|
| Contingency Planning (CP) | [CP-2 Contingency Plan](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-cp-2) \n [CP-6 Alternate Storage Site](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-cp-6) \n [CP-7 Alternate Processing Site](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-cp-7) \n [CP-9 Information System Backup](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-cp-9) \n [CP-10 Information System Recovery and Reconstitution](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-cp-10) \n [CP-10 (2) System Recovery and Reconstitution &#124; Transaction Recovery](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-cp-10.2)  |
{: caption="Table 2. Related controls in {{site.data.keyword.framework-fs_notm}}" caption-side="top"}

## Next steps
{: #next-steps}

* [Development processes and software integrity](/docs/framework-financial-services?topic=framework-financial-services-shared-development-processes)
