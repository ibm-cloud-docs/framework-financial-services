---

copyright:
  years: 2020, 2025
lastupdated: "2025-03-02"

keywords:

subcollection: framework-financial-services

---

{{site.data.keyword.attribute-definition-list}}

# Consumer connectivity to workload VPC
{: #vpc-architecture-connectivity-workload}

Previously, we saw how administrative access to the workload VPC can be accomplished from the bastion host in the management VPC. Now, we look at how consumers can connect to the workload VPC to access your service offering.
{: shortdesc}

## Consumer in same organization as application provider
{: #consumer-provider-same-org}

If the consumer is in the same organization that you are (such as the same financial institution), then the connection options are much the same as they are when you connect to the management VPC. That is, the consumer can connect to the workload VPC with either [{{site.data.keyword.dl_short}}](/docs/dl?topic=dl-dl-about) or [{{site.data.keyword.vpn_vpc_short}}](/docs/vpc?topic=vpc-using-vpn). This is shown in the diagram below.

![Connecting to workload VPC from on-premises with consumer in same organization as application provider](../images/vpc-single-region/vpc-single-region-consumer-intranet-v2.svg){: caption="Connecting to workload VPC from on-prem with consumer in same organization as application provider" caption-side="bottom"}


### {{site.data.keyword.dl_short}}
{: #vpc-architecture-connectivity-management-direct-link}

{{site.data.keyword.dl_short}} is the most secure way to enable connectivity from the consumer's on-premises environment to the workload VPC. The speed and reliability of {{site.data.keyword.dl_short}} extends your organization’s data center network and offers more consistent, higher-throughput connectivity, keeping traffic within the {{site.data.keyword.cloud_notm}} network. When using {{site.data.keyword.dl_short}}, a private [{{site.data.keyword.alb_full}} (ALB)](/docs/vpc?topic=vpc-load-balancers) is used to distribute traffic among multiple server instances within the same region of your VPC.

The following diagram shows the {{site.data.keyword.dl_short}} connection pattern.

![Consumer on-premises to workload VPC by using {{site.data.keyword.dl_short}}](../images/network-connectivity/consumer-to-workload-same-account/vpc-architecture-consumer-on-prem-to-provider-same-account-DL.svg){: caption="Consumer on-prem to workload VPC using {{site.data.keyword.dl_short}}" caption-side="bottom"}

For more information, see:

* [Interconnecting your VPC by using {{site.data.keyword.cloud_notm}} offerings](/docs/vpc?topic=vpc-interconnectivity)
* [Getting started with {{site.data.keyword.dl_short}}](/docs/dl/getting-started?topic=dl-get-started-with-ibm-cloud-dl)

### {{site.data.keyword.vpn_vpc_short}}
{: #vpc-architecture-connectivity-management-vpn}

An alternative connectivity pattern requires using the {{site.data.keyword.vpn_vpc_short}} service to securely connect from your private network to the management VPC. {{site.data.keyword.vpn_vpc_short}} can be used as a static, route-based VPN or a policy-based VPN to set up an IPsec site-to-site tunnel between your VPC and your on-premises private network, or another VPC.



The following diagram shows the {{site.data.keyword.vpn_vpc_short}} connection pattern.

![Consumer on-premises to workload VPC using {{site.data.keyword.vpn_vpc_short}}](../images/network-connectivity/consumer-to-workload-same-account/vpc-architecture-consumer-on-prem-to-provider-same-account-VPN.svg){: caption="Consumer on-prem to workload VPC using {{site.data.keyword.vpn_vpc_short}}" caption-side="bottom"}

For more information, see:

* [About VPN gateways](/docs/vpc?topic=vpc-using-vpn)
* [Creating a VPN gateway](/docs/vpc?topic=vpc-vpn-create-gateway)

## Consumer in different organization than application provider
{: #consumer-provider-different-org}



### Connecting from public internet
{: #consumer-provider-public-internet}

There are many valid cases where you might want to allow consumers to access your service through the public internet. The base architecture can be adapted to securely enable this type of access as shown in the following diagram which introduces a new edge VPC. The request from the consumer gets routed through a global load balancer outside of the edge VPC, through a web application firewall (WAF) in the edge VPC, and then to the public application load balancer within the workload VPC. This is shown in the following diagram.

![Detailed VPC reference architecture with edge VPC for the {{site.data.keyword.cloud_notm}} for Financial Services](../images/f5-bigip/vpc-single-region-edge-v2.svg){: caption="Detailed VPC reference architecture with edge VPC" caption-side="bottom"}

#### Global load balancer
{: #consumer-provider-public-internet-glb}

One option for global load balancing outside of the edge VPC is {{site.data.keyword.cis_full}} ({{site.data.keyword.cis_short_notm}}), powered with Cloudflare. {{site.data.keyword.cis_short_notm}} provides a fast, highly performant, reliable, and secure internet service for customers running their business on {{site.data.keyword.cloud_notm}}.

For more information, see the following resources:

* [Getting started with {{site.data.keyword.cis_short_notm}}](/docs/cis?topic=cis-getting-started)
* [Configuring a global load balancer](/docs/cis?topic=cis-global-load-balancer-glb-concepts)
* [Protecting TCP traffic (Range)](/docs/cis?topic=cis-cis-range&interface=ui)

#### Edge VPC with web application firewall
{: #consumer-provider-public-internet-waf}

The edge VPC is used to enhance boundary protection for both the management VPC and the workload VPC. For public internet access to the workload VPC, a WAF in the edge VPC is use to protect web applications by filtering and monitoring internet web traffic. A WAF can prevent attacks exploiting a web application's known vulnerabilities.

{{site.data.keyword.IBM_notm}} does not currently offer a Financial Services Validated solution for WAF. So, you need to install and manage your own WAF within your edge VPC. One option for WAF is to use F5 BIG-IP. See the tutorial [Setup WAF with F5 BIG-IP](/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-connectivity-waf-tutorial) for more details.
{: tip}

For [management VPC connectivity](/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-connectivity-management), your operators can connect to the environment from your on-premises network (with {{site.data.keyword.dl_short}} or {{site.data.keyword.vpn_vpc_short}}) or through a full-tunnel client-to-site VPN. In practice, all three zones in the edge VPC would be the same, but for illustrative purposes, each zone in the edge VPC box depicts one of the three scenarios for operator connectivity:

* Zone 1 - Connectivity with {{site.data.keyword.dl_short}}, so neither a full-tunnel client-to-site VPN nor {{site.data.keyword.vpn_vpc_short}} is needed.
* Zone 2 - Connectivity from the application provider is with {{site.data.keyword.vpn_vpc_short}}, so a full-tunnel client-to-site VPN is not needed.
* Zone 3 - Connectivity from the application provider is through a full-tunnel client-to-site VPN, so {{site.data.keyword.vpn_vpc_short}} is not needed.

Finally, the bastion can be put either in the edge VPC or the management VPC. If you place it in the edge VPC, then the management VPC becomes optional if you are not deploying any other management tools.

#### Application load balancer in workload VPC
{: #consumer-provider-public-internet-alb}

Use {{site.data.keyword.cloud}} {{site.data.keyword.alb_full}} (ALB) to distribute traffic among multiple server instances within the same region of your VPC. For more information, see the following resources:

* [About {{site.data.keyword.cloud_notm}} {{site.data.keyword.alb_full}}](/docs/vpc?topic=vpc-load-balancers&interface=ui)
* [Creating an {{site.data.keyword.alb_full}}](/docs/vpc?topic=vpc-load-balancers&interface=ui)

## Related controls in {{site.data.keyword.framework-fs_notm}}
{: #related-controls}

{{site.data.content.related-controls-disclaimer}}

| Family              | Control                                           |
|---------------------|---------------------------------------------------|
| System and Communications Protection (SC)  | [SC-5 Denial-of-service Protection](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-5)    \n [SC-7 Boundary Protection](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-7) \n [SC-7 (4) Boundary Protection &#124; External Telecommunications Services](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-7.4) \n [SC-7 (5) Boundary Protection &#124; Deny by Default - Allow by Exception](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-7.5) \n [SC-7 (10) Boundary Protection &#124; Prevent Exfiltration](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-7.10) \n [SC-8 Transmission Confidentiality and Integrity](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-8) \n [SC-8 (1) Transmission Confidentiality and Integrity &#124; Cryptographic Protection](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-8.1) \n [SC-11 Trusted Path](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-11)  |
{: caption="Related controls in {{site.data.keyword.framework-fs_notm}} [FSv2.0]" caption-side="top"}
{: #related-controls-fsv2.0}
{: tab-title="FSv2.0"}
{: tab-group="RelatedControls-1"}
{: class="simple-tab-table"}


| Family              | Control                                           |
|---------------------|---------------------------------------------------|
| System and Communications Protection (SC)  | [SC-5 Denial of Service Protection](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-sc-5)    \n [SC-7 Boundary Protection](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-sc-7) \n [SC-7(4) Boundary Protection &#124; External Telecommunications Services](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-sc-7.4) \n [SC-7 (5) Boundary Protection &#124; Deny By Default - Allow By Exception](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-sc-7.5) \n [SC-7 (10) Boundary Protection &#124; Prevent Exfiltration](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-sc-7.10) \n [SC-8 Transmission Confidentiality and Integrity](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-sc-8) \n [SC-8 (1) Transmission Confidentiality and Integrity &#124; Cryptographic Protection](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-sc-8.1) \n [SC-11 Trusted Path](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-sc-11)  |
{: caption="Related controls in {{site.data.keyword.framework-fs_notm}} [FSv1.1]" caption-side="top"}
{: #related-controls-fsv1.1}
{: tab-title="FSv1.1"}
{: tab-group="RelatedControls-1"}
{: class="simple-tab-table"}


## Next steps
{: #next-steps}

* [Connectivity to {{site.data.keyword.cloud_notm}} services with private endpoints](/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-connectivity-to-services)
