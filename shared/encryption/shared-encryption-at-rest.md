---

copyright:
  years: 2020, 2023
lastupdated: "2023-01-24"

keywords: 

subcollection: framework-financial-services

---

{{site.data.keyword.attribute-definition-list}}

# Data encryption at rest
{: #shared-encryption-at-rest}

Protecting data against unauthorized disclosure, modification or destruction throughout the data lifecycle is of paramount importance in the {{site.data.keyword.cloud_notm}} for Financial Services. Cryptographic controls must be in place in all regions and availability zones to protect the confidentiality and integrity of data. Data at rest is to always be encrypted using your keys.
{: shortdesc}

## Data encryption at rest in {{site.data.keyword.cloud_notm}}
{: #shared-encryption-at-rest-ibm-cloud}

Within {{site.data.keyword.cloud_notm}}, you must use [{{site.data.keyword.hscrypto}}](/docs/hs-crypto?topic=hs-crypto-overview) to manage encryption keys. {{site.data.keyword.hscrypto}} is a dedicated key management service and hardware security module (HSM). {{site.data.keyword.hscrypto}} is the only service in the cloud industry that is built on FIPS 140-2 Level 4-certified hardware.

With {{site.data.keyword.hscrypto}} you can take ownership of the cloud HSM to fully manage your encryption keys and to perform cryptographic operations by using [Keep Your Own Key (KYOK)](/docs/hs-crypto?topic=hs-crypto-understand-concepts#kyok-concept). This means you have full control and authority over encryption keys. No one except you (not even {{site.data.keyword.IBM_notm}}) has access to your encryption keys.

You must ensure that:

* Keys are created for a single purpose, and dedicated encryption keys are unique per consumer.
* Keys have a lifecycle that is defined and are rotated periodically based on {{site.data.keyword.framework-fs_notm}} controls.
* Recovery functions, if used, can be accessed only by authorized personnel.



### Setting up {{site.data.keyword.hscrypto}}
{: #setup-hpcs}

* Provision and configure an instance of {{site.data.keyword.hscrypto}}. See [Getting started with {{site.data.keyword.hscrypto}}](/docs/hs-crypto?topic=hs-crypto-get-started) for specific instructions.

For the highest level of security, you should use smart cards when initializing {{site.data.keyword.hscrypto}}. See [Initializing service instances by using smart cards and the Management Utilities](/docs/hs-crypto?topic=hs-crypto-initialize-hsm-management-utilities) for more details.
{: tip}

For more background information and considerations, see:

* [Components and concepts](/docs/hs-crypto?topic=hs-crypto-understand-concepts)
* [Bringing your encryption keys to the cloud](/docs/hs-crypto?topic=hs-crypto-importing-keys)
* [Master key rotation](/docs/hs-crypto?topic=hs-crypto-master-key-rotation-intro)
* [Root key rotation](/docs/hs-crypto?topic=hs-crypto-root-key-rotation-intro)

## Data encryption at rest in {{site.data.keyword.satelliteshort}} location
{: #shared-encryption-at-rest-satellite-location}

{{site.data.content.encryption-at-rest-in-satellite-location-important}}

## Integrating {{site.data.keyword.hscrypto}} with other {{site.data.keyword.cloud_notm}} services
{: #related-resources}

You must integrate {{site.data.keyword.hscrypto}} with the {{site.data.keyword.cloud_notm}} services that are part of your reference architecture deployments. {{site.data.keyword.block_storage_is_short}} and {{site.data.keyword.cos_short}} can be the most obvious services that need to have data encrypted at rest. But, other services have data that needs to be encrypted as well.

The following table provides references for requirements that are related to encryption of data at rest for each service in the reference architecture. See also [Integrating {{site.data.keyword.cloud_notm}} services with {{site.data.keyword.hscrypto}}](/docs/hs-crypto?topic=hs-crypto-integrate-services) for more details.


| Category | VPC reference architecture | {{site.data.keyword.satelliteshort}} reference architecture | Optional for both |
|----------|-------------------|-------------------|-------------------|
| Core  | - [VPC infrastructure services](/docs/vpc?topic=vpc-vpc-encryption-about) [^tabletext-1] | - [{{site.data.keyword.satelliteshort}}](/docs/satellite?topic=satellite-data-security#sat-data-encryption) |  |
| Containers  | - [{{site.data.keyword.openshiftshort}}](/docs/openshift?topic=openshift-encryption) \n - [{{site.data.keyword.registryshort}}](/docs/Registry?topic=Registry-registry_encrypt) | - [{{site.data.keyword.openshiftshort}}](/docs/openshift?topic=openshift-encryption) [^tabletext-satellite-enabled-openshift] \n - [{{site.data.keyword.registryshort}}](/docs/Registry?topic=Registry-registry_encrypt) |  |
| Networking  | - [VPC infrastructure services](/docs/vpc?topic=vpc-vpc-encryption-about) \n - {{site.data.keyword.dl_short}} [^tabletext-2] \n - {{site.data.keyword.tg_short}}  | |  |
| Storage  | - [{{site.data.keyword.block_storage_is_short}}](/docs/vpc?topic=vpc-block-storage-vpc-encryption) \n - [{{site.data.keyword.cos_short}}](/docs/cloud-object-storage?topic=cloud-object-storage-encryption) | - [{{site.data.keyword.cos_short}}](/docs/cloud-object-storage?topic=cloud-object-storage-encryption) |  |
| Security | - {{site.data.keyword.hscrypto}}  | - {{site.data.keyword.hscrypto}} | - [{{site.data.keyword.appid_short_notm}}](/docs/appid?topic=appid-mng-data#enable-customer-keys-hpcs) |
| Logging and monitoring  | - [{{site.data.keyword.atracker_short}}](/docs/activity-tracker?topic=activity-tracker-mng-data) \n - [{{site.data.keyword.compliance_short}}](/docs/security-compliance?topic=security-compliance-mng-data) \n - [Flow Logs for VPC](/docs/vpc?topic=vpc-vpc-encryption-about) | - [{{site.data.keyword.atracker_short}}](/docs/activity-tracker?topic=activity-tracker-mng-data) \n - [{{site.data.keyword.compliance_short}}](/docs/security-compliance?topic=security-compliance-mng-data) |  |
| Integration  | |  | - [{{site.data.keyword.messagehub}}](/docs/EventStreams?topic=EventStreams-managing_encryption) |
| Databases  |  |  | - [{{site.data.keyword.ihsdbaas_mongodb_full}}](/docs/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-hpcs-byok) \n - [{{site.data.keyword.ihsdbaas_postgresql_full}}](/docs/hyper-protect-dbaas-for-postgresql?topic=hyper-protect-dbaas-for-postgresql-hpcs-byok) | |
{: caption="Table 1. Data encryption information for {{site.data.keyword.cloud_notm}} services in the reference architectures" caption-side="top"}

[^tabletext-1]: {{site.data.content.vpc-infrastructure-services-content}}

[^tabletext-2]: Neither {{site.data.keyword.dl_short}} nor {{site.data.keyword.tg_short}} store consumer data, so no integration with {{site.data.keyword.hscrypto}} is needed.

[^tabletext-satellite-enabled-openshift]: {{site.data.content.satellite-enabled-openshift}}



## Related controls in {{site.data.keyword.framework-fs_notm}}
{: #related-controls}

{{site.data.content.related-controls-disclaimer}}

| Family              | Control                                           |
|---------------------|---------------------------------------------------|
| System and Communications Protection (SC) | [SC-12 Cryptographic Key Establishment and Management](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-12) \n [SC-12 (2) Cryptographic Key Establishment and Management &#124; Symmetric Keys](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-12.2) \n [SC-12 (3) Cryptographic Key Establishment and Management &#124; Asymmetric Keys](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-12.3) \n [SC-13 Cryptographic Protection](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-13) \n [SC-28 Protection of Information At Rest](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-28) \n [SC-28 (1) Protection of Information at Rest &#124; Cryptographic Protection](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-28.1)  |
{: caption="Table 2. Related controls in {{site.data.keyword.framework-fs_notm}}" caption-side="top"}

In addition, you must follow the guidance found in Appendix A: Cryptographic Requirements within the "Control Implementation Overview Template for Application Providers Using {{site.data.keyword.vpc_full}}".

## Next steps
{: #next-steps}

* [Data encryption in transit](/docs/framework-financial-services?topic=framework-financial-services-shared-encryption-in-transit)

