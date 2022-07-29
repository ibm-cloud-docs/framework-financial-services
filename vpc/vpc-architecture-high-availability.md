---

copyright:
  years: 2020, 2022
lastupdated: "2022-07-28"

keywords: 

subcollection: framework-financial-services

---

{{site.data.keyword.attribute-definition-list}}

# High availability for VPC reference architecture
{: #vpc-architecture-high-availability}

The [High availability (HA) overview](/docs/framework-financial-services?topic=framework-financial-services-shared-high-availability) desribes how HA is important for all cloud-based applications. In this article, you will learn more specifics about HA in the VPC reference architecture.

## Your workloads on VPC infrastructure
{: #your-workloads}

### Workloads in multizone regions
{: #your-workloads-multizone}

#### Distribute workloads across zones
{: #your-workloads-multizone-distribution}

Recall that all VPCs exist in a single region referring to the geographic area in which a VPC is deployed. The [supported regions](/docs/framework-financial-services?topic=framework-financial-services-best-practices#best-practices-financial-services-regions) for the {{site.data.keyword.cloud_notm}} for Financial Services are multizone, meaning they have multiple isolated physical data centers that host compute, network, and storage resources.

It is recommended to distribute your workloads across multiple zones in a given region as the first step in creating an HA solution. That is, if one zone goes down, the other zones can continue to support the workload. You should use [{{site.data.keyword.alb_full}} (ALB)](/docs/vpc?topic=vpc-load-balancers) to distribute your traffic across zones in a region.

#### Scale based on resource demands
{: #your-workloads-multizone-scaling}

Even with multiple zones, you should take additional steps to ensure that your environment can be scaled out based on the resources used by your workloads to enhance resiliency.

For example, by using [Auto Scale for VPC](/docs/vpc?topic=vpc-creating-auto-scale-instance-group), you can improve performance and costs by dynamically creating virtual server instances to meet the demands of your environment. You set scaling policies that define your desired average utilization for metrics like CPU, memory, and network usage. The policies that you define determine when virtual server instances are added or removed from your instance group.

For {{site.data.keyword.openshiftshort}}, you can use the `cluster-autoscaler` add-on to scale the worker pools in your cluster automatically to increase or decrease the number of worker nodes in the worker pool based on the sizing needs of your scheduled workloads. The `cluster-autoscaler` add-on is based on the [Kubernetes Cluster-Autoscaler project](https://github.com/kubernetes/autoscaler/tree/master/cluster-autoscaler). See [Autoscaling clusters](/docs/openshift?topic=openshift-ca) for more information.

In addition, it is possible to autoscale your pods. See [Scaling apps](/docs/openshift?topic=openshift-update_app#app_scaling) for more information.

### Workloads in multiple regions
{: #your-workloads-multiple-regions}

Despite the steps you take to leverage multiple zones in a region and increase resiliency with auto scaling, it is still possible for an entire region to be taken out of service. For example, a natural disaster like a hurricane, tornado, or earthquake could knock out multiple zones in a region.

So, it is also recommended to establish and configure an alternate processing site in at least one geographically separate multizone region to ensure the continuation of secure system operation. Then, in addition to spreading your workloads across zones in a region, you can distribute your workloads across regions. The alternative region(s) you use must be chosen from the [list of regions](/docs/framework-financial-services?topic=framework-financial-services-best-practices#best-practices-financial-services-regions) which are Financial Services Validated. However, your consumers must be able to opt out of having their data stored in the alternate region based on their data residency requirements.

The diagram below shows an expansion of the VPC reference architecture to use multiple regions. The key to making this work is to use the [global load balancing functionality](/docs/dns-svcs?topic=dns-svcs-global-load-balancers) in {{site.data.keyword.dns_short}}. The global load balancer offers HA and geographical distribution of your traffic, based on the health of your origin servers and the geographical region where the user request originates. So, if one region becomes unhealthy, then traffic would get routed to the next closest healthy region.


![VPC architecture in multiple regions](./images/vpc-multi-region/vpc-multi-region-consumer-intranet.svg){: caption="Figure 1. VPC architecture in multiple regions" caption-side="bottom"}

For completeness, the diagram below shows the VPC architecture spread across multiple regions when {{site.data.keyword.openshiftshort}} is used.


![{{site.data.keyword.openshiftshort}} on VPC multiple regions](./images/roks-multi-region/roks-multi-region-consumer-intranet.svg){: caption="Figure 2. {{site.data.keyword.openshiftshort}} on VPC multiple regions" caption-side="bottom"}

See the following tutorials for more information:

* [Strategies for resilient applications](/docs/solution-tutorials?topic=solution-tutorials-strategies-for-resilient-applications)
* [Deploy isolated workloads across multiple locations and zones](/docs/solution-tutorials?topic=solution-tutorials-vpc-multi-region)

## HA for {{site.data.keyword.cloud_notm}} services
{: #ibm-cloud-services}

Your high availability strategy needs to consider all of the {{site.data.keyword.cloud_notm}} services in your deployment. See [High availabiltty for {{site.data.keyword.cloud_notm}} services](/docs/framework-financial-services?topic=framework-financial-services-shared-high-availability#ibm-cloud-services) for more information.

## Related controls in {{site.data.keyword.framework-fs_notm}}
{: #related-controls}

See [related controls for high availability](/docs/framework-financial-services?topic=framework-financial-services-shared-high-availability#related-controls).

## Next steps
{: #next-steps}

- [Development processes and software integrity](/docs/framework-financial-services?topic=framework-financial-services-shared-development-processes)
