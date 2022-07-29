---

copyright:
  years: 2020, 2022
lastupdated: "2022-07-28"

keywords: 

subcollection: framework-financial-services

---

{{site.data.keyword.attribute-definition-list}}

# Storage for VPC reference architecture
{: #vpc-architecture-storage}

Learn about the Financial Services Validated storage options that are used within the VPC reference architecture.
{: shortdesc}

## {{site.data.keyword.block_storage_is_full}} 
{: #block}

[{{site.data.keyword.block_storage_is_short}}](/docs/vpc?topic=vpc-block-storage-about) provides hypervisor-mounted, high-performance data storage for your virtual server instances that you can provision within a VPC. The VPC infrastructure provides rapid scaling across zones and extra performance and security.

{{site.data.keyword.block_storage_is_short}} is used for both primary boot volumes and secondary data volumes. Boot volumes are automatically created and attached during instance provisioning. Data volumes can be created and attached during instance provisioning as well, or as stand-alone volumes that you can later attach to an instance. To protect your data, you should use KYOK encryption with {{site.data.keyword.hscrypto}}.

To start creating {{site.data.keyword.block_storage_is_short}} volumes, see the following instructions:

* [Create and attach a block storage volume when you create a new instance](/docs/vpc?topic=vpc-creating-block-storage#create-from-vsi)
* [Create a stand-alone block storage volume](/docs/vpc?topic=vpc-creating-block-storage#create-standalone-vol)
* [Creating block storage volumes with customer-managed encryption](/docs/vpc?topic=vpc-block-storage-vpc-encryption)

## {{site.data.keyword.cos_full}} 
{: #storage-cos}

{{site.data.content.service-description-cloud-object-storage-1}}

{{site.data.content.service-description-cloud-object-storage-2}}

{{site.data.content.service-description-cloud-object-storage-use-cross-region-kyok-important}}

{{site.data.content.service-description-cloud-object-storage-reference-list-intro}}

{{site.data.content.service-description-cloud-object-storage-reference-list}}

## Related controls in {{site.data.keyword.framework-fs_notm}} 
{: #related-controls}

More details on some of the major {{site.data.keyword.framework-fs_notm}} controls related to storage can be found in the following articles:

- [Encryption at rest](/docs/framework-financial-services?topic=framework-financial-services-shared-encryption-at-rest)
- [Backup and recovery](/docs/framework-financial-services?topic=framework-financial-services-shared-bcdr)

## Next steps
{: #next-steps}

* [Audit logging of {{site.data.keyword.cloud_notm}} events](/docs/framework-financial-services?topic=framework-financial-services-shared-logging-audit)
