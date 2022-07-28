---

copyright:
  years: 2020, 2022
lastupdated: "2022-07-28"

keywords: 

subcollection: framework-financial-services

---

{{site.data.keyword.attribute-definition-list}}

# Accessing the public internet
{: #vpc-architecture-connectivity-to-internet}

The {{site.data.keyword.framework-fs_notm}} does not recommend connecting to hosts on the public internet nor accepting connections from the public internet. When it is necessary to do so, you need to use either public gateways or floating IP addresses.
{: shortdesc}

## Public gateways
{: #public-gateways}

A public gateway enables a subnet and all its attached virtual server instances to connect to the internet. Subnets are private by default. After a subnet is attached to the public gateway, all instances in that subnet can connect to the internet. Although each zone has only one public gateway, the public gateway can be attached to multiple subnets.


See [Use a public gateway for external connectivity of a subnet](/docs/vpc?topic=vpc-about-networking-for-vpc#public-gateway-for-external-connectivity) for more details.

If you are from an ISV and decide to make outbound connections to the internet, then you need to make sure you document the data flows and provide evidence that shows how any consumer data that might be leaving the boundary is secured.
{: importamt}

## Floating IP addresses
{: #floating-ips}

Floating IP addresses are IP addresses that are provided by the system and are reachable from the public internet.


See [Use a floating IP address for external connectivity of a virtual server instance](/docs/vpc?topic=vpc-about-networking-for-vpc#public-gateway-for-external-connectivity) for more details.

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

* [Working with virtual servers in VPC reference architecture](/docs/allowlist/framework-financial-services?topic=framework-financial-services-shared-compute-vsi)
