---

copyright:
  years: 2020, 2022
lastupdated: "2022-07-29"

keywords: 

subcollection: framework-financial-services

---

{{site.data.keyword.attribute-definition-list}}

# Accessing the public internet
{: #vpc-architecture-connectivity-to-internet}

The {{site.data.keyword.framework-fs_notm}} does not recommend connecting to hosts on the public internet nor accepting connections from the public internet. When it is necessary to do so, you need to use either public gateways or floating IP addresses.
{: shortdesc}

## Public gateways and floating IP addresses
{: #public-gateways}

A public gateway enables a subnet and all its attached virtual server instances to connect to the internet. Subnets are private by default. After a subnet is attached to the public gateway, all instances in that subnet can connect to the internet. Although each zone has only one public gateway, the public gateway can be attached to multiple subnets.

Floating IP addresses are IP addresses that are provided by the system and are reachable from the public internet.

If you are an application provider from a technology vendor interested in becoming [{{site.data.keyword.cloud_notm}} for Financial Services Validated](/docs/framework-financial-services?topic=framework-financial-services-about#becoming-fs-validated) and want to enable outbound connectivity to the internet, then you will need to make sure you document the data flows and provide evidence that shows how any consumer data that might be leaving the boundary is secured. Similarly, for inbound connectivity, you will need to provide evidence demonstrating how your infrastructure cannot be compromised.
{: importamt}

See [External connectivity](/docs/vpc?topic=vpc-about-networking-for-vpc#external-connectivity) for more details on public gateways and floating IP addresses.

## Related controls in {{site.data.keyword.framework-fs_notm}} 
{: #related-controls}

{{site.data.content.related-controls-disclaimer}}

| Family              | Control                                           |
|---------------------|---------------------------------------------------|
| Access Control (AC) | AC-20 Use of External Information Systems |
| Security Assessment and Authorization (CA) | CA-3 System Interconnections |
| System and Communications Protection (SC)  | SC-5 Denial of Service Protection    \n SC-7 Boundary Protection \n SC-7(4) Boundary Protection &#124; External Telecommunications Services \n SC-7 (5) Boundary Protection &#124; Deny By Default - Allow By Exception \n SC-7 (10) Boundary Protection &#124; Prevent Exfiltration |
{: caption="Table 1. Related controls in {{site.data.keyword.framework-fs_notm}}" caption-side="top"}

## Next steps
{: #next-steps}

* [Working with virtual servers in VPC reference architecture](/docs/framework-financial-services?topic=framework-financial-services-shared-compute-vsi)
