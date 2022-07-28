---

copyright:
  years: 2020, 2022
lastupdated: "2022-07-28"

keywords: 

subcollection: framework-financial-services

content-type: tutorial
services: cloud-object-storage, appid, vpc
account-plan: paid
completion-time: 2h

---

{{site.data.keyword.attribute-definition-list}}

# Setting up a bastion host that uses Teleport
{: #vpc-architecture-connectivity-bastion-tutorial-teleport}
{: toc-content-type="tutorial"}
{: toc-services="cloud-object-storage, appid, vpc"}
{: toc-completion-time="2h"}

This tutorial shows you one way that can be used to meet the {{site.data.keyword.framework-fs_notm}} requirements that are related to [bastion host](/docs/allowlist/framework-financial-services?topic=framework-financial-services-vpc-architecture-connectivity-bastion). There are various ways to implement a compliant bastion solution, but we show you how to configure a bastion host in your VPC by using [Teleport Enterprise Edition](https://goteleport.com/){: external}, along with {{site.data.keyword.cos_short}} and {{site.data.keyword.appid_short_notm}} for enhanced security. You will learn how to set up a Teleport-based solution that meets the previously described {{site.data.keyword.framework-fs_notm}} requirements.
{: shortdesc}

We provide guidance, but you are solely responsible for installing, configuring, and operating {{site.data.keyword.IBM_notm}} third-party software in a way that satisfies {{site.data.keyword.framework-fs_notm}} requirements. In addition, {{site.data.keyword.IBM_notm}} does not provide support for third-party software. So, if you choose to use Teleport and encounter issues with the Teleport software that require support, you should contact Teleport.
{: important}

To implement your bastion host solution, you must complete the following high-level steps:

1. Provision an instance of {{site.data.keyword.cos_short}} and configure a bucket for storing session recordings.
2. Provision an instance of {{site.data.keyword.appid_short_notm}} and configure a SAML-based identity provider.
3. Provision a virtual server instance, install Teleport, and complete the Teleport configuration for unified access to your infrastructure.

## Before you begin
{: #setup-prereqs}

Before you deploy and configure a bastion host into a virtual server instance running on the VPC reference architecture, make sure you have completed the following prerequisites:

* Acquire a Teleport Enterprise Edition license
* Configure a VPC like *VPC 0 (Edge Network)* in the architecture diagram

![{{site.data.keyword.cloud_notm}} for Financial Services reference architecture with bastion host](../../images/bastion-host/vpc-single-region-bastion.svg){: caption="Figure 1. Single-region {{site.data.keyword.cloud_notm}} for Financial Services reference architecture for VPC with bastion host" caption-side="bottom"}



## Limitations
{: #teleport-limitations}

The following limitations currently apply for the bastion host solution that is described:

* Windows is not supported.
* RDP is not supported.

## Configure {{site.data.keyword.cos_short}}
{: #setup-install-cloud-object-storage}
{: step}

Complete the following steps to configure {{site.data.keyword.cos_short}}.

1. [Provision a {{site.data.keyword.cos_full}} instance](https://cloud.ibm.com/objectstorage/create){: external} with the `Standard Plan`.
1. Create a custom bucket for storing session recordings with the following configuration:
   * Storage class set to *Standard*
   * Resiliency set to *Regional*
   * Location set to the *region of your VPC*
   * [Retention](/docs/cloud-object-storage?topic=cloud-object-storage-immutable) set to *12 months*
1. [Generate a new set of service credentials](/docs/account?topic=account-service_credentials) with the [bucket permissions](/docs/cloud-object-storage?topic=cloud-object-storage-iam-bucket-permissions) of *Object Write* access and the advanced option of *Include HMAC Credentials* enabled.

Teleport session recordings can contain sensitive information. You should have policies to ensure that operators do not enter secrets on the command line interface. In addition, you need to employ least privilege and make sure that access to these recordings is restricted only to those employees who need the recordings to do their jobs (such as your security incident response team).
{: important}

## Configure {{site.data.keyword.appid_short_notm}}
{: #setup-install-cloud-app-id}
{: step}

1. Provision an [{{site.data.keyword.appid_full}} instance](https://cloud.ibm.com/catalog/services/app-id){: external} with the `Graduated tier Plan`.
2. If you have your own SAML-based identity provider, complete the following steps:
   1. Configure your [SAML-based identity provider](/docs/appid?topic=appid-enterprise) within App ID.
   2. Set up MFA by using a physical hardware-based security key and your identity provider.
   3. Set up the following password policies:
     * Lock user accounts after three (3) consecutive failed logon attempts within 15 minutes.
     * Lock user accounts for 30 minutes after more than three unsuccessful logon attempts. When the lockout period ends, the user can reset their password. Internal privileged accounts must remain locked until they are released by an administrator.
     * Set sessions to time out after 15 minutes of inactivity.
3. [Generate a service credential](/docs/account?topic=account-service_credentials) with the role of `Reader`.
4. From your {{site.data.keyword.appid_short_notm}} service dashboard, click **Manage Authentication > Authentication Settings** and add the [web direct URLs](/docs/appid?topic=appid-managing-idp#add-redirect-uri) of `https://<virtual server instance FQDN>:3080/v1/webapi/oidc/callback`.

## Provision virtual server instances for Teleport Enterprise
{: #setup-vsi}
{: step}

1. [Provision a virtual server instance](https://cloud.ibm.com/vpc-ext/compute/vs){: external} within each zone of the VPC cluster.  
   * Profile: cx2-4x8 with 4 vCPUs, 8 GB RAM, 8 Gbps.
   * Linux-based operating system (CentOS, Debian, RHEL, Ubuntu).
2. If your identity provider is hosted publicly and not accessible directly from the Teleport virtual server instance in the VPC, you must [provision a public gateway](/docs/vpc?topic=vpc-about-networking-for-vpc#external-connectivity).
3. Register each virtual server instance that you plan to run Teleport into a Domain Name Server.
4. Generate an SSL certificate for each of the provisioned virtual server instances or use a wildcard certificate.

## Configure a security group for your virtual server instances
{: #set-up-security-groups}
{: step}

[Configure the Security group](/docs/vpc?topic=vpc-using-security-groups) for your virtual server instances.

   | Protocol | Source type | Source | Port |
   |----------|-------------|--------|------|
   | TCP         | IP address or CIDR Block | IP address or CIDR Block value | Ports 22-22      |
   | TCP         | IP address or CIDR Block | IP address or CIDR Block value | Ports 3023-3025  |
   | TCP         | IP address or CIDR Block | IP address or CIDR Block value | Ports 3080-3080  |
   {: caption="Bastion host: Security group inbound rules" caption-side="top"}
   {: summary="This table shows the security group rules needed for the bastion server."}
   {: #simpletabtable1}
   {: tab-title="Security group inbound rules"}
   {: tab-group="SGRules"}
   {: class="simple-tab-table"}

   | Protocol | Destination type | Destination | Value |
   |------------|---------------|----------|-----------|
   | TCP        | IP address or CIDR Block | IP address or CIDR Block value |Ports Any |
   {: caption="Bastion host: Security group outbound rules" caption-side="top"}
   {: summary="This table shows the security group rules needed for the bastion server."}
   {: #simpletabtable2}
   {: tab-title="Security group outbound rules"}
   {: tab-group="SGRules"}
   {: class="simple-tab-table"}

   Add any additional security group rules that are needed for connectivity to your resources.
   {: note}

## Configure an access control list
{: #set-up-acl}
{: step}

[Configure the access control list](/docs/vpc?topic=vpc-using-acls) for the subnets that the bastion virtual server instances use to `Allow` inbound and outbound traffic. Make sure that there is a rule for each bastion virtual server instance if the ACL is used for multiple subnets.

   | Protocol | Source type | Source Value | Source Port | Destination type | Destination Value | Destination Port |
   |--------|---------------|----------|-----------|-----------|-----------|-----------|
   | TCP | IP address or CIDR Block | IP address or CIDR Block value | Any | IP address | IP address of bastion server | 22 |
   | TCP | IP address or CIDR Block | IP address or CIDR Block value | Any | IP address | IP address of bastion server | 3023-3025 |
   | TCP | IP address or CIDR Block | IP address or CIDR Block value | Any | IP address | IP address of bastion server | 3080 |
   {: caption="Bastion host: Access control list inbound rules" caption-side="top"}
   {: summary="This table shows the Access control list rules needed for the bastion server."}
   {: #simpletabtable01}
   {: tab-title="ACL inbound rules"}
   {: tab-group="ACLRules"}
   {: class="simple-tab-table"}

   | Protocol | Source type | Source Value | Source Port | Destination type | Destination Value | Destination Port |
   |------------|---------------|----------|-----------|-----------|-----------|-----------|
   | TCP | IP address | IP address of bastion server | 22 | IP address or CIDR Block| IP address or CIDR Block value| Any |
   | TCP | IP address | IP address of bastion server | 3023-3025 | IP address or CIDR Block| IP address or CIDR Block value| Any |
   | TCP | IP address | IP address of bastion server | 3080 | IP address or CIDR Block| IP address or CIDR Block value| Any |
   {: caption="Bastion host: Access control list outbound rules" caption-side="top"}
   {: summary="This table shows the Access control list rules needed for the bastion server."}
   {: #simpletabtable02}
   {: tab-title="ACL outbound rules"}
   {: tab-group="ACLRules"}
   {: class="simple-tab-table"}

   Add any additional access control list rules that are needed for connectivity to your resources.
   {: note}

## Configure virtual server instances for Teleport Enterprise
{: #setup-install-teleport}
{: step}

With your virtual server instances provisioned and set-up with a security group and ACL, you can continue configuring the instance and installing Teleport. Then, complete the Teleport configuration for unified access to your infrastructure.  

1. Connect to your virtual server instance by using [SSH](/docs/vpc?topic=vpc-vsi_is_connecting_linux).
1. Download [Teleport Enterprise version 7.1.0](https://get.gravitational.com/teleport-ent-v7.1.0-linux-amd64-bin.tar.gz){: external} and extract its contents. Copy the following packages to the `/usr/local/bin` directory:
   * `cp <path of extracted contents>/teleport /usr/local/bin`
   * `cp <path of extracted contents>/tctl /usr/local/bin`
   * `cp <path of extracted contents>/tsh /usr/local/bin`
1. Create the directory `/var/lib/teleport` on the file system of your virtual server instance. This directory might exist depending on your installation method.
1. Copy your license file from Teleport Enterprise into the file `/var/lib/teleport/license.pem`.
1. Copy your SSL certificate for the virtual server instance into `/var/lib/teleport/`. You should have a file for the certificate and key. If you have an intermediate certificate, make sure it is after your certificate. 
1. Create the file `teleport.yaml` in the directory `/etc` and copy the sample content from the following example into the file. Ensure the appropriate changes where noted with `<variables>`. 

   ```
   #teleport.yaml
   teleport:
     nodename: <fqdn of node>
     data_dir: /var/lib/teleport
     log:
       output: stderr
       severity: DEBUG 
     storage:
       audit_sessions_uri: "s3://<Bucket>?endpoint=<COS Endpoint>&region=ibm"

   auth_service:
     enabled: "yes"
     listen_addr: 0.0.0.0:3025
     authentication:
       type: oidc
       local_auth: false
     license_file: /var/lib/teleport/license.pem
     message_of_the_day: "<banner message to be displayed to a user that must be acknowledged before logging into the bastion>"

   ssh_service:
     enabled: "yes"
     commands:
     - name: hostname
       command: [hostname]
       period: 1m0s
     - name: arch
       command: [uname, -p]
       period: 1h0m0s

   proxy_service:
     enabled: "yes"
     listen_addr: 0.0.0.0:3023
     web_listen_addr: 0.0.0.0:3080
     tunnel_listen_addr: 0.0.0.0:3024
     https_cert_file: /var/lib/teleport/<SSL Certificate PEM File>
     https_key_file: /var/lib/teleport/<SSL Certificate Key PEM File>
   ```
   {: codeblock}

1. Create the file `/etc/systemd/system/teleport.service` with the following example content.  Make sure to enter your HMAC credentials that you generated previously in this procedure. For more information, see [Systemd Unit File](https://goteleport.com/docs/admin-guide/#systemd-unit-file){: external}. Ensure the appropriate changes where noted with `<variables>`.

   ```
   [Unit]
   Description=Teleport Service
   After=network.target

   [Service]
   Type=simple
   Restart=on-failure
   Environment=AWS_ACCESS_KEY_ID="<HMAC access_key_id>"
   Environment=AWS_SECRET_ACCESS_KEY="<HMAC secret_access_key>"
   ExecStart=/usr/local/bin/teleport start --config=/etc/teleport.yaml --pid-file=/run/teleport.pid
   ExecReload=/bin/kill -HUP $MAINPID
   PIDFile=/run/teleport.pid

   [Install]
   WantedBy=multi-user.target
   ```
   {: codeblock}

1. Issue the following systemctl to load the teleport service.
   ```
   sudo systemctl daemon-reload
   sudo systemctl start teleport
   sudo systemctl enable teleport
   ```
1. Some operating systems have open firewalls but if your operating system limits traffic, you must add firewall rules to allow TCP traffic for ports 3023, 3024, 3025, and 3080.  CentOS is an example of an operating system that blocks traffic by default.
1. Start the Teleport process `systemctl start teleport`.  
1. Create a Teleport Role.  For more information, see [Create Teleport Roles](https://goteleport.com/docs/enterprise/sso/oidc/#create-teleport-roles){: external}. For more information on settings for a Teleport role, see [Teleport Access Control Reference](https://goteleport.com/docs/access-controls/reference/){: external}.
   1. Create the file `role.yaml` within the directory `/var/lib/teleport`. The following sample role provides full access to the system.  You can also create roles within the Teleport web console under *Teams -> Roles*.
   
   ```
   #example role
   kind: "role"
   version: "v3"
   metadata:
     name: "teleport-admin"
   spec:
     options:
       max_connections: 3
       cert_format: standard
       client_idle_timeout: 15m
       disconnect_expired_cert: no
       enhanced_recording:
       - command
       - network
       forward_agent: true
       max_session_ttl: 1h
       port_forwarding: false
     allow:
       logins: [root]
       node_labels:
         "*": "*"
       rules:
       - resources: ["*"]
         verbs: ["*"]
   ```
   {: codeblock}

1. Create the OIDC connector.
   1. Create the file `oidc.yaml` within the directory `/var/lib/teleport` by using the following example content. Ensure the appropriate changes where noted with `<variables>`. For more information, see [OIDC Authentication](https://goteleport.com/docs/enterprise/sso/oidc/){: external}.  

   ```
   #oidc connector
   kind: oidc
   version: v2
   metadata:
     name: appid
   spec:
     redirect_url: "https://<virtual server instance DNS FQDN>:3080/v1/webapi/oidc/callback"
     client_id: "<Client ID from AppID Service Credentials>"
     display: AppID
     client_secret: "<secret from AppID Service Credentials>"
     issuer_url: "<oauthServerUrl from AppID Service Credentials>"
     scope: ["openid", "email"]
     claims_to_roles: 
     - {claim: "email", value: "<Email Address>", roles: ["teleport-admin"]}
   ```
   {: codeblock}

   Example claim names can be `email`, `family_name`, `given_name`, or `name`.  The value is what that claims value will be set to.

1. Using the [tctl](https://goteleport.com/docs/cli-docs/){: external} to apply the yamls:
   * `tctl create /var/lib/teleport/role.yaml` 
   * `tctl create /var/lib/teleport/oidc.yaml` 
1. Set up forwarding of Teleport logs and system logs. Teleport logs are located in the directory `/var/lib/teleport` and system logs in `/var/logs`.  

Logs must be forwarded to an operational logging solution. For more information, see [operational logging](/docs/allowlist/framework-financial-services?topic=framework-financial-services-vpc-architecture-logging-operational)
{: important}

## Log in to the bastion host
{: #setup-login}
{: step}

You can log in to the bastion host through the web console or tsh client as described in the following sections.

### Log in through the the web console

1. Access the web console on port 3080.
  
   `https://<fqdn of node>:3080`

1. Start a terminal session under *Servers*. There should be a single server with a *connect* button. Click *connect* and select the user that you would like to log in with. 

### Log in through the tsh client

1. Install the [Teleport client tool tsh](https://goteleport.com/docs/user-manual/#installing-tsh){: external}.
1. [Log in using tsh](https://goteleport.com/docs/user-manual/#logging-in){: external}.

    `tsh login --proxy=<fqdn of telport server>:3080`

1. Run shell or execute a command on a remote SSH node by using the [tsh ssh command](https://goteleport.com/docs/cli-docs/#tsh-ssh){: external}

    `tsh ssh <[user@]host>`

## Install tools
{: #setup-tooling}
{: step}

Now that the bastion host is set up and configured, you can install the tools that are needed to interact with your infrastructure, such as:

1. [IBM Cloud CLI](/docs/cli?topic=cli-install-ibmcloud-cli) and associated [plug-ins](/docs/cli?topic=cli-plug-ins)
1. [{{site.data.keyword.openshiftshort}} CLI](https://docs.openshift.com/container-platform/4.6/cli_reference/openshift_cli/getting-started-cli.html){: external}

## Remove SSH port from security group and access control list
{: step}

Now that Teleport is installed and setup, remove SSH port 22 from the allowed list of ports within the configured security group and access control list that is assigned to the virtual server and subnet.

## Related controls in {{site.data.keyword.framework-fs_notm}} 
{: #controls}

See the [related controls for bastion host](/docs/allowlist/framework-financial-services?topic=framework-financial-services-vpc-architecture-connectivity-bastion#next-steps).

