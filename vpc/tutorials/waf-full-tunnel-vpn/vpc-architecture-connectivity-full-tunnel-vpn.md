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

# Setting up full tunnel client-to-site VPN that uses F5 BIG-IP
{: #vpc-architecture-connectivity-full-tunnel-vpn}
{: toc-content-type="tutorial"}
{: toc-services="f5"}
{: toc-completion-time="2h"}

One of the approaches for [enabling connectivity to the management VPC](/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-connectivity-management) is with a client to site full tunnel VPN. Application providers who want to use this option must deploy their own solution. This tutorial shows one approach that uses [F5 BIG-IP Virtual Edition](https://www.f5.com/trials/big-ip-virtual-edition){: external} that can be used to meet {{site.data.keyword.framework-fs_notm}} requirements. Alternatively, [Client {{site.data.keyword.vpn_vpc_short}}](/docs/vpc?topic=vpc-vpn-client-to-site-overview) can be used which also meets the requirements of the {{site.data.keyword.framework-fs_notm}}.
{: shortdesc}

The following diagram shows a deployment of the VPC reference architecture with a BIG-IP full tunnel VPN deployment.

![{{site.data.keyword.cloud_notm}} for Financial Services reference architecture with BIG-IP full Tunnel VPN host ](../../images/f5-bigip/fsv2-0/vpc-single-region-f5-fsv2.0.1.svg){: caption="Single-region {{site.data.keyword.cloud_notm}} for Financial Services reference architecture for VPC with BIG-IP full tunnel VPN host" caption-side="bottom"}

## Objectives
{: #full-tunnel-vpn-solution-objectives}

The objective of this tutorial is to create a full VPN tunnel host that uses BIG-IP that provides the following functions:

* LDAP authentication
* Access only to resources on the bastion subnet, including the bastion host

BIG-IP currently cannot be integrated with App ID for authentication.  Work is ongoing with the BIG-IP team to identify a solution.
{: important}

### Before you begin
{: #setup-prereqs}

The following items are needed to deploy and configure the reference architecture with a full tunnel VPN provided by BIG-IP:

* [Provisioned and licensed BIG-IP](/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-connectivity-f5-tutorial)
* Configured LDAP

## Enable license modules on the BIG-IP
{: #vpn-license}
{: step}

1. Under **System > License**, click the **Module Allocation** tab.
1. If **Access Policy (APM)** is not enabled, enable it with the **Nominal** selection.
1. After BIG-IP restarts, log back into the BIG-IP console.

## Set up an AAA Server in BIG-IP for VPN users
{: #f5-vpn-user-database}
{: step}

For this setup, we are using an LDAP database for VPN users. BIG-IP provides several other connectors for authentication.

1. Log in to the BIG-IP configuration utility that uses the admin user account.
2. Go to **Access > Authentication > LDAP**.
3. Click **Create**.
4. Enter a name, for example, `vpn_users` and click **OK**.
5. For the **Server Connection** setting, select **Use Pool** even if you have only one LDAP server.
6. In the **Server Pool Name** field, type a name for the AAA server pool.
7. Populate the **Server Addresses** field by typing the IP address of a pool member and clicking **Add**.
8. For the **Mode** setting, select **LDAPS**.
9. In the **Service Port** field, retain the default port number for LDAPS, 636, or type the port number for the SSL service on the server.
10. In the **Admin DN** field, type the distinguished name (DN) of the user with administrator rights. Type the value in this format: `CN=administrator,CN=users,DC=sales,DC=mycompany,DC=com`
11. In the **Admin Password** field, type the administrative password for the server.
12. In the **Verify Admin Password** field, retype the administrative password for the server.
13. From the **SSL Profile (Server)** list, select an SSL server profile. You can select the default profile, **serverssl**, if you do not need a custom SSL profile.
14. Skip the **Timeout** field because it does not apply when you select **Use Pool**.
15. Click **Finished**.
16. The new LDAPS server displays on the LDAP server list.

For more information about LDAP setup as a AAA Server in BIG-IP, see the [LDAP and LDAPS Authentication](https://techdocs.f5.com/en-us/bigip-16-1-0/big-ip-access-policy-manager-authentication-methods/ldap-and-ldaps-authentication.html){: external}.

## Set up a client SSL profile
{: #f5-vpn-client-ssl-profile}
{: step}

If you have set up and configured the [web application firewall](/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-connectivity-waf-tutorial), you can skip to the next step.
{: note}

1. [Import client certificate and key to BIG-IP](/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-connectivity-waf-tutorial#client-certs-key).
1. [Configure cipher suites](/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-connectivity-waf-tutorial#f5-waf-ciphers).
1. [Create a client SSL profile](/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-connectivity-waf-tutorial#ssl-profiles).

## Create a BIG-IP VPN
{: #f5-vpn-setup}
{: step}

1. GO to the **Wizards > Device Wizards** page.
2. Select **Network Access Setup Wizard for Remote Access** and click **Next**.
3. Provide a policy name and caption, for example, **vpn_policy** and click **Next**.
4. On the Authentication page, select **Use Existing, LDAP** for server type and select the LDAP server that you previously created. Provide a user DN, for example, `uid=%{session.logon.last.username},ou=Users,dc=example,dc=com`, and click **Next**.
5. Configure the **Lease Pool** by providing an IP range from the previously allocated subnet for VPN in the VPC subnets. For example, `10.5.10.0/24`. Click **Next**.
6. Keep the compression setting set to **No compression**. For the Traffic options, ensure that **Force all traffic through tunnel** is selected. Click **Next**.
7. Keep the DNS servers set to what is populated by default and click **Next**.
8. Provide the virtual server IP to be the private IP address of the external interface, for example, `10.5.40.4`. Click **Next**.
9. Review the wizard inputs and click **Next**.
10. Confirm the summary and click **Finished**.

## Configure the VPN virtual server
{: #f5-vpn-vs-setup}
{: step}

1. After the creation of VPN, navigate to **Local Traffic > Virtual Servers** and click the name of the virtual server that is associated with the VPN.
1. If you are using the web application service, under **Service Port**, change the port number to another number other than 443 (for example, 4443).
1. Under **SSL Profile (Client)**, move the client SSL profile that you created to the section labeled **Selected** and move any other ones out from under it to the section labeled **Available**.
1. Click **Update**.

## Test the BIG-IP VPN
{: #f5-vpn-test}
{: step}

1. Access the BIG-IP WebTop by navigating to `https://&#60;floating-ip-address-on-external-interface-of-f5&#62;:&#60;port` that you defined for the VPN virtual server&#62; in your browser.
2. Follow the prompts to allow BIG-IP endpoint inspection. You might be required to install the BIG-IP endpoint inspector application/plugin, if it is not already installed. 
3. Enter the credentials for an LDAP user and click **Submit**.
4. Follow the prompts to allow BIG-IP VPN link. You might be required to install the BIG-IP VPN application/plugin, if it's not already installed.
5. A BIG-IP VPN connection details window displays.
6. You are now connected to a full tunnel VPN.
   * You have access to resources on the bastion subnet such as the bastion host. 
   * You do not have access to resources on the workload subnet. The workload resources must be accessed by using the bastion.
