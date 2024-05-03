---

copyright:
  years: 2020, 2024
lastupdated: "2024-05-03"

keywords:

subcollection: framework-financial-services

---

{{site.data.keyword.attribute-definition-list}}

# Single-region {{site.data.keyword.cloud_notm}} for Financial Services reference architecture for VPC with {{site.data.keyword.openshiftshort}}
{: #vpc-architecture-detailed-openshift}

If you want to use containers, you can add [{{site.data.keyword.openshiftlong_notm}}](/docs/openshift?topic=openshift-roks-overview) to your VPC. Except for the addition of {{site.data.keyword.openshiftshort}}, you use the same architectural patterns and components that were described for the [Single-region {{site.data.keyword.cloud_notm}} for Financial Services reference architecture for VPC with {{site.data.keyword.vsi_is_short}}](/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-detailed-vsi).
{: shortdesc}

The following diagram shows a more detailed view of both the management and workload VPCs when {{site.data.keyword.openshiftshort}} is introduced.

## Architecture diagram
{: #vpc-openshift-diagram}

![Single region {{site.data.keyword.cloud_notm}} for Financial Services reference architecture for VPC with {{site.data.keyword.openshiftshort}}](../images/roks-single-region/roks-single-region-consumer-intranet-v2.svg){: caption="Figure 1. Single region {{site.data.keyword.cloud_notm}} for Financial Services reference architecture for VPC with {{site.data.keyword.openshiftshort}}" caption-side="bottom"}

You can choose to use {{site.data.keyword.openshiftshort}} alongside (or instead of) virtual servers in either or both VPCs. Even though it is shown in the diagram as an option, it is not required to put {{site.data.keyword.openshiftshort}} in your management VPC.

## {{site.data.keyword.openshiftshort}} concepts
{: #concepts}

[{{site.data.keyword.openshiftlong_notm}}](/docs/openshift?topic=openshift-roks-overview) is a managed offering to create your own {{site.data.keyword.openshiftshort}} cluster of compute hosts to deploy and manage containerized apps on {{site.data.keyword.cloud_notm}}. {{site.data.keyword.openshiftlong_notm}} provides intelligent scheduling, self-healing, horizontal scaling, service discovery and load balancing, automated rollouts and rollbacks, and secret and configuration management for your apps. Combined with an intuitive user experience, built-in security and isolation, and advanced tools to secure, manage, and monitor your cluster workloads, you can rapidly deliver highly available and secure containerized apps in the public cloud.

{{site.data.keyword.openshiftshort}} extends the Kubernetes platform with built-in software to enhance app lifecycle development, operations, and security. The master nodes are entirely {{site.data.keyword.IBM_notm}}'s responsibility, while there is shared responsibility for the worker nodes.

For {{site.data.keyword.cloud_notm}} for Financial Services, you should provision {{site.data.keyword.openshiftlong_notm}} only in a VPC and not in classic infrastructure.
{: important}

For more information, see [Understanding {{site.data.keyword.openshiftshort}}](/docs/openshift?topic=openshift-roks-overview).

## Next steps
{: #next-steps}

* [Setup environment for deployment and configuration](/docs/framework-financial-services?topic=framework-financial-services-shared-deployment-setup-environment).
