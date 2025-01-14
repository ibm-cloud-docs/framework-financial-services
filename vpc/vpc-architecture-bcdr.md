---

copyright:
  years: 2020, 2024
lastupdated: "2024-11-19"

keywords: 

subcollection: framework-financial-services

---

{{site.data.keyword.attribute-definition-list}}

# Business continuity and disaster recovery for VPC reference architecture
{: #vpc-architecture-bcdr}

As described in [Business continuity and disaster recovery (BCDR) overview](/docs/framework-financial-services?topic=framework-financial-services-shared-bcdr), BCDR is important for all cloud-based applications. In this article, you will learn about specifics of BCDR in the {{site.data.keyword.satelliteshort}} reference architecture.

## Workloads in VPC reference architecture
{: #your-workloads}

### Workloads on virtual server instances
{: #your-workloads-snapshots-for-vpc}

If you are running virtual server instances and _not_ trying to get your application designated as Financial Services Validated, then you can consider [Snapshots for VPC](/docs/vpc?topic=vpc-snapshots-vpc-about). Snapshots for VPC is a regional offering that lets you create a point-in-time copy of your block storage boot or data volumes. The initial snapshot you take is a full backup of the volume. Subsequent snapshots of the same volume are incremental; only the changes since the last snapshot are captured. You can select a snapshot during instance provisioning, and restore a new, fully-provisioned boot volume to start the instance. You can also create and attach a data volume from a snapshot within a running virtual server instance.

For more information, see [General procedure for creating and using snapshots](/docs/vpc?topic=vpc-snapshots-vpc-planning&interface=ui).

### Workloads on {{site.data.keyword.openshiftshort}}
{: #your-workloads-openshift}

If you are running {{site.data.keyword.openshiftshort}} and not trying to get your application designated as Financial Services Validated, then you can consider [Portworx](https://portworx.com/products/portworx-enterprise//){: external}. Portworx is a highly available software-defined storage solution that you can use to manage local persistent storage for your containerized databases and other stateful apps, or to share data between pods across multiple zones.

See [Storing data on software-defined storage (SDS) with Portworx](/docs/openshift?topic=openshift-portworx) for more information.

## Backup and disaster recovery for {{site.data.keyword.cloud_notm}} services
{: #ibm-cloud-services}

In addition to backing up your workloads and having the capability to restore service in the face of a disaster, your BCDR strategy needs to consider all of the {{site.data.keyword.cloud_notm}} services in your deployment. See [Backup and disaster recovery for {{site.data.keyword.cloud_notm}} services](/docs/framework-financial-services?topic=framework-financial-services-shared-bcdr#ibm-cloud-services).

## Related controls in {{site.data.keyword.framework-fs_notm}}
{: #related-controls}

See [related controls for backup and recovery](/docs/framework-financial-services?topic=framework-financial-services-shared-bcdr#related-controls).

## Next steps
{: #next-steps}

- [High availability overview](/docs/framework-financial-services?topic=framework-financial-services-shared-high-availability)
