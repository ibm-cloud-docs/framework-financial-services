---

copyright:
  years: 2020, 2022
lastupdated: "2022-03-23"

keywords: 

subcollection: framework-financial-services

content-type: tutorial
services: cis, vpc, f5
account-plan: paid
completion-time: 4h
---

{{site.data.keyword.attribute-definition-list}}

# Setting up a web application firewall with F5 BIG-IP
{: #vpc-architecture-connectivity-waf-tutorial}
{: toc-content-type="tutorial"}
{: toc-services="cis, vpc, f5"}
{: toc-completion-time="4h"}

There are several architectural approaches to enabling [consumer connectivity to the workload VPC](/docs/allowlist/framework-financial-services?topic=framework-financial-services-vpc-architecture-connectivity-workload). When using [public internet access to the workload VPC](/docs/allowlist/framework-financial-services?topic=framework-financial-services-vpc-architecture-connectivity-workload), a web application firewall (WAF) is required. A WAF helps protect web applications by filtering and monitoring internet traffic. This tutorial guides you through one approach to enabling WAF by using [{{site.data.keyword.cis_full}}](/docs/cis?topic=cis-getting-started) ({{site.data.keyword.cis_short_notm}}) and [F5 BIG-IP Virtual Edition](https://www.f5.com/trials/big-ip-virtual-edition){: external}. Global load balancing along with a WAF forms the basis for you to meet {{site.data.keyword.framework-fs_notm}} requirements for boundary protection.
{: shortdesc}

Guidance is provided, but you are solely responsible for installing, configuring, and operating {{site.data.keyword.IBM_notm}} third-party software in a way that satisfies {{site.data.keyword.framework-fs_notm}} requirements. In addition, {{site.data.keyword.IBM_notm}} does not provide support for third-party software.
{: important}

The following architecture diagram shows a deployment of the VPC reference architecture that uses BIG-IP for WAF.

![{{site.data.keyword.cloud_notm}} for Financial Services reference architecture with BIG-IP WAF](../../images/f5-bigip/vpc-single-region-edge.svg){: caption="Figure 1. Single-region {{site.data.keyword.cloud_notm}} for Financial Services reference architecture for VPC with BIG-IP" caption-side="bottom"}

There are two main components to this solution:

* {{site.data.keyword.cis_short_notm}} provides global load balancing and layer 3/4 protection against distributed denial-of-service (DDoS) attacks.
* BIG-IP that provides WAF protection and layer 7 protection against denial-of-service (DoS) attacks.

{{site.data.keyword.cis_short_notm}} is not Financial Services Validated. Because of this, TLS connections must not be terminated in {{site.data.keyword.cis_short_notm}} and should be configured only for pass-through connections. {{site.data.keyword.cis_short_notm}} global load balancers must be configured with the **proxy** configuration setting value of **off**. {{site.data.keyword.cis_short_notm}} Use range applications sto provide DDoS protection in front of global load balancers. 
{: note}

## Network path
{: #network-path}

The following diagram shows the flow of network traffic from a client application passing through {{site.data.keyword.cis_short_notm}} and BIG-IP to server-side applications running in {{site.data.keyword.openshiftshort}}.

![{{site.data.keyword.cloud_notm}} Network path from client application through {{site.data.keyword.cis_short_notm}} and BIG-IP to provider's {{site.data.keyword.openshiftshort}} application](../../images/f5-bigip/cis-flow.svg){: caption="Figure 2. Network path from client application to {{site.data.keyword.openshiftshort}} application passing through {{site.data.keyword.cis_short_notm}} and BIG-IP." caption-side="bottom"}

The network flow goes through the following steps:

1. Client applications connect to a {{site.data.keyword.cis_short_notm}} Range application through a public hostname that is exposed by {{site.data.keyword.cis_short_notm}}. This connection is treated as a TCP connection (not an HTTPS connection) by {{site.data.keyword.cis_short_notm}} and passes through {{site.data.keyword.cis_short_notm}} without TLS termination. The Range application provides DDoS protection.
2. A {{site.data.keyword.cis_short_notm}} global load balancer routes traffic to the WAF provided by BIG-IP instances. The global load balancer has a public hostname, but it is not advertised for use.
3. The first TLS termination occurs in BIG-IP running in {{site.data.keyword.cloud_notm}}. BIG-IP treats the connection as an HTTPS connection. The application's public certificate is used by BIG-IP for TLS termination.
4. Network traffic is reencrypted between BIG-IP and the application. The {{site.data.keyword.openshiftshort}} ingress controller is configured for pass through so that it does not terminate the TLS connection. The final TLS termination is done by the application.

The following table summarizes the fully qualified domain names (FQDNs) used by the three layers:

| Host description | Example FQDN | DNS | How Protected |
| -----------------| ------------ | --- | ------------- |
| Range application hostname (user application hostname) | `www.application.example.com` | Managed by {{site.data.keyword.cis_short_notm}}, no explicit record | {{site.data.keyword.cis_short_notm}} Layer 3/4 |
| GLB hostname | `<uuid>-example.com` | Managed by {{site.data.keyword.cis_short_notm}}, no explicit record, public network | Obfuscated, BIG-IP Virtual Server accepts only the Range Application Host Header |
| BIG-IP hostname | `f5-app.example.com` | A record maps to public external IP of BIG-IP, public network | Security Group to [only allow traffic from {{site.data.keyword.cis_short_notm}}](/docs/cis?topic=cis-cis-allowlisted-ip-addresses) |
| {{site.data.keyword.openshiftshort}} application hostname (not externalized) | `application-internal.example.com` | CNAME record maps to private-router ALB, private network | Only accessible within network and protected by BIG-IP firewall. Security group restricts traffic to accept traffic only from BIG-IP. |

## Before you begin
{: #waf-setup-prereqs}

The following items are needed to deploy and configure the reference architecture with {{site.data.keyword.cis_short_notm}} and BIG-IP to create a WAF:

* [Provisioned and licensed BIG-IP](/docs/allowlist/framework-financial-services?topic=framework-financial-services-vpc-architecture-connectivity-f5-tutorial)
* Registered domain for {{site.data.keyword.cis_short_notm}}
* Certificate for your registered domain (can be a wildcard certificate)
* Intermediate certificate for your server-side application

## Provision {{site.data.keyword.cis_short_notm}}
{: #provision-cis}
{: step}

1. Provision an instance of [{{site.data.keyword.cis_short_notm}}](https://cloud.ibm.com/catalog/services/internet-services){: external} that uses the **Enterprise Usage** plan.
2. Click **Create** to provision the service.

## Configure {{site.data.keyword.cis_short_notm}} TLS Mode
{: #configure-cis-TLS}
{: step}

1. [Register your domain name with {{site.data.keyword.cis_short_notm}}](/docs/cis?topic=cis-multi-domain-support).
2. Go to **Security > TLS** set the TLS encryption mode to [**End-to-end flexible**](/docs/cis?topic=cis-cis-tls-options#tls-encryption-modes-end-to-end-flexible). **End-to-End CA signed** must not be used because it causes the TLS connection to be terminated in {{site.data.keyword.cis_short_notm}}.

## Configure {{site.data.keyword.cis_short_notm}} Global Load Balancer
{: #configure-cis-global-loadl-balancer}
{: step}

When a global load balancer is created in {{site.data.keyword.cis_short_notm}}, it is automatically assigned the fully qualified domain name (FQDN) of `<global load balancer name>.<domain>`. This hostname is registered in public DNS. However, this hostname should not be used for the application because it lacks DDoS protection. Use he hostname that is assigned to the Range application in the next step for the application.

1. Go to **Reliability > Global load balancers**.
1. Click **Create**.
1. Set **Enable** to **On**.
1. Set **Proxy** to **Off**. You must do this to avoid terminating the TLS connection in {{site.data.keyword.cis_short_notm}}.
1. Enter a **Name** for the load balancer. This name is not used externally, but is referenced by the Range application.
1. Set values for **TTL** and **Traffic steering** that are appropriate for your application.
1. Click **Create**.
1. Configure an [origin pool](/docs/cis?topic=cis-glb-features-pools) for the new load balancer that contains the BIG-IP endpoints.
1. Configure appropriate [health checks](/docs/cis?topic=cis-glb-features-healthchecks).

You can use the following curl command to verify that the global load balancer is working correctly.

```
$ curl -v https://<global load balancer name>.<domain>/<path>
```
{: codeblock}

The verbose output from the curl command shows the signer of the SSL certificate that is terminating TLS. The signer should be the SSL certificate that is within the BIG-IP. If the signer of the certificate includes `Cloudflare`, then {{site.data.keyword.cis_short_notm}} is incorrectly terminating the TLS connection. Check that the global load balancer's **Proxy** is turned off and that the **TLS encryption mode** is set to **End-to-End CA Flexible**.

## Configure {{site.data.keyword.cis_short_notm}} Range Application
{: #configure-cis-range-application}
{: step}

DDoS protection in {{site.data.keyword.cis_short_notm}} is enabled by using a Range application. The Range application becomes the application's front end and sets the FQDN for the service.

1. Go to **Security > Range**.
1. Click **Create**.
1. Select **TCP** as the Type. This enables pass-through without terminating TLS.
1. Set the **Name** to the hostname that you want to expose for the application. The application's FQDN becomes `<name>.<CIS domain>`.
1. Set the **Edge port** to the appropriate value. For HTTPS traffic, set the edge port to 443.
1. Select an appropriate value for **Edge IP connectivity**.
1. Set the **Origin** to **Load balancer**.
    1. Set **Load balancer** to the name of the global load balancer created in the previous step.
    1. Set the **Port** to the port that is exposed by the global load balancer.
1. You can enable **IP firewall** if you want to restrict the IP ranges that can connect to the service. If IP filtering is not needed, disable **IP firewall**.
1. **TLS Termination** must be set to **off** to prevent the TLS connection from being terminated in {{site.data.keyword.cis_short_notm}}.
1. Leave **TLS mode** greyed out.
1. Set **Proxy protocol** to **Off**.

You can use the following curl command to verify that the combination of the Range Application and Global Load Balancer is working correctly.

```
$ curl -v https://<Range application name>.<domain>/<path>
```
{: codeblock}

The verbose output from the curl command shows the signer of the SSL certificate where TLS is being terminated. The signer should be the signer of the application's SSL certificate. If the signer of the certificate includes "Cloudflare", then {{site.data.keyword.cis_short_notm}} is incorrectly terminating the TLS connection. Check that the Global Load Balancer's **Proxy** is configured to **Off** and that the **TLS encryption mode** is set to **End-to-End CA Flexible**.

When the edge application is working, it is important to prevent users from using the global load balancer directly. This is especially important to prevent a DDoS attack against the global load balancer. To prevent users from accessing the global load balancer directly, create a policy within BIG-IP that allows traffic to pass that specifies the FQDN of the Range application only.

## Enable license modules on the BIG-IP
{: #f5-waf-license}
{: step}

1. Under **System > License**, click the **Module Allocation** tab.
1. If **Application Security (ASM)** is not enabled, enable it with the **Nominal** selection.
1. After BIG-IP restarts, log back into the BIG-IP console.

## Import client certificate and key to BIG-IP
{: #client-certs-key}
{: step}

### Import client key
{: #client-key}

1. In the BIG-IP console, go to **System > Certificate Management > Traffic Certificate Management**.
1. Click **Import**.
1. Select **Key** from the **Import Type**.
1. Type in a name for your key under the option **Key Name**.
1. Either upload your key or paste your key under the option **Key Source**.
1. Select the proper security type.
1. Click **Import**.

### Import client certificate
{: #client-cert}

1. In the BIG-IP console, go to **System > Certificate Management > Traffic Certificate Management**.
1. Click **Import**.
1. Select **Certificate** from the **Import Type**.
1. In the **Certificate Name** field, enter a name for your certificate.
1. Either upload your certificate or paste your certificate under the option **Certificate Source**.
1. Click **Import**.


BIG-IP feature enhancement for HPCS integration is being worked on.
{: note}

## Import server-side intermediate certificates to BIG-IP
{: #server-certs-import}
{: step}

For each server certificate that you have, complete the following actions:

1. In the console, go to **System > Certificate Management > Traffic Certificate Management**.
1. Click **Import**.
1. Select **Certificate** from the **Import Type**.
1. In the **Certificate Name** field, enter a name for your certificate.
1. Either upload your certificate or paste your certificate under the option **Certificate Source**.
1. Click **Import**.


BIG-IP feature enhancement for HPCS integration is being worked on.
{: note}

## Configure ciphers
{: #f5-waf-ciphers}
{: step}

In this step, you create a cipher policy and group to limit the ciphers that are able to be used for a secure connection.

For more information on TLS requirements and acceptable cipher suites, see [Data encryption in transit](/docs/allowlist/framework-financial-services?topic=framework-financial-services-vpc-architecture-encryption-in-transit).
{: important}

### Create a cipher policy
{: #cipher-policy}

1. In the BIG-IP console, go to **Local Traffic > Ciphers > Rules**.
1. Click **Create**.
1. Set the following options:
   * **Name**: Name that you want to user for this policy
   * **Cipher Suites**: `ECDHE-ECDSA-CHACHA20-POLY1305-SHA256:ECDHE-RSA-CHACHA20-POLY1305-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:TLSV1_3`
1. Click **Finished**.

### Create a cipher group
{: #cipher-group}

1. In the BIG-IP console, go to **Local Traffic > Ciphers > Groups**.
1. Click **Create**.
1. Set the following options:
   * **Name**: Name that you want to user for this group
   * Under **Group Details**, move the rule that you created to the **Allow the following** section
1. Click **Finished**.

## Create SSL profiles in BIG-IP
{: #ssl-profiles}
{: step}

In this step, you create a client and server SSL profile by using the client certificate or key and server certificate from the previous steps.

### Create a client SSL profile
{: #ssl-profiles-client}

1. In the BIG-IP console, go to **Local Traffic > Profiles > SSL > Client**.
1. Click **Create**.
1. Set the following options:
   * **Name**: Name that you want to user for this profile
1. In the **Configuration** section, change **Basic** to **Advanced**, select **Custom** and set the following values:
   * **Certificate Key Chain**: Add both the key and certificate that you imported in the previous steps
   * **Ciphers**: select Cipher Group and the name of the group that you created in the previous step
   * **Options**: `Options List...`
   * **Options List**: The only enabled option should be `Don't insert empty fragments`
1. Click **Finished**.

### Create a server SSL profile
{: #ssl-profiles-server}

For each server certificate that you imported, create a server SSL profile by completing the following steps:

1. In the BIG-IP console, navigate to **Local Traffic > Profiles > SSL -> Server**.
1. Click **Create**.
1. In the **General Properties** section, specify:
   * **Name**: Name that you want to use for this profile
1. In the **Configuration** section, change **Basic** to **Advanced**, click the **Custom** checkbox and set the following values:
   * **Secure Renegotiation**: **Require Strict**
   * **Server Name**: FQDN of your custom application
   * **Ciphers**: select Cipher Group and the name of the group you created in the step above
   * **Options**: `Options List...`
   * **Options List**: Only enabled option should be `Don't insert empty fragments`
1. In the **Server Authentication** section, set the following options:
   * **Server Certificate**: **require...**
   * **Trusted Certificate Authorities**: Set to your intermediate server certificate
1. Click **Finished**.


## Create a BIG-IP virtual server
{: #create-virtual-server}
{: step}

In this step, you create a BIG-IP virtual server for traffic distribution with a backend pool.

### Create iRules
{: #vs-irule}

An iRule is a feature within the BIG-IP local traffic management (LTM) system that you can use to manage your network traffic. See [Getting started with iRules: Basic Concepts](https://devcentral.f5.com/s/articles/getting-started-with-irules-basic-concepts-20397?tab=series&page=1){: external} for more information about iRules.

1. If you have a backend that serves multiple HTTPS sites by using the TLS Server Name Indication feature, for more information see [Configure a virtual server to serve multiple HTTPS sites using the TLS Server Name Indication feature](https://support.f5.com/csp/article/K13452){: external}.
1. Host header rewrite is used if your public facing domain is different than your backend. Use the following example to rewrite the host header based on the FQDN of the incoming request:

```
when HTTP_REQUEST {
    set hostname [getfield [HTTP::host] ":" 1]
}
when HTTP_REQUEST_SEND {
    switch -glob [string tolower $hostname] {
    "mysiteA.public.domain" {
      clientside {
        HTTP::header replace host "mysiteA.backend.domain"
      }
    }
    "mysiteB.public.domain" {
      clientside {
        HTTP::header replace host "mysiteB.backend.domain"
      }
    }
    default {
      reject
    }
  }
}
```
{: codeblock}

### Create the backend pool
{: #vs-backend-pool}

1. In the BIG-IP console, go to **Local Traffic > Pools > Pool List**.
1. Click **Create**.
1. In the **Configuration** section, set the following fields:
   * **Name**: Name that you want to use for the pool
   * **Health Monitor**: Select an appropriate protocol to monitor the health of your members
1. In the **Resources** section, set the following fields:
   * **Load Balancing Method**: Select an appropriate method that you would like to use for balancing traffic
   * **New Members**: For each IP or FQDN, input the members of your backend server

### Create policies
{: #vs-policies}

If you have multiple backend pools to which you need to route traffic, you must create a policy to route traffic to the proper backend pool. For more information, see [Introducing Local Traffic Policies](https://techdocs.f5.com/kb/en-us/products/big-ip_ltm/manuals/product/local-traffic-policies-getting-started-12-1-0/1.html){: external}.

1. Go to **Local Traffic > Policies > Policy List**.
2. Click **Create**.
3. Set the following under **General Properties**:
   * **Policy Name**: Name that you want to use for the policy
   * **Strategy**: Set the strategy that you would like to use for rule matching
   * **Type**: **Traffic Policy**
4. Click **Create**.
5. Under **Rules**, click **Create**.
6. Under **Name**, enter the name that you want to specify for this rule.
7. Click the **+** symbol and enter the conditions that must be matched.
8. Click the **+** symbol and enter what should be done when the condition is met. If you want to forward to a backend pool, select **Forward Traffic**.
9. If you need to create multiple rules, click **Create** again and repeat the previous steps.
10. After all rules are created, click **Save Draft**.
11. Under **Local Traffic > Policies > Policy List**, select your policy and click **Publish**.

### Create the virtual server
{: #vs-create}

1. In the console, go to **Local Traffic > Virtual Servers > Virtual Server List**.
1. Click **Create** 
1. In the **General Properties** section, select or input the following fields:
   * **Name**: Name that you want to use for the virtual server
   * **Type**: **Standard**
   * **Destination Address/Mask**: Internal IP of the External interface
   * **Service Port**: `443`
   * **State**: **Enabled**
1. Under the **Configuration** section, select or input the following fields:
   * **Protocol**: **TCP**
   * **Protocol Profile (Client)**: **tcp**
   * **Protocol Profile (Server)**: **(Use Client Profile)**
   * **HTTP Profile (Client)**: **http**
   * **HTTP Profile (Server)**: **(Use Client Profile)**
   * **SSL Profile (Client)**: Name of the client SSL profile you created previously
   * **SSL Profile (Server)**: If you have one server profile, select the name that you created in a previous step, otherwise see the iRules sections
   * **Source Address Translation**: **Auto Map**
1. Click **Finished**.
1. Click the name of your newly created virtual server from the **Virtual Server** List.
1. Click the **Resources** tab.
1. Under the **Load Balancing** section select the following options:
   * **Default Pool**: Name of backend pool that you created in a previous step
1. Under the **iRules** section:
   * Click **Manage**.
   * Move the iRules that you want to enable to the **Enabled** section.
1. Under the **Policies** section:
   * Click **Manage**.
   * Move the policy that you want to enable to the **Enabled** section.

## Configure WAF on the virtual server
{: #waf-configure}
{: step}

### Enable license
{: #waf-license}

1. Under **System > License**, click the **Module Allocation** tab.
1. If **Application Security (ASM)** is not enabled, enable it with the **Nominal** selection.
1. BIG-IP restarts.
1. When the restart completes, log back into BIG-IP.

### Set up WAF
{: #waf-configure}

1. Go to **Security > Guided Configuration**.
1. Click **Web Application Protocol** and then **Web Application Comprehensive Protection**.
1. Click **Next**.
1. Under **Security Layers*** do the following:
   1. Enter a configuration name.  
   1. Enable any security layers that you would like. The security layers of *Security Policy* and *Behavioral Analysis DoS* should at least be enabled.
   1. Click **Save** and **Next**.
1. Under **Web Application Security Policy Properties**:
   1. Set the following:
     * **Select Enforcement Mode**: **Blocking**
     * **Select type of policy to protect application**: **Generic**
     * **Application Language**: Select the language encoding for your web application
   1. Click **Save** and **Next**.
1. Under **Bot Defense Properties**
   1. Set the following:
     * **Select Enforcement Mode**: **Blocking**
     * **Profile Template**: **Balanced**
   1. Click **Save** and **Next**.
1. Under **DoS Profile Properties**
   1. Set the following under **Operation Mode**:
     * **Operation Mode**: **Blocking**
     * Verify that **Bad actors behavior detection** and **Request signature detection** are selected
   1. Set the following under **Mitigation Mode**:
     * Mitigation Mode: **Standard**
   1. Click **Save** and **Next**.
1. Under **Virtual Server Properties**
   1. Check **Assign Policy to Virtual Server(s)**. 
   1. Under **Virtual Server** select **Use Existing**.
   1. Under **Assign Virtual Servers**, move the virtual server that you created previously over from **Available** to **Selected**.
   1. Click **Save** and **Next**.
1. Click **Deploy**.
1. Verify that the configuration is under **Deployed* status.
