---

copyright:
  years: 2020, 2022
lastupdated: "2022-07-28"

keywords: 

subcollection: framework-financial-services

---

{{site.data.keyword.attribute-definition-list}}

# VPC reference architecture for {{site.data.keyword.cloud_notm}} for Financial Services
{: #vpc-architecture-about}

[{{site.data.keyword.vpc_full}} (VPC)](/docs/vpc?topic=vpc-about-vpc#about-vpc) is a public cloud offering that lets an enterprise establish its own private cloud-like computing environment on shared public cloud infrastructure. A VPC gives an enterprise the ability to define and control a virtual network that is logically isolated from all other public cloud tenants, creating a private, secure place on the public cloud. The VPC reference architecture for the {{site.data.keyword.cloud_notm}} for Financial Services is designed to provide a framework for building a VPC-based offering according to the [best practices and requirements of the {{site.data.keyword.framework-fs_notm}}](/docs/framework-financial-services?topic=framework-financial-services-best-practices). We detail this architecture and provide guidance for deploying, configuring, and managing it.
{: shortdesc}

## Architecture diagram
{: #vpc-arch-diagram}

![High-level VPC reference architecture for {{site.data.keyword.cloud_notm}} for Financial Services](../images/vpc-high-level/vpc-high-level-v2.svg){: caption="Figure 1. High-level VPC reference architecture for {{site.data.keyword.cloud_notm}} for Financial Services" caption-side="bottom"}

Central to the architecture are two VPCs, which provide for separation of concerns between provider management functionality and consumer workloads.

Management VPC
:   Provides compute, storage, and network services to enable the application application provider's administrators to monitor, operate, and maintain the environment.

Workload VPC
:   Provides compute, storage, and network services to support hosted applications and operations that deliver services to the consumer.

Other key features to note:

* Supports a single tenant.
* Resides in one or more [multizone regions](/docs/overview?topic=overview-locations) from the [list of approved regions](/docs/framework-financial-services?topic=framework-financial-services-best-practices#best-practices-financial-services-regions).
* Gives two options for compute that can be mixed and matched: [{{site.data.keyword.vsi_is_full}}](#services-compute-vsi) and [{{site.data.keyword.openshiftlong}}](#services-containers-openshift).
* Enables access to the management VPC from the application provider's enterprise environment through [{{site.data.keyword.dl_full}} Dedicated](#services-networking-direct-link) or [{{site.data.keyword.cloud}} {{site.data.keyword.vpn_vpc_full}}](#services-networking-vpn).
* Provides connectivity from the consumer's enterprise environment to the workload VPC through {{site.data.keyword.dl_short}} or {{site.data.keyword.vpn_vpc_short}}.
* Connects management VPC and workload VPC by using [{{site.data.keyword.tg_full}}](#services-networking-transit-gateway).
* Allows connectivity to {{site.data.keyword.cloud_notm}} services that use [{{site.data.keyword.cloud_notm}} Virtual Private Endpoints for VPC](#services-networking-vpe).
* Encrypts data by using [{{site.data.keyword.cloud_notm}} {{site.data.keyword.hscrypto}}, which enables keep your own key (KYOK) functionality which provides technical assurance that {{site.data.keyword.IBM_notm}} cannot access your keys.

## Variation with edge or transit VPC for public internet access
{: #edge-vpc-architecture}

The architecture in the previous section is the most secure way of enabling consumers to access the applications that are running in a workload VPC. However, there might be valid cases where it is desirable to allow consumers to access your service through the public internet. The same base architecture can be adapted to securely enable this type of access.

![High-level VPC reference architecture with edge VPC for the {{site.data.keyword.cloud_notm}} for Financial Services](../images/vpc-high-level/vpc-high-level-v2-with-edge.svg){: caption="Figure 2. High-level VPC reference architecture with edge/transit VPC" caption-side="bottom"}

The revised architecture adds:

* [{{site.data.keyword.cis_full}}](/docs/cis?topic=cis-getting-started) ({{site.data.keyword.cis_short_notm}}) to provide global load balancing and layer 3/4 protection against distributed denial-of-service (DDoS) attacks.
* Virtual network firewall software in the workload VPC to provide web application firewall (WAF) protection and layer 7 protection against denial-of-service (DoS) attacks.

See [VPC architecture with virtual servers](/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-detailed-vsi#edge-vpc-architecture) for more details on this variation.



## Financial Services Validated services
{: #financial-services-validated-services}

Deploying the reference architecture depends upon VPC infrastructure and PaaS services that are [{{site.data.keyword.cloud_notm}} for Financial Services Validated](https://cloud.ibm.com/catalog?search=label%3Afs_ready){: external}. This means that they have evidenced compliance to the controls of the {{site.data.keyword.framework-fs_notm}}. Financial Services Validated services are designed to help address the requirements of financial institutions for regulatory compliance, security, and resiliency. When properly configured and managed, services that are Financial Services Validated work together so you can deliver a solution that conforms to the best practices of the {{site.data.keyword.framework-fs_notm}}.

{{site.data.content.fs-validated-disclaimer-important}}

| Category | Required services | Optional services |
|----------|-------------------|-------------------|
| Compute [^fs-validated-table-1-1]  | - [{{site.data.keyword.vsi_is_full}}](#services-compute-vsi)  | - [Dedicated hosts for VPC](#services-compute-dedicated-hosts) [^fs-validated-table-2-1] \n - [{{site.data.keyword.cloud}} Auto Scale for VPC](#services-compute-auto-scale) [^fs-validated-table-2-2] |
| Containers [^fs-validated-table-3]  | - [{{site.data.keyword.openshiftlong}}](#services-containers-openshift) \n - [{{site.data.keyword.registrylong}}](#services-containers-registry) |  |
| Networking - VPC infrastructure  | - [{{site.data.keyword.vpc_full}}](/docs/vpc?topic=vpc-about-vpc) \n - [{{site.data.keyword.cloud}} {{site.data.keyword.alb_full}}](#services-networking-alb) \n - [{{site.data.keyword.cloud}} {{site.data.keyword.vpn_vpc_full}}](#services-networking-vpn) [^fs-validated-table-4-1] \n - [{{site.data.keyword.dns_full}}](#services-networking-dns-services) \n - [{{site.data.keyword.cloud}} Virtual Private Endpoints for VPC](#services-networking-vpe) |  |
| Networking - interconnectivity  | - [{{site.data.keyword.dl_full}} (2.0)](#services-networking-direct-link)[^fs-validated-table-4-2] \n - [{{site.data.keyword.tg_full}}](#services-networking-transit-gateway) |  |
| Storage  | - [{{site.data.keyword.block_storage_is_full}}](#services-storage-block) \n - [{{site.data.keyword.cos_full}}](#services-storage-cos) |  |
| Security  | - [{{site.data.keyword.cloud}} {{site.data.keyword.hscrypto}}](#services-security-hpcs)  | - [{{site.data.keyword.appid_full}}](#services-security-app-id) |
| Logging and monitoring  | - [{{site.data.keyword.atracker_full_notm}}](#services-logging-platform-events) [^fs-validated-table-5] \n - [{{site.data.keyword.compliance_long}}](#services-scc) [^tabletext-not-yet-validated] \n - [{{site.data.keyword.cloud}} Flow Logs for VPC](#services-logging-flow-logs) [^fs-validated-table-1-2]  |  |
| Integration  |  | - [{{site.data.keyword.messagehub_full}}](#services-integration-event-streams) |
| Databases  |  | - [{{site.data.keyword.ihsdbaas_mongodb_full}}](#services-databases-ihsdbaas_mongodb) \n - [{{site.data.keyword.ihsdbaas_postgresql_full}}](#services-databases-ihsdbaas_postgresql)  |
{: caption="Table 1. Required and optional services for VPC reference architecture" caption-side="top"}

[^fs-validated-table-1-1]: {{site.data.content.only-required-virtual-servers}}

[^fs-validated-table-1-2]: {{site.data.content.only-required-virtual-servers}}

[^fs-validated-table-2-1]: {{site.data.content.highly-recommended-with-virtual-servers}}

[^fs-validated-table-2-2]: {{site.data.content.highly-recommended-with-virtual-servers}}

[^fs-validated-table-3]: {{site.data.content.only-required-openshift}}

[^fs-validated-table-4-1]: {{site.data.content.only-one-of-dl-or-vpc}}

[^fs-validated-table-4-2]: {{site.data.content.only-one-of-dl-or-vpc}}

[^fs-validated-table-5]: {{site.data.content.event-routing-fs-validation}}

[^tabletext-not-yet-validated]: {{site.data.content.not-yet-validated}}

The remainder of this section goes into more detail about how these services fit into the reference architecture.

### Compute
{: #required-services-compute}

#### {{site.data.keyword.vsi_is_short}} 
{: #services-compute-vsi}

One option for compute within your VPC is [{{site.data.keyword.vsi_is_short}}](/docs/vpc?topic=vpc-about-advanced-virtual-servers). {{site.data.keyword.vsi_is_short}} is an infrastructure-as-a-service (IaaS) offering that gives you access to all of the benefits of VPC, including network isolation, security, and flexibility.

With {{site.data.keyword.vsi_is_short}}, you can quickly provision instances with high network performance. When you provision an instance, you select a profile that matches the amount of memory and compute power that you need for the application that you plan to run on the instance. Instances are available on the x86 architecture.

#### Dedicated hosts for VPC (optional)
{: #services-compute-dedicated-hosts}

With [Dedicated hosts for VPC](/docs/vpc?topic=vpc-creating-dedicated-hosts-instances), you can carve out a single-tenant compute node, free from users outside of your organization. Within that dedicated space, you can create virtual server instances according to your needs. Additionally, you can create dedicated host groups that contain dedicated hosts for a specific purpose. Because a dedicated host is a single-tenant space, only users within your account that have the required permissions can create instances on the host.

Dedicated hosts are highly recommended when you use virtual servers, particularly for any parts of your application that process regulated data and keep it in memory.

#### {{site.data.keyword.cloud_notm}} Auto Scale for VPC (optional)
{: #services-compute-auto-scale}

[Auto Scale for VPC](/docs/vpc?topic=vpc-creating-auto-scale-instance-group) is highly recommended if you are using virtual servers. With Auto Scale for VPC, you can improve performance and costs by dynamically creating virtual server instances to meet the demands of your environment. You set scaling policies that define your desired average utilization for metrics like CPU, memory, and network usage. The policies that you define determine when virtual server instances are added or removed from your instance group.

### Containers
{: #services-containers}

#### {{site.data.keyword.openshiftlong_notm}} 
{: #services-containers-openshift}

In addition to {{site.data.keyword.vsi_is_short}}, you can use [{{site.data.keyword.openshiftlong_notm}}](/docs/openshift?topic=openshift-roks-overview) if you want to run containers. {{site.data.content.openshift-service-description}}

In practice, when you choose {{site.data.keyword.openshiftlong_notm}} for your primary compute, you might also need one or more instances of {{site.data.keyword.vsi_is_short}} for other parts of the reference architecture.

#### {{site.data.keyword.registrylong_notm}}  
{: #services-containers-registry}

{{site.data.content.service-description-container-registry}}

### Networking - VPC infrastructure
{: #services-networking-vpc}

#### {{site.data.keyword.cloud_notm}} {{site.data.keyword.alb_full}} 
{: #services-networking-alb}

Use [{{site.data.keyword.alb_full}} (ALB)](/docs/vpc?topic=vpc-load-balancers) to distribute traffic among multiple server instances within the same region of your VPC. You can create a public or private ALB.

[{{site.data.keyword.cloud}} {{site.data.keyword.nlb_full}} (NLB)](/docs/vpc?topic=vpc-network-load-balancers) is also {{site.data.keyword.cloud_notm}} for Financial Services Validated, but does not span zones. So, NLBs are not typically used in applications where high availability is needed.
{: note}

#### {{site.data.keyword.cloud_notm}} {{site.data.keyword.vpn_vpc_full}} 
{: #services-networking-vpn}

You can use the [{{site.data.keyword.vpn_vpc_short}}](/docs/vpc?topic=vpc-using-vpn) service to securely connect your VPC to another private network. Use a static, route-based VPN or a policy-based VPN to set up an IPsec site-to-site tunnel between your VPC and your on-premises private network, or another VPC.

{{site.data.keyword.vpn_vpc_short}} is required to connect to the management VPC if not using {{site.data.keyword.dl_short}}.


#### {{site.data.keyword.dns_full_notm}} 
{: #services-networking-dns-services}

[{{site.data.keyword.dns_short}}](/docs/dns-svcs?topic=dns-svcs-about-dns-services) provides private DNS to VPC users. Private DNS zones are resolvable only on {{site.data.keyword.cloud_notm}}, and only from explicitly permitted networks in an account.

#### {{site.data.keyword.cloud_notm}} Virtual Private Endpoints for VPC
{: #services-networking-vpe}

With [Virtual Private Endpoints (VPE) for VPC](/docs/vpc?topic=vpc-about-vpe) you can connect to supported {{site.data.keyword.cloud_notm}} services from your VPC network by using the IP addresses of your choosing, which is allocated from a subnet within your VPC.

{{site.data.content.service-description-vpe}}

### Networking - Interconnectivity
{: #services-networking-interconnectivity}

#### {{site.data.keyword.dl_full_notm}} 
{: #services-networking-direct-link}

{{site.data.content.service-description-direct-link}}

{{site.data.keyword.dl_short}} is required if not using {{site.data.keyword.vpn_vpc_full}} (see next section).

#### {{site.data.keyword.tg_full_notm}} 
{: #services-networking-transit-gateway}

As the number of your VPCs grow, you need an easy way to manage the interconnection between these resources across multiple regions. [{{site.data.keyword.tg_short}}](/docs/transit-gateway?topic=transit-gateway-about) is designed specifically for this purpose, and is the means for connecting your management VPC to your workload VPC.

### Storage
{: #services-storage}

#### {{site.data.keyword.block_storage_is_short}} 
{: #services-storage-block}

[{{site.data.keyword.block_storage_is_short}}](/docs/vpc?topic=vpc-block-storage-about) provides hypervisor-mounted, high-performance data storage for your virtual server instances that you can provision within a VPC. The VPC infrastructure provides rapid scaling across zones and extra performance and security.

{{site.data.keyword.block_storage_is_short}} provides primary boot volumes and secondary data volumes. Boot volumes are automatically created and attached during instance provisioning. Data volumes can be created and attached during instance provisioning as well, or as stand-alone volumes that you can later attach to an instance. To protect your data, you can use your own encryption keys with {{site.data.keyword.hscrypto}}.

#### {{site.data.keyword.cos_full_notm}} 
{: #services-storage-cos}

{{site.data.content.service-description-cloud-object-storage-1}}

{{site.data.content.service-description-cloud-object-storage-2}}

### Security
{: #services-security}

#### {{site.data.keyword.cloud_notm}} {{site.data.keyword.hscrypto}} 
{: #services-security-hpcs}

{{site.data.content.service-description-hpcs}}

#### {{site.data.keyword.appid_full_notm}} (optional) 
{: #services-security-app-id}

{{site.data.content.service-description-app-id}}

### Logging and monitoring
{: #services-logging-monitoring}

#### {{site.data.keyword.atracker_full_notm}}
{: #services-logging-platform-events}

{{site.data.content.service-description-activity-tracker-event-routing-1}}

{{site.data.content.service-description-activity-tracker-event-routing-2}}

{{site.data.content.service-description-activity-tracker-event-routing-3-important}}

#### {{site.data.keyword.compliance_full}}
{: #services-scc}

{{site.data.content.service-description-scc}}

#### {{site.data.keyword.cloud_notm}} Flow Logs for VPC
{: #services-logging-flow-logs}

[Flow Logs for VPC](/docs/vpc?topic=vpc-flow-logs) enables the collection, storage, and presentation of information about the Internet Protocol (IP) traffic flowing to and from network interfaces within your VPC.

Flow logs can help with a number of tasks, including:

* Troubleshooting why specific traffic isn't reaching an instance, which helps to diagnose restrictive security group rules
* Recording the metadata of network traffic that is reaching your instance
* Determining source and destination traffic from the network interfaces
* Adhering to compliance regulations
* Assisting with root cause analysis

### Integration
{: #vpc-architecture-optional-services-integration}

#### {{site.data.keyword.IBM_notm}} {{site.data.keyword.messagehub}} for {{site.data.keyword.cloud_notm}} (optional) 
{: #services-integration-event-streams}

{{site.data.content.service-description-event-streams-1}}

{{site.data.content.service-description-event-streams-2}}

{{site.data.content.service-description-event-streams-3-unordered-list}}

{{site.data.content.service-description-event-streams-4}}

### Databases
{: #vpc-architecture-optional-services-databases}

#### {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}} (optional) 
{: #services-databases-ihsdbaas_mongodb}

{{site.data.content.service-description-ihsdbaas-mongodb-1}}

{{site.data.content.service-description-ihsdbaas-mongodb-2}}

#### {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_postgresql_full}} (optional) 
{: #services-databases-ihsdbaas_postgresql}

{{site.data.content.service-description-ihsdbaas-postgresql-1}}

{{site.data.content.service-description-ihsdbaas-postgresql-2}}


## Reference architecture components
{: #vpc-architecture-detailed-vsi-component-summary}

The following table provides a summary of the main features of the VPC reference architecture and associated {{site.data.keyword.cloud_notm}} services.

| Architectural component | Technology  |
|-------------------------|---------------------------------------------------|
| Compute  | {{site.data.keyword.vsi_is_short}} \n Dedicated hosts for VPC |
| Containers [^component-tabletext-1] | {{site.data.keyword.openshiftlong_notm}} \n {{site.data.keyword.registryshort}} 
| Inbound connectivity to management VPC | {{site.data.keyword.dl_short}} or \n {{site.data.keyword.vpn_vpc_short}} |
| Inbound connectivity to workload VPC | {{site.data.keyword.dl_short}} or \n {{site.data.keyword.vpn_vpc_short}} |
| Virtual network firewall | Install your own software [^component-tabletext-2] |
| Connectivity between VPCs  | {{site.data.keyword.tg_short}} |
| Connectivity to {{site.data.keyword.cloud_notm}} services | Virtual Private Endpoints for VPC |
| Load balancing | {{site.data.keyword.alb_full}} |
| DNS  | {{site.data.keyword.dns_short}} |
| Bastion host | Install your own software |
| Scaling compute  | Auto Scale for VPC |
| Web app authentication in workload VPC | {{site.data.keyword.appid_short_notm}} |
| Secrets management | Install your own software |
| IBM Cloud platform audit logging | {{site.data.keyword.atracker_short}} [^component-tabletext-3]  |
| Application provider audit logging | Install your own software for SIEM |
| Application provider operational logging | Install your own software  |
| Application provider operational monitoring  | Install your own software |
| Compliance monitoring  | {{site.data.keyword.compliance_short}} [^component-tabletext-4] |
| Flow/traffic logging | Flow Logs for VPC  |
| Encryption at rest | {{site.data.keyword.hscrypto}}  |
| Encryption in transit (TLS offload)  | {{site.data.keyword.hscrypto}} |
| Cross-zone high availability | Multizone region |
| Cross-region high availability | Deploy in multiple regions |
| Backup and recovery  | Install your own software |
| Developer tools  | Install your own software |
| Endpoint protection  | Install your own software |
| Event queues | {{site.data.keyword.messagehub}} or \n Install your own software |
| Databases | {{site.data.keyword.ihsdbaas_mongodb_full}} or \n {{site.data.keyword.ihsdbaas_postgresql_full}} or \n Install your own software |
{: caption="Table 2. Services needed for different parts of the VPC reference architecture" caption-side="top"}

[^component-tabletext-1]: {{site.data.content.only-required-openshift}}

[^component-tabletext-2]: {{site.data.content.byo-software-disclaimer}}

[^component-tabletext-3]: {{site.data.content.event-routing-fs-validation}}

[^component-tabletext-4]: {{site.data.content.not-yet-validated}}

## Next steps
{: #next-steps}

* Learn about some important [VPC concepts](/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-concepts).
* Explore more detailed views of the VPC reference architecture based on compute type:
    * [{{site.data.keyword.vsi_is_short}}](/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-detailed-vsi)
    * [{{site.data.keyword.openshiftlong_notm}}](/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-detailed-openshift)
