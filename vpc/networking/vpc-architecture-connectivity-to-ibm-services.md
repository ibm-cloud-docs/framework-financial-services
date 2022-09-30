---

copyright:
  years: 2020, 2022
lastupdated: "2022-09-30"

keywords: 

subcollection: framework-financial-services

---

{{site.data.keyword.attribute-definition-list}}

# Connectivity to {{site.data.keyword.cloud_notm}} services with private endpoints
{: #vpc-architecture-connectivity-to-services}

{{site.data.keyword.cloud_notm}} services should be used only over private routes. Private routes are not accessible or reachable over the internet. By using the {{site.data.keyword.cloud_notm}} private endpoints feature, you can protect your data from threats from the public network and logically extend your private network.
{: shortdesc}

When inside a VPC, this private access can be accomplished by using a [virtual private endpoint (VPE)](/docs/vpc?topic=vpc-about-vpe) to map a VPC IP address to the {{site.data.keyword.cloud_notm}} service. VPEs are virtual IP interfaces that are bound to an endpoint gateway created on a per service, or service instance, basis (depending on the service operation model). The endpoint gateway is a virtualized function that scales horizontally, is redundant and highly available, and spans all availability zones of your VPC. Endpoint gateways enable communications from virtual server instances within your VPC and {{site.data.keyword.cloud_notm}} service on the private backbone. VPE for VPC gives you the experience of controlling all the private addressing within your cloud.

The [VPE supported services](/docs/vpc?topic=vpc-vpe-supported-services) page list all of the {{site.data.keyword.cloud_notm}} services that support VPE and provide links that describe the private hosts to use and any special instructions that might be needed. Not only does this list include the Financial Services Validated IaaS and PaaS services that are available, but they also include a number of platform services, such as [{{site.data.keyword.iamshort}} (IAM)](/apidocs/iam-access-groups#endpoint-urls).

Private endpoints should be used whether you are accessing a service by using the CLI, API, or Terraform. For more information, see:

* [Securing your connection when using the {{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-service-connection)
* `visibility` input parameter in [Configuring the IBM Cloud Provider plug-in](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-provider-reference#provider-parameter-ov)



## Creating endpoint gateways
{: #create-vpes}

* Create endpoint gateways for the management and workload cluster. For more information, see [Creating an endpoint gateway](/docs/vpc?topic=vpc-ordering-endpoint-gateway).

## Related controls in {{site.data.keyword.framework-fs_notm}} 
{: #related-controls}

{{site.data.content.related-controls-disclaimer}}


| Family              | Control                                           |
|---------------------|---------------------------------------------------|
| Access Control (AC) | [AC-20 Use of External Information Systems](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-ac-20) |
| Security Assessment and Authorization (CA) | [CA-3 System Interconnections](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-ca-3) |
| System and Communications Protection (SC)  | [SC-5 Denial of Service Protection](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-5)    \n [SC-7 Boundary Protection](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-7) \n [SC-7(4) Boundary Protection &#124; External Telecommunications Services](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-7.4) \n [SC-7 (5) Boundary Protection &#124; Deny By Default - Allow By Exception](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-7.5) \n [SC-7 (10) Boundary Protection &#124; Prevent Exfiltration](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-7.10) |
{: caption="Table 1. Related controls in {{site.data.keyword.framework-fs_notm}}" caption-side="top"}

## Next steps
{: #next-steps}

* [Connectivity to public internet](/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-connectivity-to-internet)
