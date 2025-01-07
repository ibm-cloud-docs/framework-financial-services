---

copyright:
  years: 2020, 2025
lastupdated: "2025-01-07"

keywords: 

subcollection: framework-financial-services

---

{{site.data.keyword.attribute-definition-list}}

# Working with virtual servers in VPC reference architecture
{: #shared-compute-vsi}

[{{site.data.keyword.vsi_is_short}}](/docs/vpc?topic=vpc-about-advanced-virtual-servers) is an infrastructure-as-a-service (IaaS) offering that gives you access to all of the benefits of VPC, including network isolation, security, and flexibility. You can quickly provision instances with high network performance. When you provision an instance, you select a profile that matches the amount of memory and compute power that you need for the application that you plan to run on the instance. Instances are available on the x86 architecture.
{: shortdesc}

You can optionally use [dedicated hosts for VPC](/docs/vpc?topic=vpc-creating-dedicated-hosts-instances). You can create a dedicated host to carve out a single-tenant compute node, free from users outside of your organization. Within that dedicated space, you can create virtual server instances according to your needs. Additionally, you can create dedicated host groups that contain dedicated hosts for a specific purpose. Because a dedicated host is a single-tenant space, only users within your account that have the required permissions can create instances on the host.

Dedicated hosts are highly recommended when you use virtual servers --  particularly for any parts of your application that process regulated data and keep it in memory.
{: tip}

## Deployment
{: #deployment}

1. (Optional) Create a dedicated host if you want to carve out a single-tenant compute node for your virtual servers. See [Creating dedicated hosts and groups](/docs/vpc?topic=vpc-creating-dedicated-hosts-instances) for more details.

   Dedicated hosts are highly recommended, particularly for any parts of your application that process regulated data and keep it in memory.
   {: note}

2. Create one or more virtual servers.
   * If using dedicated hosts, see the following for more details:
      * [Creating instances on dedicated hosts](/docs/vpc?topic=vpc-creating-instance-on-dh)
   * If not using dedicated hosts, see the following for more details:
      * [Planning for instances](/docs/vpc?topic=vpc-vsi_best_practices)

  

  

  

## Next steps
{: #next-steps}

* If you plan to use {{site.data.keyword.openshiftshort}}, see [Working with {{site.data.keyword.openshiftlong_notm}}](/docs/framework-financial-services?topic=framework-financial-services-shared-containers-openshift).
* Otherwise, see [Audit logging of {{site.data.keyword.cloud_notm}} events](/docs/framework-financial-services?topic=framework-financial-services-shared-logging-audit).



<!--
## Next steps
{: #next-steps}

If you plan to use [{{site.data.keyword.openshiftlong_notm}}:
  
* Explore a more detailed view of the [VPC reference architecture with {{site.data.keyword.openshiftshort}}](/docs/framework-financial-services?topic=framework-financial-services-shared-create-vpc-vsi-openshift)

If you don't plan to use {{site.data.keyword.openshiftlong_notm}}, you can skip ahead to learn more about connectivity:

* [Connectivity to management VPC](/docs/framework-financial-services?topic=framework-financial-services-shared-connectivity-management)
* [Connectivity via bastion host](/docs/framework-financial-services?topic=framework-financial-services-shared-connectivity-bastion)
* [Connectivity to workload VPC](/docs/framework-financial-services?topic=framework-financial-services-shared-connectivity-workload)
-->

<!--
To learn how to create VPC resources, see these tutorials:

* [Using the {{site.data.keyword.cloud_notm}} console to create VPC resources](/docs/vpc?topic=vpc-creating-a-vpc-using-the-ibm-cloud-console)
* [Using the CLI to create VPC resources](/docs/vpc?topic=vpc-creating-a-vpc-using-cli)
* [Using the REST APIs to create VPC resources](/docs/vpc?topic=vpc-creating-a-vpc-using-the-rest-apis)
* [Using Terraform: Provisioning a virtual server instance in a VPC](/docs/terraform?topic=terraform-getting-started#sample_vpc_config)

For a general overview of the VPC infrastructure and related compute, networking and storage concepts, see the following topics:

* [About Virtual Private Cloud](/docs/vpc?topic=vpc-about-vpc#about-vpc)
* [About networking for VPC](/docs/vpc?topic=vpc-about-networking-for-vpc)
* [About virtual server instances for VPC](/docs/vpc?topic=vpc-about-advanced-virtual-servers)
* [About storage for VPC](/docs/vpc?topic=vpc-block-storage-about)
* [Understanding IaaS basics](/docs/cloud-infrastructure?topic=cloud-infrastructure-getting-started-tutorial)
* [{{site.data.keyword.cloud_notm}} monitoring services](/docs/cloud-infrastructure?topic=cloud-infrastructure-monitoring)

* [Networking](/docs/vpc?topic=vpc-about-networking-for-vpc)
* [Compute](/docs/vpc?topic=vpc-about-advanced-virtual-servers)
* [Storage](/docs/vpc?topic=vpc-block-storage-about)

For more information, see [Create a dedicated host](/apidocs/vpc#create-dedicated-host).

For details about the `$vpc_api_endpoint` and `$iam_token` variables, see the Authentication and Endpoint URLs sections in [Virtual Private Cloud API Introduction](/apidocs/vpc#about-vpc-api).

## Next steps
{: #dh-next-steps}

When you have a dedicated host created, you can start provisioning virtual server instances on the dedicated host. For more information, see [Creating instances on dedicated hosts](/docs/vpc?topic=vpc-creating-instance-on-dh). 

[Understanding your responsibilities when using Virtual Private Cloud](/docs/vpc?topic=vpc-responsibilities-vpc)

## Solution tutorials

* [Public frontend and private backend in a Virtual Private Cloud](/docs/solution-tutorials?topic=solution-tutorials-vpc-public-app-private-backend)
* [Deploy isolated workloads across multiple locations and zones](/docs/solution-tutorials?topic=solution-tutorials-vpc-multi-region)
* [Use a VPC/VPN gateway for secure and private on-premises access to cloud resources](/docs/solution-tutorials?topic=solution-tutorials-vpc-site2site-vpn)
* [Install software on virtual server instances in VPC](/docs/solution-tutorials?topic=solution-tutorials-vpc-app-deploy)
* [Securely access remote instances with a bastion host](/docs/solution-tutorials?topic=solution-tutorials-vpc-secure-management-bastion-server) <!-- TODO -- I need to remind myself why this bastion solution is not an option? -->
