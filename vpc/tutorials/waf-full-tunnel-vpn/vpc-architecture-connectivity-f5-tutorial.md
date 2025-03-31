---

copyright:
  years: 2020, 2025
lastupdated: "2025-03-31"

keywords:

subcollection: framework-financial-services

content-type: tutorial
services: f5
account-plan: paid
completion-time: 2h
---

{{site.data.keyword.attribute-definition-list}}


# Deploying and configuring F5 BIG-IP
{: #vpc-architecture-connectivity-f5-tutorial}
{: toc-content-type="tutorial"}
{: toc-services="f5"}
{: toc-completion-time="2h"}

If you need to have a firewall within the VPC, it can be achieved using [F5 BIG-IP](https://www.f5.com/trials/big-ip-virtual-edition){: external}. In this tutorial, you will learn how to provision an instance of BIG-IP. This is a prerequisite for using BIG-IP to [set up a full tunnel client-to-site VPN](/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-connectivity-full-tunnel-vpn) or [enable a WAF](/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-connectivity-waf-tutorial) in a way that meets {{site.data.keyword.framework-fs_notm}} requirements.
{: shortdesc}

Guidance is provided here, but you are solely responsible for installing, configuring, and operating {{site.data.keyword.IBM_notm}} third-party software in a way that satisfies {{site.data.keyword.framework-fs_notm}} requirements. In addition, {{site.data.keyword.IBM_notm}} does not provide support for third-party software.
{: important}

The architecture diagram shows a deployment of the VPC reference architecture with an instance of BIG-IP in the edge/transit VPC.

![{{site.data.keyword.cloud_notm}} for Financial Services reference architecture with BIG-IP WAF](../../images/f5-bigip/fsv2-0/vpc-single-region-f5-fsv2.0.1.svg){: caption="Single-region {{site.data.keyword.cloud_notm}} for Financial Services reference architecture for VPC with BIG-IP" caption-side="bottom"}

The architecture diagram shows a BIG-IP installation with four interfaces within 4 different VPC subnets: management, external, workload, and bastion. Depending on the services that you need and how you name your VPC subnets, you must update the values within the tutorial.
{: tip}

## Objectives
{: #f5-objectives}

The objective of this solution is to provision an instance of BIG-IP that is:

* Licensed with a bring-your-own license (BYOL).
* Configure management UI cipher suite.
* Replace the configuration utility's self-signed device certificate.

## Before you begin
{: #f5-prereqs}

You need the following items to deploy and configure this reference architecture with an instance of BIG-IP:

* [BIG-IP license](https://www.f5.com/products/get-f5){: external}
* Edge/transit VPC with a number of subnets that depend on the services that you plan to configure with the BIG-IP. All of these subnets must be in the same zone as your BIG-IP installation, and must be replicated for each zone BIG-IP is deployed to. Note of the ID's of your provisioned subnets because they are  used later in this tutorial:
    * 3 subnets: If you are using only the BIG-IP full tunnel VPN service (external, management, bastion)
    * 3 subnets: If you are using only the BIG-IP WAF (external, management, workload)
    * 4 subnets: If you are using the BIG-IP full tunnel VPN and WAF service (external, management, workload, bastion)
* [VPC SSH key](/docs/vpc?topic=vpc-ssh-keys)
* Following best practices for [access management](/docs/framework-financial-services?topic=framework-financial-services-best-practices#best-practices-zero-trust) and create an access group with minimum access policies for VPC Infrastructure Services:
    * Platform access: Editor
    * Service access: Writer and IP spoofing operator

## Provision BIG-IP
{: #provision-big-ip}
{: step}

1. Install the [F5 BIG-IP Virtual Edition for VPC](https://cloud.ibm.com/catalog/content/ibmcloud_schematics_bigip_multinic_declared-1.0-d33f1544-e938-478a-b0dd-d883370f08d0-global){: external} offering from the {{site.data.keyword.cloud_notm}} catalog. In the **Parameters without default values** section, pay special attention to the following parameters and the values that need to be set:
   * **external_subnet_id**: ID of the external subnet
   * **internal_subnet_id**: ID of the workload subnet
   * **tmos_admin_password**: Password for the admin account that must meet the following requirements:
      * Minimum length of 15 characters
      * Required Characters: Numeric = 1, Uppercase = 1, Lowercase = 1
2. In the **Parameters with default values** section, pay special attention to the following parameters and the values that should be set:
   * **cluster_subnet_id**: ID of the bastion subnet
   * **bigip_external_floating_ip**: `true`
   * **instance_profile**:
     * If you are using BIG-IP for either full tunnel VPN or web application firewall, use a minimum profile of `cx2-4x8`.
     * If you are using BIG-IP for full tunnel VPN and web application firewall, use a minimum profile of `cx2-8x16`.
   * **tmos_image_name**: `bigip-16-1`
3. To encrypt the boot disk of the F5 you must enter a value for the optional parameter encryption_key_crn . To get the value for that parameter:
   1. In the IBM Cloud console, go to your HPCS instance page
   2. On the left side of the page select 'KMS Keys'
   3. Find which key you use to encrypt disks. SLZ normally names this key "vsi-volume"
   4. Click on the three dot menu on the right side of the row containing your key
   5. Select View Key Details
   6. Copy the entire contents of the 'Cloud Resource Name' to your clipboard and use it to populate the value for `encryption_key_crn`
4. When you click **Install**, an {{site.data.keyword.bplong_notm}} workspace is created and initialized.
5. Click **Generate Plan** in the top right.
6. After the plan is generated successfully, click **Apply Plan**.
7. Verify that the BIG-IP instance successfully deployed.

Access the BIG-IP installation through the IP address only that is contained in the management subnet (https://&#60;VSI-management-subnet-IP-address&#62;). Use an access control list and security group to limit traffic of the BIG-IP management console to the IP address of the management subnet.
{: note}

## Complete licensing for BIG-IP
{: #f5-configuration-license}
{: step}

1. Access BIG-IP configuration utility through your web browser, and login by using the *admin* user account.
1. Click the **Licensing** tab in the left menu, and then click **Activate** to complete licensing.
1. Provide the registration key, and click **Next**.
1. Click **Accept**. This restarts the BIG-IP modules and services and redirects to the login page.
1. Log in again, using the admin credentials.



## Configure management UI cipher suite
{: #f5-management-ciphersuite}
{: step}

You are responsible for meeting certain [TLS requirements](/docs/framework-financial-services?topic=framework-financial-services-shared-encryption-in-transit#shared-encryption-in-transit-tls-requirements) along with [acceptable cipher suites](/docs/framework-financial-services?topic=framework-financial-services-shared-encryption-in-transit#shared-encryption-in-transit-tls-cipher-suites).
{: important}

Next, configure the management UI to allow the acceptable cipher suites of TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384 and TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256 only. You can do this configuration only with the command line interface.

1. Log on with SSH port 22 to the management IP address of the BIG-IP.
2. Issue the following commands:

   ```
   tmsh modify /sys httpd ssl-ciphersuite 'ECDHE-RSA-AES256-GCM-SHA384:ECDHE-RSA-AES128-GCM-SHA256'
   tmsh save /sys config
   tmsh list /sys httpd ssl-ciphersuite
   ```
   {: codeblock}

3. Logout of the command line interface.

## Replacing the configuration utility's self-signed device certificate
{: #f5-configuration-ssl-cert}
{: step}

By default, the configuration utility is deployed with a self-signed certificate. You must replace the certificate according to the [TLS requirements](/docs/framework-financial-services?topic=framework-financial-services-shared-encryption-in-transit#shared-encryption-in-transit-tls-requirements).  For more information on replacing the BIG-IP self-signed certificate, see [Replacing the Configuration utility's self-signed device certificate with a CA-signed device certificate](https://support.f5.com/csp/article/K42531434){: external}.

## Accessing the F5 BIG-IP
{: #f5-access}
{: step}

All access, whether it is CLI with SSH or the configuration utility, should be through the bastion host. The following example shows how you can connect to the F5 BIG-IP configuration utility by using port forwarding with [Teleport as a bastion host](/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-connectivity-bastion-tutorial-teleport).

1. Create a [Teleport role](https://goteleport.com/docs/reference/access-controls/roles/#role-options){: external} with the option **port_forwarding** set to **true**.
1. [Log in to the bastion host with the tsh client](/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-connectivity-bastion-tutorial-teleport#log-in-via-the-tsh-client).
1. Set up a [port forwarding connection with the tsh client](https://goteleport.com/docs/connect-your-client/tsh/#port-forwarding){: external}. The following example binds port 5000 on your local machine and allows access to the BIG-IP through the browser at *https://127.0.0.1:5000*.
   ```
   tsh ssh -L 5000:<IP ADDRESS OF BIG-IP MANAGEMENT INTERFACE>:443 [user@]bastion_host
   ```
   {: codeblock}

1. Access the BIG-IP configuration utility through your browser at `https://<bind_ip>:<listen_port>`. When you access the configuration utility, the browser receives a warning because you are accessing it through `bind_ip` that you set.

Teleport does not provide session recordings through port forwarding. However, Teleport does provide audit events of the session that is associated with port forwarding, and the F5 BIG-IP does have an audit log of all administrative actions.
{: important}



## Enabling GUI audit logging
{: #f5-configuration-gui-audit-logging}
{: step}

By default, the audit logging for the GUI is disabled. Perform the following steps to enable it.

1. Access BIG-IP configuration utility through your web browser, and login by using the *admin* user account.
1. Under **System > Logs > Configuration**, click the **Options**.
1. Selec **Enable** for the GUI setting in the Audit Logging area.
1. Click **Update**.

## Setup BIG-IP routes
{: #f5-routes}
{: step}

BIG-IP routes control the flow of traffic and determine which interface to use. By routing to the proper interface, you are able to use access controls lists (ACLs) and security groups to control traffic to and from the subnet and interface. Route traffic coming into the interface labeled *External* (for example, 10.5.40.5) and route it to your workload or application (for example, 10.40.0.0/18).

1. Go to **Network > Routes**.
1. Click **Add**.
1. In the **Properties** field complete the following information:
   * **Name**: Name that you want to use for the route
   * **Description**: Description you want to use for the route
   * **Destination**: IP prefix of your target workload or application (for example, 10.40.0.0)
   * **Netmask**: Netmask prefix of your target workload or application (for example, 255.255.192.0)
   * **Resource**: `Use Gateway`
   * **Gateway Address**: Gateway IP address of the interface that is labeled **Workload**. This is the second address in the CIDR range of the subnet labeled **Workload** (for example, 10.5.50.1).

Routes are also needed for Bastion connectivity with full tunnel VPN that uses the interface that is labeled **Bastion** and for the F5 management UI that uses the interface that is labeled **MGMT**.
{: note}

## Access control lists and security groups
{: #f5-acl-sg}
{: step}

You are responsible for isolating and controlling traffic from each VPC subnet and interface. By using [access control lists](/docs/vpc?topic=vpc-using-acls) and [security groups](/docs/vpc?topic=vpc-using-security-groups), you can control all incoming and outgoing traffic.

The example security groups and access and control list's based on the architecture diagram and the F5 interfaces (management, external, workload, bastion) in Zone 1 of the edge/transit VPC. The example assumes that the VPN is on port 4443 and your workload target port is 443. Include any other services that the F5 BIG-IP will need connectivity to. It is encouraged to update to meet your needs and architecture.
{: note}

### F5 management interface

Restrict SSH port 22 and F5 configuration utility port 443 into the BIG-IP to the bastion host.
{: note}

#### Security group

| Protocol | Source type | Source | Port |
|----------|-------------|--------|------|
| TCP         | CIDR Block | CIDR Block value from Bastion host subnet (for example, 10.5.70.0/24) | Ports 22-22      |
| TCP         | CIDR Block | CIDR Block value from Bastion host subnet (for example, 10.5.70.0/24) | Ports 443-443    |
{: caption="F5 management interface: Security group inbound rules" caption-side="top"}
{: summary="This table shows the security group rules that are needed for the F5 management interface."}
{: #simpletabtable01}
{: tab-title="Security group inbound rules"}
{: tab-group="ManagementSGRules"}
{: class="simple-tab-table"}

#### Access control list

| Allow or Deny | Protocol | Source type | Source Value | Source Port | Destination type | Destination Value | Destination Port |
|----------------|--------|---------------|----------|-----------|-----------|-----------|-----------|
| Allow | TCP | IP or CIDR | CIDR Block value from Bastion host subnet (for example, 10.5.70.0/24) | Any | IP address | IP address of F5 management interface (for example, 10.5.30.4) | 22 |
| Allow | TCP | IP or CIDR | CIDR Block value from Bastion host subnet (for example, 10.5.70.0/24) | Any | IP address | IP address of F5 management interface (for example, 10.5.30.4) | 443 |
| Deny | ALL | Any | | Any | Any | | |
{: caption="F5 management interface: access control list inbound rules" caption-side="top"}
{: summary="This table shows the inbound access control list rules that are needed for the F5 management interface."}
{: #simpletabtable02}
{: tab-title="ACL inbound rules"}
{: tab-group="ManagementACLRules"}
{: class="simple-tab-table"}

| Allow or Deny | Protocol | Source type | Source Value | Source Port | Destination type | Destination Value | Destination Port |
|---------------|------------|---------------|----------|-----------|-----------|-----------|-----------|
| Allow | TCP | IP or CIDR | IP address of F5 management interface (for example, 10.5.30.4) | 22 | IP or CIDR | CIDR Block value from Bastion host subnet (for example, 10.5.70.0/24) | Any |
| Allow | TCP | IP or CIDR | IP address of F5 management interface (for example, 10.5.30.4) | 443 | IP or CIDR | CIDR Block value from Bastion host subnet (for example, 10.5.70.0/24) | Any |
| Deny | ALL | Any | | Any | Any | | |
{: caption="F5 management interface: access control list outbound rules" caption-side="top"}
{: summary="This table shows the outbound access control list rules that are needed for the F5 management interface."}
{: #simpletabtable03}
{: tab-title="ACL outbound rules"}
{: tab-group="ManagementACLRules"}
{: class="simple-tab-table"}

### F5 external interface

Restrict incoming traffic on port 443 to CIS global load balancers allowlisted IP addresses. The allowlisted IP addresses might periodically change and should be updated.
{: note}

#### Security group

| Protocol | Source type | Source | Port |
|----------|-------------|--------|------|
| TCP         | [CIS global load balancers allowlisted IP addresses](/docs/cis?topic=cis-cis-allowlisted-ip-addresses) | | Ports 443-443      |
| TCP         | Any | | Ports 4443-4443   |
{: caption="F5 external interface: Security group inbound rules" caption-side="top"}
{: summary="This table shows the security group rules that are needed for the F5 external interface."}
{: #simpletabtable04}
{: tab-title="Security group inbound rules"}
{: tab-group="ExternalSGRules"}
{: class="simple-tab-table"}

#### Access control list

| Allow or Deny | Protocol | Source type | Source Value | Source Port | Destination type | Destination Value | Destination Port |
|----------------|--------|---------------|----------|-----------|-----------|-----------|-----------|
| Allow | TCP | Any | | Any | IP address | IP address of F5 external interface (for example, 10.5.40.5) | 443 |
| Allow | TCP | Any | | Any | IP address | IP address of F5 external interface (for example, 10.5.40.5) | 4443 |
| Deny | ALL | Any | | Any | Any | | |
{: caption="F5 external interface: access control list inbound rules" caption-side="top"}
{: summary="This table shows the inbound access control list rules that are needed for the F5 external interface."}
{: #simpletabtable05}
{: tab-title="ACL inbound rules"}
{: tab-group="ExternalACLRules"}
{: class="simple-tab-table"}

| Allow or Deny | Protocol | Source type | Source Value | Source Port | Destination type | Destination Value | Destination Port |
|---------------|------------|---------------|----------|-----------|-----------|-----------|-----------|
| Allow | TCP | IP or CIDR | IP address of F5 external interface (for example, 10.5.40.5) | 443 | Any | | Any |
| Allow | TCP | IP or CIDR | IP address of F5 external interface (for example, 10.5.40.5) | 4443 | Any | | Any |
| Deny | ALL | Any | | Any | Any | | |
{: caption="F5 external interface: access control list outbound rules" caption-side="top"}
{: summary="This table shows the outbound access control list rules that are needed for the F5 external interface."}
{: #simpletabtable06}
{: tab-title="ACL outbound rules"}
{: tab-group="ExternalACLRules"}
{: class="simple-tab-table"}

### F5 workload interface

#### Security group

| Protocol | Destination type | Destination | Port |
|----------|-------------|--------|------|
| TCP         | CIDR block | CIDR block of the subnet your workload or application is located (for example, 10.40.0.0/18) | Ports 443-443      |
| TCP         | CIDR block | CIDR block of the subnet your workload or application is located (for example, 10.50.0.0/18) | Ports 443-443      |
| TCP         | CIDR block | CIDR block of the subnet your workload or application is located (for example, 10.60.0.0/18) | Ports 443-443      |

{: caption="F5 workload interface: Security group outbound rules" caption-side="top"}
{: summary="This table shows the security group rules that are needed for the F5 workload interface."}
{: #simpletabtable07}
{: tab-title="Security group outbound rules"}
{: tab-group="InternalSGRules"}
{: class="simple-tab-table"}

#### Access control list

| Allow or Deny | Protocol | Source type | Source Value | Source Port | Destination type | Destination Value | Destination Port |
|----------------|--------|---------------|----------|-----------|-----------|-----------|-----------|
| Allow | TCP | IP or CIDR | CIDR block of the subnet your workload or application is located (for example, 10.40.0.0/18) | 443 | IP or CIDR | IP address of F5 workload interface (for example, 10.5.50.5) | Any |
| Allow | TCP | IP or CIDR | CIDR block of the subnet your workload or application is located (for example, 10.50.0.0/18) | 443 | IP or CIDR | IP address of F5 workload interface (for example, 10.5.50.5) | Any |
| Allow | TCP | IP or CIDR | CIDR block of the subnet your workload or application is located (for example, 10.60.0.0/18) | 443 | IP or CIDR | IP address of F5 workload interface (for example, 10.5.50.5) | Any |
| Deny | ALL | Any | | Any | Any | | |
{: caption="F5 workload interface: access control list inbound rules" caption-side="top"}
{: summary="This table shows the inbound access control list rules that are needed for the F5 workload interface."}
{: #simpletabtable08}
{: tab-title="ACL inbound rules"}
{: tab-group="InternalACLRules"}
{: class="simple-tab-table"}

| Allow or Deny | Protocol | Source type | Source Value | Source Port | Destination type | Destination Value | Destination Port |
|---------------|------------|---------------|----------|-----------|-----------|-----------|-----------|
| Allow | TCP | IP or CIDR | IP address of F5 workload interface (for example, 10.5.50.5) | Any | IP or CIDR | CIDR block of the subnet your workload or application is located (for example, 10.40.0.0/18) | 443 |
| Allow | TCP | IP or CIDR | IP address of F5 workload interface (for example, 10.5.50.5) | Any | IP or CIDR | CIDR block of the subnet your workload or application is located (for example, 10.50.0.0/18) | 443 |
| Allow | TCP | IP or CIDR | IP address of F5 workload interface (for example, 10.5.50.5) | Any | IP or CIDR | CIDR block of the subnet your workload or application is located (for example, 10.60.0.0/18) | 443 |
| Deny | ALL | Any | | Any | Any | | |
{: caption="F5 workload interface: access control list outbound rules" caption-side="top"}
{: summary="This table shows the outbound access control list rules that are needed for the F5 workload interface."}
{: #simpletabtable09}
{: tab-title="ACL outbound rules"}
{: tab-group="InternalACLRules"}
{: class="simple-tab-table"}

### F5 bastion interface

#### Security group

| Protocol | Destination type | Destination | Port |
|----------|-------------|--------|------|
| TCP         | CIDR block | CIDR Block value from Bastion subnet (for example, 10.5.70.0/24) | Ports 3023-3025      |
| TCP         | CIDR block | CIDR Block value from Bastion subnet (for example, 10.5.70.0/24) | Ports 3080-3080      |

{: caption="F5 bastion interface: Security group outbound rules" caption-side="top"}
{: summary="This table shows the security group rules that are needed for the F5 bastion interface."}
{: #simpletabtable10}
{: tab-title="Security group outbound rules"}
{: tab-group="BastionSGRules"}
{: class="simple-tab-table"}

#### Access control list

| Allow or Deny | Protocol | Source type | Source Value | Source Port | Destination type | Destination Value | Destination Port |
|----------------|--------|---------------|----------|-----------|-----------|-----------|-----------|
| Allow | TCP | IP or CIDR | CIDR Block value from Bastion subnet (for example, 10.5.70.0/24) | 3023-3025 | IP or CIDR | IP address of F5 bastion interface (for example, 10.5.60.5) | Any |
| Allow | TCP | IP or CIDR | CIDR Block value from Bastion subnet (for example, 10.5.70.0/24) | 3080-3080 | IP or CIDR | IP address of F5 bastion interface (for example, 10.5.60.5) | Any |
| Deny | ALL | Any | | Any | Any | | |
{: caption="F5 Bastion interface: access control list inbound rules" caption-side="top"}
{: summary="This table shows the inbound access control list rules that are needed for the F5 bastion interface."}
{: #simpletabtable11}
{: tab-title="ACL inbound rules"}
{: tab-group="BastionACLRules"}
{: class="simple-tab-table"}

| Allow or Deny | Protocol | Source type | Source Value | Source Port | Destination type | Destination Value | Destination Port |
|---------------|------------|---------------|----------|-----------|-----------|-----------|-----------|
| Allow | TCP | IP or CIDR | IP address of F5 bastion interface (for example, 10.5.60.5) | Any | IP or CIDR | CIDR Block value from Bastion subnet (for example, 10.5.70.0/24) | 3023-3025 |
| Allow | TCP | IP or CIDR | IP address of F5 bastion interface (for example, 10.5.60.5) | Any | IP or CIDR | CIDR Block value from Bastion subnet (for example, 10.5.70.0/24) | 3080-3080 |
| Deny | ALL | Any | | Any | Any | | |
{: caption="F5 bastion interface: access control list outbound rules" caption-side="top"}
{: summary="This table shows the outbound access control list rules that are needed for the F5 bastion interface."}
{: #simpletabtable12}
{: tab-title="ACL outbound rules"}
{: tab-group="BastionACLRules"}
{: class="simple-tab-table"}


## Next steps

* [Set up a full tunnel client-to-site VPN with BIG-IP](/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-connectivity-full-tunnel-vpn)
* [Enable a WAF with BIG-IP](/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-connectivity-waf-tutorial)
