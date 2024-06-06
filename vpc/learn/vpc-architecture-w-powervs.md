---

copyright:
  years: 2024
lastupdated: "2024-06-05"

keywords:

subcollection: framework-financial-services

version: 1.0

deployment-url:

use-case: BankingAndFinanceIndustry, BankingCustomerExperience, BankingSecurity

industry: Banking, FinancialSector

content-type: reference-architecture

---

{{site.data.keyword.attribute-definition-list}}

# {{site.data.keyword.vsi_is_short}} and {{site.data.keyword.powerSys_notm}} reference architecture for {{site.data.keyword.cloud_notm}} for Financial Services
{: #vpc-architecture-detailed-vsi-w-powervs}
{: toc-content-type="reference-architecture"}
{: toc-industry="Banking, FinancialSector"}
{: toc-use-case="BankingAndFinanceIndustry, BankingCustomerExperience, BankingSecurity"}
{: toc-version="1.0"}


This reference architecture summarizes how to extend VSI on VPC reference architecture for {{site.data.keyword.cloud_notm}} for Financial Services to include {{site.data.keyword.powerSys_notm}} workloads.
{: shortdesc}

IBM Power is a family of high-performance servers that are designed for running large-scale data-driven and mission-critical workloads and are known for their scalability, reliability, sustainability, and performance. {{site.data.keyword.powerSysFull}} is a Power Systems offering in IBM Cloud. As stated in the documentation, [{{site.data.keyword.powerSys_notm}}](/docs/power-iaas?topic=power-iaas-getting-started) are located in the IBM data centers, distinct from the IBM Cloud servers with separate networks and direct-attached storage. The internal networks are fenced but offer connectivity options to IBM Cloud infrastructure or on-premises environments. This infrastructure design enables {{site.data.keyword.powerSys_notm}} to maintain key enterprise software certification and support as the {{site.data.keyword.powerSys_notm}} architecture is identical to certified on-premises infrastructure.

The {{site.data.keyword.framework-fs_notm}} provides different flavors of [reference architectures](/docs/framework-financial-services?topic=framework-financial-services-reference-architecture-overview) to be used as the basis for meeting the security and regulatory requirements defined in the framework. This document extends the {{site.data.keyword.vsi_is_short}} based reference architecture and provides a solution design for deploying sensitive enterprise workloads on {{site.data.keyword.powerSys_notm}} in cloud, especially for financial and regulated industries, by adopting the {{site.data.keyword.cloud_notm}} for Financial Services Framework and follow the principals and best practices of the framework.

There are different scenarios that client may want to deploy workloads on {{site.data.keyword.powerSys_notm}} in cloud, and here are some examples:
* With Power 8 and earlier versions reach End of Support (EoS), client may want to exit on-prem data center and deploy workloads to {{site.data.keyword.powerSys_notm}} in cloud, and get benefits from cloud;
* Client may have Power on-prem, but want to have dev/test environment in cloud;
* Client may have Power on-prem, but want to have HA/DR environment in cloud;
* Client may have Power on-prem, but want to burst workloads to {{site.data.keyword.powerSys_notm}} in cloud;
* New client may want to take advantages of the scalability, reliability, sustainability, and performance that Power can offer, and pay for only what they need with {{site.data.keyword.powerSys_notm}} in cloud.



## Architecture diagram
{: #architecture-diagram}

The diagram below represents the architecture for secure {{site.data.keyword.powerSys_notm}} workloads in IBM Cloud and is an extension of the [{{site.data.keyword.vsi_is_short}} reference architecture](/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-detailed-vsi) for {{site.data.keyword.cloud_notm}} for Financial Services.

![{{site.data.keyword.cloud_notm}} for Financial Services reference architecture for VPC and {{site.data.keyword.powerSys_notm}}](../images/vpc-powervs/ra-fs-powervs.svg){: caption="Figure 1. {{site.data.keyword.cloud_notm}} for Financial Services reference architecture for VPC and {{site.data.keyword.powerSys_notm}}" caption-side="bottom"}

Central to the architecture are three VPCs, which provide for separation of concerns for edge traffic control, management functionality, and consumer workloads.

Management VPC
:    Provides compute, storage, and network services to enable the client or service provider's administrators to monitor, operate, and maintain the environment.

Workload VPC
:    Provides compute, storage, and network services to support hosted applications and operations that deliver services to the consumer.

Edge VPC
:    The edge VPC is used to enhance boundary protection for the workload VPC, by allow consumers to access public facing interface through internet.

This reference architecture is an extension of the VPC reference architecture for {{site.data.keyword.cloud_notm}} for Financial Services. Here are some key features:


* Supports a single tenant.
* Resides in one or more multizone regions.
* Enables access to the management VPC from the application provider's enterprise environment through {{site.data.keyword.dl_full}} or {{site.data.keyword.vpn_full}}.
* Manages traffic flow from outside IBM Cloud via Edge VPC.
* Supports multiple subnets, ACLs and Security Groups to manage traffic flow to the subnets and to the instances. Load balancers can be used.
* Supports Bastion host to manage access to internal servers. {{site.data.keyword.powerSys_notm}} instances should not be exposed directly to external world.
* Allows connectivity to IBM Cloud services that use {{site.data.keyword.vpe_full}}. Custom DNS server and proxy server can be used.
* {{site.data.keyword.cis_full}} to provide global load balancing and layer 3/4 protection against distributed denial-of-service (DDoS) attacks.
* Virtual network firewall software in the Edge VPC to provide web application firewall (WAF) protection and layer 7 protection against denial-of-service (DoS) attacks.
* {{site.data.keyword.powerSys_notm}} are located in the IBM data centers, distinct from the IBM Cloud servers with separate networks and direct-attached storage. [Power Edge Router](/docs/power-iaas?topic=power-iaas-per) (PER) (or [Cloud Connections](/docs/power-iaas?topic=power-iaas-cloud-connections) where PER is not available) provides connection between Power Virtual Servers and IBM Cloud resources.
* Power workloads can be deployed on {{site.data.keyword.powerSys_notm}} instances, cloud native workloads can be deployed in VPC, and {{site.data.keyword.tg_short}} can be used to define and control communication between resources on the IBM Cloud network. Separates enterprise traffic and IBM Cloud internal traffic via different {{site.data.keyword.tg_short}}.
* Power workloads can be deployed on {{site.data.keyword.powerSys_notm}} instances in different regions. Global {{site.data.keyword.tg_short}} can be used in this case.
* [{{site.data.keyword.hscrypto}}](/docs/power-iaas?topic=power-iaas-integrate-hpcs) or {{site.data.keyword.keymanagementserviceshort}} can be integrated with {{site.data.keyword.powerSys_notm}}.
* FS validated services are used. {{site.data.keyword.compliance_short}} provides security and compliance support.


## Design concepts
{: #design-concepts}

Following the [Architecture Framework](/docs/architecture-framework?topic=architecture-framework-intro), this document covers the following solution aspects and domains:

![heatmap](../images/vpc-powervs/heat-map-fs-powervs.svg "Current diagram"){: caption="Figure 2. Architecture design scope" caption-side="bottom"}


## Requirements
{: #requirements}

The following table outlines the requirements that are addressed in this architecture.

| Aspect | Requirements |
| -------------- | -------------- |
| Compute            | Provide properly isolated compute resources with adequate compute capacity for the applications. |
| Storage            | Provide storage that meets the application and database performance requirements. |
| Networking         | Deploy workloads in isolated environment and enforce information flow policies. \n Provide secure, encrypted connectivity to the cloud’s private network for management purposes. \n Distribute incoming application requests across available compute resources. |
| Security           | Ensure all operator actions are executed securely through a bastion host. \n Protect the boundaries of the application against denial-of-service and application-layer attacks. \n Encrypt all application data in transit and at rest to protect from unauthorized disclosure. \n Encrypt all security data (operational and audit logs) to protect from unauthorized disclosure. \n Encrypt all data using customer managed keys to meet regulatory compliance requirements for additional security and customer control. \n Protect secrets through their entire lifecycle and secure them using access control measures. \n Firewalls must be restrictively configured to prevent all traffic, both inbound and outbound, except that which is required, documented, and approved. |
| DevOps            | Delivering software and services at the speed the market demands requires teams to iterate and experiment rapidly. They must deploy new versions frequently, driven by feedback and data. |
| Resiliency         | Support application availability targets and business continuity policies. \n Ensure availability of the application in the event of planned and unplanned outages. \n Backup application data to enable recovery in the event of unplanned outages. \n Provide highly available storage for security data (logs) and backup data. |
| Service Management | Monitor system and application health metrics and logs to detect issues that might impact the availability of the application. \n Generate alerts/notifications about issues that might impact the availability of applications to trigger appropriate responses to minimize down time. \n Monitor audit logs to track changes and detect potential security problems. \n Provide a mechanism to identify and send notifications about issues found in audit logs. |
{: caption="Table 1. Requirements" caption-side="bottom"}


## Components
{: #components}

The following table outlines the products or services used in the architecture for each aspect.

| Aspects | Architecture components | How the component is used |
| -------------- | -------------- | -------------- |
| Application platforms | [{{site.data.keyword.openshiftlong}}](https://cloud.ibm.com/docs/openshift?topic=openshift-getting-started) | Deploy and secure enterprise workloads |
|  | [{{site.data.keyword.codeenginefull}}](https://cloud.ibm.com/docs/codeengine?topic=codeengine-getting-started) | Run your application, batch job, or container on a managed serverless platform |
|  | [{{site.data.keyword.registryfull}}](https://cloud.ibm.com/docs/Registry?topic=Registry-getting-started) | Securely store container images and monitor their vulnerabilities in a private registry |
| Data | [{{site.data.keyword.messagehub_full}}](https://cloud.ibm.com/docs/EventStreams/index.html?topic=EventStreams-getting-started#getting_started) | Leverage fully managed Kafka service to build intelligent applications that react to events in real time |
| Compute | [{{site.data.keyword.vsi_is_full}}](https://cloud.ibm.com/docs/vpc?topic=vpc-creating-virtual-servers&interface=ui) | Virtual machines with faster provisioning, higher performance, and enhanced isolation |
|  | [Dedicated hosts for VPC](https://cloud.ibm.com/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-about#services-compute-dedicated-hosts) | Provision single-tenant hosts that offer dedicated resources and maximum control over instance placement |
|  | [{{site.data.keyword.powerSysFull}}](https://cloud.ibm.com/docs/power-iaas?topic=power-iaas-getting-started) | Virtual Server offering on IBM Power systems |
| Storage | [{{site.data.keyword.cos_full}}](https://cloud.ibm.com/docs/cloud-object-storage?topic=cloud-object-storage-getting-started-cloud-object-storage) | Provide flexible, cost-effective, and scalable cloud storage for unstructured data |
|  | [{{site.data.keyword.block_storage_is_full}}](https://cloud.ibm.com/docs/vpc?topic=vpc-creating-block-storage&interface=ui) | Persistent storage for use as boot and data storage for Virtual Servers in a VPC network |
| Networking | [{{site.data.keyword.vpc_full}}](https://cloud.ibm.com/docs/vpc/getting-started.html) | Fully customizable, software-defined virtual network with superior isolation |
|  | [{{site.data.keyword.alb_full}}](https://cloud.ibm.com/docs/vpc?topic=vpc-load-balancers-about) | Distribute layer 7 and 4 traffic among server instances within the same region of your VPC and support Secure Sockets Layer (SSL) offloading |
|  | [{{site.data.keyword.nlb_full}}](https://cloud.ibm.com/docs/vpc?topic=vpc-network-load-balancers) | Distribute layer 4 traffic among multiple server instances within the same region of your VPC |
|  | [{{site.data.keyword.vpn_vpc_full}}](https://cloud.ibm.com/docs/vpc?topic=vpc-vpn-overview) | Connect your on-premises network to the IBM Cloud VPC network |
|  | [Client VPN for VPC](https://cloud.ibm.com/docs/vpc?topic=vpc-vpn-client-to-site-overview) | Securely connect to your IBM Cloud resources from anywhere through a client-to-site VPN server |
|  | [{{site.data.keyword.cis_full}}](https://cloud.ibm.com/docs/cis?topic=cis-getting-started) | Offers capabilities to enhance your workflow |
|  | [{{site.data.keyword.dns_full}}](https://cloud.ibm.com/docs/dns-svcs/getting-started.html) | Manage hostnames and IP addresses while limiting access to the DNS records from permitted networks only |
|  | [{{site.data.keyword.vpe_full}}](https://cloud.ibm.com/docs/vpc?topic=vpc-ordering-endpoint-gateway&interface=ui) | Delivers private connectivity through Endpoint Gateways to IBM Cloud Services utilizing client assigned IP addresses from within the VPC |
|  | [{{site.data.keyword.dl_full}}](https://cloud.ibm.com/docs/dl) | Establish and deliver connectivity to IBM Cloud |
|  | [{{site.data.keyword.tg_full}}](https://cloud.ibm.com/docs/transit-gateway?topic=transit-gateway-getting-started) | Creates secure connectivity between your networks within IBM Cloud |
| Security | [{{site.data.keyword.appid_full}}](https://cloud.ibm.com/docs/appid) | User Authentication and User Profiles for your apps |
|  | [{{site.data.keyword.hscrypto}}](https://cloud.ibm.com/docs/hs-crypto?topic=hs-crypto-get-started) | Keep Your Own Key for cloud data encryption with a dedicated key management service built on FIPS 140-2 Level 4 certified HSM |
|  | [{{site.data.keyword.keymanagementservicelong}}](https://cloud.ibm.com/docs/key-protect/index.html) | Create or manage cryptographic keys in the cloud or in Satellite to protect data at rest |
|  | [{{site.data.keyword.secrets-manager_full}}](https://cloud.ibm.com/docs/secrets-manager?topic=secrets-manager-getting-started#getting-started) | Create, lease, and centrally manage secrets that are used in your apps and services |
| DevOps | [{{site.data.keyword.contdelivery_full}}](https://cloud.ibm.com/docs/ContinuousDelivery?topic=ContinuousDelivery-getting-started) | Support DevOps best practices by using Git, issue tracking, source code vulnerability analysis, and CI/CD pipelines in the Cloud |
|  | [Toolchain](https://cloud.ibm.com/docs/apps?topic=apps-devops-toolchains) | Automates the tasks of developing and deploying your app |
| Resiliency | [IBM Cloud Backup for VPC](https://cloud.ibm.com/docs/vpc?topic=vpc-backup-service-about&interface=ui) | Provides the ability to schedule VPC block storage snapshot backups and manage retention through backup policies |
|  | [Block Storage Snapshots for VPC](https://cloud.ibm.com/docs/vpc?topic=vpc-snapshots-vpc-create&interface=ui) | Back up block storage volumes to IBM Cloud Object Storage with this regional snapshot service |
| Service management | [IBM Cloud Activity Tracker Event Routing ](https://cloud.ibm.com/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-about#services-logging-platform-events) | Collect auditable platform events that are generated by services in your IBM Cloud account |
|  | [{{site.data.keyword.compliance_full}}](https://cloud.ibm.com/docs/security-compliance?topic=security-compliance-getting-started) | Manage your security and compliance posture |
|  | [{{site.data.keyword.fl_full}}](https://cloud.ibm.com/docs/vpc?topic=vpc-flow-logs) | enable the collection, storage, and presentation of information about the Internet Protocol (IP) traffic going to and from network interfaces within your Virtual Private Cloud (VPC) |
{: caption="Table 2. Components" caption-side="bottom"}
