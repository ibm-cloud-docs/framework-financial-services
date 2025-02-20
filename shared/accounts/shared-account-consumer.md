---

copyright:
  years: 2020, 2025
lastupdated: "2025-02-20"

keywords:

subcollection: framework-financial-services

---

{{site.data.keyword.attribute-definition-list}}

# Consumer accounts for application provider workloads
{: #shared-account-consumer}

You must have a system for authentication and authorization of the consumer organization's users when they connect to your application workloads through a web app or API. As the best practice for [enabling a zero trust environment](/docs/framework-financial-services?topic=framework-financial-services-best-practices#best-practices-zero-trust) says, you need to provide proper role-based access control (RBAC) for these users.
{: shortdesc}

You can use [{{site.data.keyword.appid_short_notm}}](/docs/appid?topic=appid-about) to secure your apps, back-end resources, and APIs by using standards-based authentication. {{site.data.keyword.appid_short_notm}} makes it easy to add an authentication step to your applications with a few lines of code. You can add email or username, social, or enterprise sign in to your apps with APIs, SDKs, prebuilt UIs, or your own branded UIs.

You can choose between several identity providers. For more information, see [Managing authentication](/docs/appid?topic=appid-managing-idp). The two most likely options that you would use for {{site.data.keyword.cloud_notm}} for Financial Services are:

* [Security Assertion Markup Language (SAML)](/docs/appid?topic=appid-enterprise#enterprise) - You can create a single sign-on experience for your users by integrating with the consumers identity provider.
* [Cloud Directory](/docs/appid?topic=appid-cloud-directory) - You can maintain your own user registry in the cloud. When a user signs up for your app, they are added to your directory of users. This option gives your users more freedom to manage their own account within your app.

## {{site.data.keyword.appid_short_notm}} integrations with {{site.data.keyword.cloud_notm}} services
{: #app-id-integrations}


You can use {{site.data.keyword.appid_short_notm}} with other {{site.data.keyword.cloud_notm}} offerings. For example, if you're using {{site.data.keyword.openshiftshort}}, you can configure Ingress in your cluster to secure your apps at the cluster level. For more details, see [Setting up Ingress](/docs/openshift?topic=openshift-ingress-roks4) and [{{site.data.keyword.appid_short_notm}} authentication Ingress annotation](/docs/containers?topic=containers-comm-ingress-annotations#app-id-authenticationh) to get started.

## Related controls in {{site.data.keyword.framework-fs_notm}}
{: #related-controls}

The following {{site.data.keyword.framework-fs_notm}} controls are most related to this guidance. However, in addition to following the guidance here, do your own due diligence to ensure you have met the requirements.

| Family | Control |
|----------------------------------------|------------------------------------------|
| Access Control (AC)  | [AC-2 Account Management](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-ac-2) \n [AC-5 Separation of Duties](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-ac-5) \n [AC-6 Least Privilege](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-ac-6)  |
| Identification and Authentication (IA) | [IA-5 Authenticator Management](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-ia-5) \n [IA-5 (1) Authenticator Management &#124; Password-Based Authentication](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-ia-5.1) |
{: caption="Related controls in {{site.data.keyword.framework-fs_notm}} [FSv2.0]" caption-side="top"}
{: #related-controls-fsv2.0}
{: tab-title="FSv2.0"}
{: tab-group="RelatedControls-1"}
{: class="simple-tab-table"}


| Family | Control |
|----------------------------------------|------------------------------------------|
| Access Control (AC)  | [AC-2 Account Management](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-ac-2) \n [AC-5 Separation of Duties](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-ac-5) \n [AC-6 Least Privilege](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-ac-6)  |
| Identification and Authentication (IA) | [IA-5 Authenticator Management](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-ia-5) \n [IA-5 (1) Authenticator Management &#124; Password-Based Authentication](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-ia-5.1) |
{: caption="Related controls in {{site.data.keyword.framework-fs_notm}} [FSv1.1]" caption-side="top"}
{: #related-controls-fsv1.1}
{: tab-title="FSv1.1"}
{: tab-group="RelatedControls-1"}
{: class="simple-tab-table"}


## Next steps
{: #next-steps}

* [Tagging {{site.data.keyword.cloud_notm}} resources and managing access through tags](/docs/framework-financial-services?topic=framework-financial-services-shared-tagging-resources)
