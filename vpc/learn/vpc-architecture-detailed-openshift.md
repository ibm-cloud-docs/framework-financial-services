---

copyright:
  years: 2020, 2022
lastupdated: "2022-07-29"

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

![Single region {{site.data.keyword.cloud_notm}} for Financial Services reference architecture for VPC with {{site.data.keyword.openshiftshort}}](../images/roks-single-region/roks-single-region-consumer-intranet.svg){: caption="Figure 1. Single region {{site.data.keyword.cloud_notm}} for Financial Services reference architecture for VPC with {{site.data.keyword.openshiftshort}}" caption-side="bottom"}

You can choose to use {{site.data.keyword.openshiftshort}} alongside (or instead of) virtual servers in either or both VPCs. Even though it is shown in the diagram as an option, it is not required to put {{site.data.keyword.openshiftshort}} in your management VPC.

## {{site.data.keyword.openshiftshort}} concepts
{: #concepts}

{{site.data.content.service-description-openshift}}

{{site.data.keyword.openshiftshort}} extends the Kubernetes platform with built-in software to enhance app lifecycle development, operations, and security. The master nodes are entirely {{site.data.keyword.IBM_notm}}'s responsibility, while there is shared responsibility for the worker nodes.

For {{site.data.keyword.cloud_notm}} for Financial Services, you should provision {{site.data.keyword.openshiftlong_notm}} in a VPC only and not in classic infrastructure.
{: important}

For more information, see [Understanding {{site.data.keyword.openshiftshort}} on IBM Cloud](/docs/openshift?topic=openshift-roks-overview).

## Next steps
{: #next-steps}

* [Setup environment for deployment and configuration](/docs/framework-financial-services?topic=framework-financial-services-shared-deployment-setup-environment).
