---

copyright:
  years: 2020, 2025
lastupdated: "2025-03-22"

keywords:

subcollection: framework-financial-services

---

{{site.data.keyword.attribute-definition-list}}

# Connecting application provider to the management VPC
{: #vpc-architecture-connectivity-management}

The management VPC should be accessed only by you, the application provider. It's important that the connection be secure to avoid bad actors gaining access and conducting malicious operations. There are two options to enable this connectivity from your on-premises enterprise network: [{{site.data.keyword.dl_short}}](/docs/dl?topic=dl-dl-about) and [{{site.data.keyword.vpn_vpc_short}}](/docs/vpc?topic=vpc-using-vpn). Alternatively, if you want to support connectivity without going through your enterprise network, you can deploy your own full tunnel client-to-site VPN solution. After a connection is established, operators can [complete actions through a bastion host](/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-connectivity-bastion) in the management VPC.
{: shortdesc}

Operators who are connecting to the on-premises enterprise network from offsite (such as their home) should connect to the enterprise network only by using a full tunnel client-to-site VPN solution. After connected to the enterprise network through a full tunnel, they can access the management VPC to perform their duties.
{: note}

## {{site.data.keyword.dl_short}}
{: #direct-link}

{{site.data.keyword.dl_short}} is the most secure way to enable connectivity from the application provider's on-premises environment to the management VPC. The speed and reliability of {{site.data.keyword.dl_short}} extends your organization’s data center network and offers more consistent, higher-throughput connectivity, keeping traffic within the {{site.data.keyword.cloud_notm}} network. When using {{site.data.keyword.dl_short}}, a private [{{site.data.keyword.alb_full}} (ALB)](/docs/vpc?topic=vpc-load-balancers) is used to distribute traffic among multiple server instances within the same region of your VPC.

The following diagram shows the {{site.data.keyword.dl_short}} connection pattern.

![Application provider on-premises to management VPC by using {{site.data.keyword.dl_short}}](../images/network-connectivity/provider-to-management-vpc/vpc-architecture-provider-on-prem-to-management-vpc-DL-fsv2.0.1.svg){: caption="Application provider on-prem to management VPC using {{site.data.keyword.dl_short}}" caption-side="bottom"}

For more information, see:

* [Interconnecting your VPC that uses {{site.data.keyword.cloud_notm}} offerings](/docs/vpc?topic=vpc-interconnectivity)
* [Getting started with {{site.data.keyword.dl_short}}](/docs/dl/getting-started?topic=dl-get-started-with-ibm-cloud-dl)
* [Ordering {{site.data.keyword.dl_full_notm}}](/docs/dl?topic=dl-how-to-order-ibm-cloud-dl-dedicated)
* [Integrating with Virtual Private Endpoint for VPC](/docs/dl?topic=dl-vpe-connection&interface=cli)



## {{site.data.keyword.vpn_vpc_short}}
{: #vpn-gateway}

An alternative connectivity pattern is to use the {{site.data.keyword.vpn_vpc_short}} service to securely connect from your private network to the management VPC. {{site.data.keyword.vpn_vpc_short}} can be used as a static, route-based VPN or a policy-based VPN to set up an IPsec site-to-site tunnel between your VPC and your on-premises private network, or another VPC.

The following diagram shows the {{site.data.keyword.vpn_vpc_short}} connection pattern.

![Application provider on-premises to management VPC by using {{site.data.keyword.vpn_vpc_short}}](../images/network-connectivity/provider-to-management-vpc/vpc-architecture-provider-on-prem-to-management-vpc-VPN-fsv2.0.1.svg){: caption="Application provider on-prem to management VPC using {{site.data.keyword.vpn_vpc_short}}" caption-side="bottom"}

For more information, see:

* [About VPN gateways](/docs/vpc?topic=vpc-using-vpn)
* [Creating a VPN gateway](/docs/vpc?topic=vpc-vpn-create-gateway)
* [Use a VPC/VPN gateway for secure and private on-premises access to cloud resources](/docs/solution-tutorials?topic=solution-tutorials-vpc-site2site-vpn)

## Full tunnel client-to-site VPN
{: #vpn-client-to-site}

The third option for connectivity for your operators is to use a full tunnel client-to-site VPN, so they do not have to be on your on-premises network. However, {{site.data.keyword.IBM_notm}} does not provide a Financial Services Validated full tunnel client-to-site VPN solution. So, if you want to use this option, you need to deploy your own. See [Setting up full tunnel VPN with FS BIG-IP](/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-connectivity-full-tunnel-vpn) for one example of how to do this.

## Related controls in {{site.data.keyword.framework-fs_notm}}
{: #related-controls}

{{site.data.content.related-controls-disclaimer}}


| Family              | Control                                           |
|---------------------|---------------------------------------------------|
| Access Control (AC) | [AC-4 (21) Information Flow Enforcement &#124; Physical or Logical Separation of Information Flows](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-ac-4.21) \n [AC-17 Remote Access](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-ac-17) \n [AC-20 Use of External Systems](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-ac-20)  |
| Audit and Accountability (AU) | [AU-10 Non-repudiation](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-au-10)  |
| Security Assessment and Authorization (CA)  | [CA-3 Information Exchange](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-ca-3)  |
| System and Communications Protection (SC)  | [SC-7 Boundary Protection](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-7) \n [SC-7 (4) Boundary Protection &#124; External Telecommunications Services](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-7.4) \n [SC-7 (5) Boundary Protection &#124; Deny by Default - Allow by Exception](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-7.5) \n [SC-7 (10) Boundary Protection &#124; Prevent Exfiltration](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-7.10) \n [SC-8 Transmission Confidentiality and Integrity](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-8) \n [SC-8 (1) Transmission Confidentiality and Integrity &#124; Cryptographic Protection](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-8.1) \n [SC-10 Network Disconnect](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-10) \n [SC-11 Trusted Path](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-11)  |
{: caption="Related controls in {{site.data.keyword.framework-fs_notm}} [FSv2.0]" caption-side="top"}
{: #related-controls-fsv2.0}
{: tab-title="FSv2.0"}
{: tab-group="RelatedControls-1"}
{: class="simple-tab-table"}


| Family              | Control                                           |
|---------------------|---------------------------------------------------|
| Access Control (AC) | [AC-4 (21) Information Flow Enforcement &#124; Physical or Logical Separation of Information Flows](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-ac-4.21) \n [AC-17 Remote Access](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-ac-17) \n [AC-20 Use of External Systems](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-ac-20)  |
| Audit and Accountability (AU) | [AU-10 Non-repudiation](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-au-10)  |
| Security Assessment and Authorization (CA)  | [CA-3 Information Exchange](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-ca-3)  |
| System and Communications Protection (SC)  | [SC-7 Boundary Protection](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-sc-7) \n [SC-7 (4) Boundary Protection &#124; External Telecommunications Services](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-sc-7.4) \n [SC-7 (5) Boundary Protection &#124; Deny By Default - Allow By Exception](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-sc-7.5) \n [SC-7 (10) Boundary Protection &#124; Prevent Exfiltration](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-sc-7.10) \n [SC-8 Transmission Confidentiality and Integrity](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-sc-8) \n [SC-8 (1) Transmission Confidentiality and Integrity &#124; Cryptographic Protection](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-sc-8.1) \n [SC-10 Network Disconnect](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-sc-10) \n [SC-11 Trusted Path](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-sc-11)  |
{: caption="Related controls in {{site.data.keyword.framework-fs_notm}} [FSv1.1]" caption-side="top"}
{: #related-controls-fsv1.1}
{: tab-title="FSv1.1"}
{: tab-group="RelatedControls-1"}
{: class="simple-tab-table"}


## Next steps
{: #next-steps}

* [Performing operator actions through a bastion host](/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-connectivity-bastion)
