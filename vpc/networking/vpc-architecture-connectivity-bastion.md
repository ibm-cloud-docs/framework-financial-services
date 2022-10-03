---

copyright:
  years: 2020, 2022
lastupdated: "2022-09-30"

keywords: 

subcollection: framework-financial-services

---

{{site.data.keyword.attribute-definition-list}}

# Running operator actions through a bastion host
{: #vpc-architecture-connectivity-bastion}

All interactive operator actions must be run through a bastion host in the management VPC. A bastion host is a server that can be accessed through SSH, Windows Remote Desktop Protocol (RDP), or `kubectl`, but only through a {{site.data.keyword.dl_short}} or {{site.data.keyword.vpn_vpc_short}} connection. After set up, the bastion host allows a secure connection to virtual server instances or {{site.data.keyword.openshiftshort}} clusters within the management VPC and the workload VPC. Administrative tasks on the individual servers are completed by using SSH, RDP, or `kubectl`, proxied through the bastion. Access to the servers and regular internet access from the servers, for example, for software installation, is allowed only with a special maintenance security group that is attached to those servers. In addition, session auditing must be enabled to record all privileged user actions.
{: shortdesc}

After [connecting to the management VPC](/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-connectivity-management) through {{site.data.keyword.dl_short}} or {{site.data.keyword.vpn_vpc_short}}, a complete bastion solution should include the following details:

{{site.data.content.bastion-host-requirements}}





## Set up a bastion host
{: #setup}

You need to install and manage your own bastion solution within your management VPC. There are various ways a bastion solution can be implemented. For one example that uses Teleport Enterprise Edition, see [Setting up a bastion host for secure connectivity](/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-connectivity-bastion-tutorial-teleport). 

## Related controls in {{site.data.keyword.framework-fs_notm}} 
{: #next-steps}

{{site.data.content.related-controls-disclaimer}}

| Family              | Control                                           |
|---------------------|---------------------------------------------------|
| Audit and Accountability (AU) | [AU-14 Session Audit](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-au-14)  |
| Access Control (AC) | [AC-6 (9) Least Privilege &#124; Auditing Use of Privileged Functions](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-ac-6.9) \n [AC-17 Remote Access](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-ac-17)  |
| Identification and Authentication (IA) | [IA-2 (1) Identification and Authentication (Organizational Users) &#124; Network Access to Privileged Accounts](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-ia-2.1) |
{: caption="Table 1. Related controls in {{site.data.keyword.framework-fs_notm}}" caption-side="top"}

## Next steps
{: #next-steps}

* [Consumer connectivity to workload VPC](/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-connectivity-workload)
