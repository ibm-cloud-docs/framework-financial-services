---

copyright:
  years: 2020, 2022
lastupdated: "2022-07-28"

keywords: 

subcollection: framework-financial-services

---

{{site.data.keyword.attribute-definition-list}}

# Storage for {{site.data.keyword.satelliteshort}} reference architecture
{: #satellite-architecture-storage}

You need to consider storage options both in {{site.data.keyword.cloud_notm}} and in the on-premises {{site.data.keyword.satelliteshort}} location.
{: shortdesc}

## Storage in {{site.data.keyword.cloud_notm}}
{: #satellite-architecture-storage-ibm-cloud}

All {{site.data.keyword.satelliteshort}} control plane data is backed up to an [{{site.data.keyword.cos_full_notm}}](/docs/cloud-object-storage?topic=cloud-object-storage-about-cloud-object-storage) service instance within {{site.data.keyword.cloud_notm}}. This is done so that your location can be restored after a disaster. When you create a location, you also provide a {{site.data.keyword.cos_short}} service instance that you control for backup of the location control plane worker nodes. Control plane master data is backed up by {{site.data.keyword.IBM_notm}} and stored in an {{site.data.keyword.IBM_notm}}-owned {{site.data.keyword.cos_short}} instance. {{site.data.keyword.satelliteshort}} cluster master data is backed up to the {{site.data.keyword.cos_short}} instance that you own.

Aside from backing up data from the {{site.data.keyword.satelliteshort}} location, {{site.data.keyword.cos_short}} is also used to store auditable {{site.data.keyword.cloud_notm}} events. For more information, see [Audit logging of {{site.data.keyword.cloud_notm}} events](/docs/allowlist/framework-financial-services?topic=framework-financial-services-shared-logging-audit).

### About {{site.data.keyword.cos_short}}
{: #satellite-architecture-storage-ibm-cloud-cos}

{{site.data.content.service-description-cloud-object-storage-1}}

{{site.data.content.service-description-cloud-object-storage-2}}

{{site.data.content.service-description-cloud-object-storage-use-cross-region-kyok-important}}

{{site.data.content.service-description-cloud-object-storage-reference-list-intro}}

{{site.data.content.service-description-cloud-object-storage-reference-list}}

## Storage in {{site.data.keyword.satelliteshort}} location
{: #satellite-architecture-storage-satellite-location}

Within the {{site.data.keyword.satelliteshort}} location, {{site.data.keyword.satelliteshort}} storage uses {{site.data.keyword.satelliteshort}} Config to provide a convenient way to install various storage drivers in {{site.data.keyword.openshiftshort}} clusters, by using storage templates. The storage templates are provided and tested by the vendors. After you install {{site.data.keyword.satelliteshort}} storage, your cluster users can use Kubernetes persistent volume claims (PVCs) to order and save their application data in persistent storage. For more information, see [Understanding {{site.data.keyword.satelliteshort}} storage](/docs/satellite?topic=satellite-sat-storage-template-ov).

{{site.data.content.encryption-at-rest-in-satellite-location-important}}

## Related controls in {{site.data.keyword.framework-fs_notm}} 
{: #related-controls}

More details on some of the major {{site.data.keyword.framework-fs_notm}} controls related to storage can be found in the following articles:

- [Encryption at rest](/docs/allowlist/framework-financial-services?topic=framework-financial-services-shared-encryption-at-rest)
- [Backup and recovery](/docs/allowlist/framework-financial-services?topic=framework-financial-services-shared-bcdr)

## Next steps
{: #next-steps}

* [Audit logging of {{site.data.keyword.cloud_notm}} events](/docs/allowlist/framework-financial-services?topic=framework-financial-services-shared-logging-audit)
