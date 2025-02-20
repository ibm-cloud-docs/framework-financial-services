---

copyright:
  years: 2020, 2025
lastupdated: "2025-02-20"

keywords:

subcollection: framework-financial-services

---

{{site.data.keyword.attribute-definition-list}}

# Handling and securing secrets
{: #shared-secrets}

A secret is any piece of data that is sensitive within the context of an application or service. Secrets must be securely protected through their entire lifecycle.
{: shortdesc}

Secrets include all of the following but are not limited to:

* Passwords of any type (database logins, OS accounts, functional IDs, and so on)
* API keys
* Long-lived authentication tokens (OAuth2, GitHub, IAM, and so on)
* SSH keys
* Encryption keys
* Other private keys (PKI/TLS certificates, HMAC keys, signing keys, and so on)

Application providers should ensure:

* Secrets are generated and stored in the environment (for example, dev, test, and production) where your service is deployed.
* Secrets never leave their environments (for example, dev, test, and production) and should be secured by using access control measures. Service design should minimize the number of machines and people with access to secrets by using both authorization and network restrictions based on the principle of least privilege.
* Secrets are rotated in according with the requirements of the {{site.data.keyword.framework-fs_notm}} with minimal or no downtime.
* Secrets are never stored in source code, configuration files, or documentation.

The following table lists the different solutions that you can use to protect your application secrets.



| Scenario | What to use |
| --- | --- |
| You need to create, lease, and manage API keys, credentials, database configurations, and other secrets for your services and applications. | Use [{{site.data.keyword.secrets-manager_full_notm}}](/docs/secrets-manager?topic=secrets-manager-getting-started).  |
| You need to generate, renew, and manage TLS/SSL certificates for your deployments. | Use [{{site.data.keyword.secrets-manager_full_notm}}](/docs/secrets-manager?topic=secrets-manager-getting-started). |
| You need to create and manage encryption keys. | Use [{{site.data.keyword.hscrypto}}](/docs/hs-crypto?topic=hs-crypto-overview) to manage encryption keys in a single-tenant service with dedicated hardware. |
| You need secrets in your {{site.data.keyword.openshiftshort}} environment for microservices to connect to system resources. | Use [Kubernetes secrets](https://kubernetes.io/docs/concepts/configuration/secret/){: external} encrypted using [{{site.data.keyword.hscrypto}}](/docs/openshift?topic=openshift-encryption#keyprotect) as your Key Management Service (KMS) provider. |
{: caption="Secrets management and data protection scenarios" caption-side="top"}

[^tabletext-1]: {{site.data.content.fs-validated-disclaimer}}

[^tabletext-2]: {{site.data.content.fs-validated-disclaimer}}



## Related controls in {{site.data.keyword.framework-fs_notm}}
{: #related-controls}

{{site.data.content.related-controls-disclaimer}}

| Family              | Control                                           |
|---------------------|---------------------------------------------------|
| Access Control (AC) | [AC-2 Account Management](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-ac-2)  |
| Identification and Authentication (IA)  | [IA-2 User Identification and Authentication](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-ia-2) \n [IA-5 Authenticator Management](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-ia-5)  |
{: caption="Related controls in {{site.data.keyword.framework-fs_notm}} [FSv2.0]" caption-side="top"}
{: #related-controls-fsv2.0}
{: tab-title="FSv2.0"}
{: tab-group="RelatedControls-1"}
{: class="simple-tab-table"}


| Family              | Control                                           |
|---------------------|---------------------------------------------------|
| Access Control (AC) | [AC-2 Account Management](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-ac-2)  |
| Identification and Authentication (IA)  | [IA-2 User Identification and Authentication](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-ia-2) \n [IA-5 Authenticator Management](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-ia-5)  |
{: caption="Related controls in {{site.data.keyword.framework-fs_notm}} [FSv1.1]" caption-side="top"}
{: #related-controls-fsv1.1}
{: tab-title="FSv1.1"}
{: tab-group="RelatedControls-1"}
{: class="simple-tab-table"}


## Next steps
{: #next-steps}

* [Consumer accounts for application provider workloads](/docs/framework-financial-services?topic=framework-financial-services-shared-account-consumer)
