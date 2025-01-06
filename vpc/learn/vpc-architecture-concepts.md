---

copyright:
  years: 2020, 2025
lastupdated: "2025-01-06"

keywords: 

subcollection: framework-financial-services

---

{{site.data.keyword.attribute-definition-list}}

# VPC concepts
{: #vpc-architecture-concepts}

Now that you've seen the [high-level VPC reference architecture for {{site.data.keyword.cloud_notm}} for Financial Services](/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-about), there are a number of important VPC concepts that will help you understand the components within the management and workload VPCs as we dive into lower-level details. Each VPC is a virtual network that is linked to your customer account. It gives you cloud security, with the ability to scale dynamically, by providing fine-grained control over your virtual infrastructure and your network traffic segmentation.
{: shortdesc}

## Network
{: #network}

### Regions, zones, and subnets
{: #regions}

All VPCs exist in a single region. A [region](#x2091391){: term} is an abstraction that is related to the geographic area in which a VPC is deployed. Each [multizone region](/docs/overview?topic=overview-locations) contains multiple zones. A [zone](x2070723){: term} is another an abstraction that refers to the physical data center that hosts the compute, network, and storage resources, as well as the related cooling and power, which provides services and applications.

VPCs that are created in a one of the multizone regions can span multiple zones to facilitate high availability. For {{site.data.keyword.cloud_notm}} for Financial Services, it is recommended to use at least three zones in each VPC to help ensure resiliency.

Subnets consist of a specified IP address range (CIDR block). Subnets are bound to a single zone, and they cannot span multiple zones or regions. Subnets in the same VPC are connected to each other.

For more information, see [About networking](/docs/vpc?topic=vpc-about-networking-for-vpc).

<!-- Might want to note for {[fs-cloud-notm]} that stuff in About networking like
Public Gateway and Floating IPs should not generally be used. -->

### Access control lists and security groups
{: #security-groups-access-control-lists}

Access control lists (ACLs) and security groups provide ways to control the traffic across the subnets and instances in your VPC by using rules that you specify. ACLs control traffic to and from a subnet, while security groups control the traffic at the virtual server instance (VSI) level.



For more information, see [Security in your VPC](/docs/vpc?topic=vpc-security-in-your-vpc).

## Compute
{: #compute}

### {{site.data.keyword.vsi_is_short}}
{: #compute-vsi}

{{site.data.keyword.vsi_is_short}} gives you access to all of the benefits of {{site.data.keyword.vpc_short}}, including network isolation, security, and flexibility. When you provision an instance, you select a profile that matches the amount of memory and compute power that you need for the application that you plan to run on the instance. Instances are available on the x86 architecture.

See [About virtual server instances for VPC](/docs/vpc?topic=vpc-about-advanced-virtual-servers) for more details.

### Dedicated hosts
{: #compute-dedicated-hosts}

Provision your virtual servers on a dedicated host. Dedicated hosts are used to carve out a single-tenant compute node, free from users outside of your organization. Within that dedicated space, you can create virtual server instances according to your needs. Additionally, you can create dedicated host groups that contain dedicated hosts for a specific purpose. Because a dedicated host is a single-tenant space, only users within your account that have the required permissions can create instances on the host.

See [Creating dedicated hosts and groups](/docs/vpc?topic=vpc-creating-dedicated-hosts-instances) for more details.

### {{site.data.keyword.openshiftlong_notm}}
{: #compute-openshift}

You can also use {{site.data.keyword.openshiftlong_notm}} to run applications. It is also backed by virtual server instances, but, in that case, the virtual service instances are managed by IBM.

See [VPC reference architecture with {{site.data.keyword.openshiftshort}}](/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-detailed-openshift) for more details.

## Storage
{: #storage}

 [{{site.data.keyword.block_storage_is_short}}](/docs/vpc?topic=vpc-block-storage-about) provides hypervisor-mounted, high-performance data storage for your virtual server instances that you can provision within a VPC. The VPC infrastructure provides rapid scaling across zones and extra performance and security.

{{site.data.keyword.block_storage_is_short}} is used for both primary boot volumes and secondary data volumes. Boot volumes are automatically created and attached during instance provisioning. Data volumes can be created and attached during instance provisioning as well, or as stand-alone volumes that you can later attach to an instance. To protect your data, you should use KYOK encryption with {{site.data.keyword.hscrypto}}.

See [About Block Storage for VPC](/docs/vpc?topic=vpc-block-storage-about) for more details.

## Next steps
{: #next-steps}

* Take a deeper look at the [VPC reference architecture with virtual servers](/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-detailed-vsi).
