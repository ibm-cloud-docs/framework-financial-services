---

copyright:
  years: 2020, 2023
lastupdated: "2023-09-18"

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










