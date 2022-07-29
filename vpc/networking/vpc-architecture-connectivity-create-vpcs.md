---

copyright:
  years: 2020, 2022
lastupdated: "2022-07-28"

keywords: 

subcollection: framework-financial-services

---

{{site.data.keyword.attribute-definition-list}}

# Creating and connecting the management and workload VPCs
{: #vpc-architecture-connectivity-create-vpcs}

After completing the work for [account setup and management](/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-account-setup), you can now create the management and workload VPCs from the [VPC reference architecture](/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-detailed-vsi) and connect them using [{{site.data.keyword.tg_short}}](/docs/transit-gateway?topic=transit-gateway-about).
{: shortdesc}

1. Create two VPCs in one of the [approved MZRs](/docs/framework-financial-services?topic=framework-financial-services-best-practices#best-practices-financial-services-regions), one for the management VPC and another for the workload VPC. See [Create a VPC](/docs/vpc?topic=vpc-creating-a-vpc-using-cli#create-a-vpc-cli) for more details. For now, you should not follow the instructions in any of the other sections in that reference.

   Do _not_ create default address prefixes when you create the VPCs. You can specify `manual` for the `--address-prefix-management` argument in the [`ibmcloud is vpc-create` command](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference#vpc-create), such as in `ibmcloud is vpc-create my-vpc --address-prefix-management manual`.
   {: tip}

1. Review [Designing an addressing plan for a VPC](/docs/vpc?topic=vpc-vpc-addressing-plan-design) to get guidance how to plan for addressing within your VPC. VPC uses [Classless Inter-Domain Routing (CIDR) notation](/docs/vpc?topic=vpc-choosing-ip-ranges-for-your-vpc) for specifying addresses.

1.  Create the address prefix for each zone in both VPCs based on the plan that you developed in the previous step. See [`ibmcloud is vpc-address-prefix-create`]](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference#vpc-address-prefix-create) for details.

1. Create the subnets for the three zones by using the CIDRs. For more information, see [Create a subnet](/docs/vpc?topic=vpc-creating-a-vpc-using-cli#create-a-subnet-cli).

   You need to specify zones. For `us-south`, the three zones are `us-south-1`, `us-south-2`, `us-south-3`. For `us-east`, the three zones are `us-east-1`, `us-east-2`, `us-east-3`. See [multizone regions](/docs/overview?topic=overview-locations#mzr-table) for more information, including the zone identifiers for other [approved regions](/docs/framework-financial-services?topic=framework-financial-services-best-practices#best-practices-financial-services-regions).
   {: tip}

1. Create security groups to define inbound and outbound traffic that's allowed for virtual server instances. For more information, see [Using security groups](/docs/vpc?topic=vpc-using-security-groups) and [Overview of network security options](/docs/openshift?topic=openshift-vpc-network-policy).

1. Configure ACLs for your subnets that do not contain virtual servers or {{site.data.keyword.openshiftshort}} clusters (for example, subnets that contain a VPN Gateway or VPEs). For more information, see [Set up network ACLs](/docs/vpc?topic=vpc-using-acls).

   Even though we recommend security groups where possible, there are subnets without virtual servers or {{site.data.keyword.openshiftshort}} clusters that require ACLs. In our example, we're referring to the subnets that contain virtual private endpoints (VPEs) only or {{site.data.keyword.vpn_vpc_short}}.

   For more information, see:

   * [Configuring ACLs and security groups for use with VPN](/docs/vpc?topic=vpc-acls-security-groups-vpn)

1. [Order {{site.data.keyword.tg_short}}](/docs/transit-gateway?topic=transit-gateway-ordering-transit-gateway).

   For additional consideration, see the following resources:

   * [Interconnect two or more VPCs in the same MZR](/docs/transit-gateway?topic=transit-gateway-about#use-case-1-interconnect-two-or-more-vpcs-in-the-same-mzr)
   * [Planning for {{site.data.keyword.tg_short}}](/docs/transit-gateway?topic=transit-gateway-helpful-tips)
   * [Team based privacy that uses IAM, VPC, {{site.data.keyword.tg_short}}, and DNS](/docs/solution-tutorials?topic=solution-tutorials-vpc-tg-dns-iam)

## Related controls in {{site.data.keyword.framework-fs_notm}} 
{: #related-controls}

{{site.data.content.related-controls-disclaimer}}

| Family              | Control                                           |
|---------------------|---------------------------------------------------|
| Access Control (AC) | AC-20 Use of External Information Systems |
| Security Assessment and Authorization (CA) | CA-3 System Interconnections |
| System and Communications Protection (SC)  | SC-2 Application Partitioning \n SC-3 Security Function Isolation \n SC-5 Denial of Service Protection \n SC-7 Boundary Protection \n SC-7(4) Boundary Protection &#124; External Telecommunications Services \n SC-7 (5) Boundary Protection &#124; Deny By Default - Allow By Exception \n SC-7 (10) Boundary Protection &#124; Prevent Exfiltration \n SC-8 Transmission Confidentiality and Integrity \n SC-8 (1) Transmission Confidentiality and Integrity &#124; Cryptographic Protection \n SC-11 Trusted Path  |
{: caption="Table 1. Related controls in {{site.data.keyword.framework-fs_notm}}" caption-side="top"}

## Next steps
{: #next-steps}

* [Application provider connectivity to management VPC](/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-connectivity-management)
