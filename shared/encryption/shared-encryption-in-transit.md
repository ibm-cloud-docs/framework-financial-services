---

copyright:
  years: 2020, 2024
lastupdated: "2024-11-19"

keywords: 

subcollection: framework-financial-services

---

{{site.data.keyword.attribute-definition-list}}

# Data encryption in transit
{: #shared-encryption-in-transit}

Data in transit must always be encrypted. Cryptographic controls must be in place in all regions and availability zones to protect the confidentiality and integrity of data. Data must be protected by at least two levels of encryption such as transport and field level or envelope level.
{: shortdesc}

As an application provider, you must:

* Ensure that all traffic, tunneling, and other protection of data in transit is encrypted by using TLS 1.2 at a minimum. You must disable the SSL/TLS 1.0/TLS 1.1 protocols on all software that is deployed in your VPCs. See the following [TLS requirements](/docs/framework-financial-services?topic=framework-financial-services-shared-encryption-in-transit#shared-encryption-in-transit-tls-requirements) section for more details.
* Use {{site.data.keyword.hscrypto}} for TLS offload for all data that is requested from outside of {{site.data.keyword.cloud_notm}} (inbound traffic to {{site.data.keyword.cloud_notm}}) and protected by a certificate that is signed by a public certificate authority. In this scenario, you should configure any web servers to use TLS offload to set up the session so that the private key never leaves {{site.data.keyword.hscrypto}}. See [Use {{site.data.keyword.cloud_notm}} {{site.data.keyword.hscrypto}} to offload NGINX TLS](https://developer.ibm.com/tutorials/use-hyper-protect-crypto-services-to-offload-nginx-tls/){: external} for an example.
* Enforce encryption that uses directives like HTTP Strict Transport Security (HSTS).
* Disable caching for responses that contain sensitive data.
* Never send confidential information with URL parameters.
* Use HTTPS for all connections from the web. HTTP connections must be refused or redirected to HTTPS.

 The requirement to encrypt data in transit also applies to traffic within your VPCs. This might be traffic from one virtual server instance to another, or from one {{site.data.keyword.openshiftshort}} container to another. If using containers, you might consider [{{site.data.keyword.openshiftshort}} Service Mesh](/docs/solution-tutorials?topic=solution-tutorials-openshift-service-mesh) which can [enable mTLS between containers](/docs/solution-tutorials?topic=solution-tutorials-openshift-service-mesh#openshift-service-mesh-secure_services) without code changes.
{: important}

## TLS requirements
{: #shared-encryption-in-transit-tls-requirements}

To be effective, TLS must be configured correctly otherwise the data in transit is vulnerable to modification or eavesdropping. The following steps are required to deploy a secure TLS connection:

* Securely generate and store a private key.
* Obtain a certificate associated with the private key, signed by the appropriate trusted certificate authority (CA).
* Configure the TLS server to use approved TLS versions (1.2 or 1.3) and approved cipher suites.
* Configure the TLS client to check certificate validity and use approved TLS versions only.
* Deploy certificate and private key to TLS server.
* Enable the TLS server.
* Replace the certificate before it expires (typically certificates expire after 1 year).

### TLS protocol requirements
{: #shared-encryption-in-transit-tls-protocol-requirements}

1. Only TLS versions 1.2 or 1.3 are permitted. All older versions of TLS and all version of SSL must not be used.
2. Prohibited versions of SSL/TLS must be disabled on the server to prevent downgrading to a vulnerable version of the protocol.
3. Only cipher suites that are listed under Acceptable cipher suites can be enabled. All others must be disabled.

### Key size requirements
{: #shared-encryption-in-transit-tls-key-size-requirements}

Key size requirements are as follows:

* RSA – minimum key size is 2048, and 4096 is recommended.
* ECDSA – minimum key size is 224.
* ECDHE – minimum key size is 224.

### Acceptable cipher suites
{: #shared-encryption-in-transit-tls-cipher-suites}

For TLS 1.2, the acceptable cipher suites are as follows:

* TLS_ECDHE_ECDSA_WITH_CHACHA20_POLY1305_SHA256
* TLS_ECDHE_RSA_WITH_CHACHA20_POLY1305_SHA256
* TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384
* TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384
* TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256
* TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256

For TLS 1.2, the acceptable cipher suites are as follows:

* TLS_AES_128_GCM_SHA256
* TLS_AES_256_GCM_SHA384
* TLS_CHACHA20_POLY1305_SHA256

### Certificate requirements
{: #shared-encryption-in-transit-tls-cert-requirements}

1. Certificate-private keys must be unique to that certificate. Private keys must not be reused for any other purpose
2. Certificate-private keys must not be reused when a certificate is renewed.
3. Certificates must specify the FQDN for the certificate in the **Subject Alternative Name** field; single-domain certificates should have that domain repeated in the SAN field.
4. TLS certificates must be used only as specified by the Enhanced Key Usage attribute within the certificate. The following key uses are acceptable for TLS certificates: TLS Web Server Authentication, TLS Web Client Authentication
5. External facing certificates that are issued by an external CA must have a maximum lifetime of 1 year.
6. Private keys of external facing certificates must be stored in HSM
7. Certificates should uniquely identify an endpoint and any subject alternative names, rather than using wildcards. Wildcard certificates should be used only when TLS endpoints are programmatically generated and must be limited to the smallest usable subdomain.

## Related controls in {{site.data.keyword.framework-fs_notm}}
{: #shared-encryption-in-transit-related-controls}

{{site.data.content.related-controls-disclaimer}}

| Family              | Control                                           |
|---------------------|---------------------------------------------------|
| System and Communications Protection (SC) | [SC-8 Transmission Confidentiality and Integrity](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-8) \n [SC-8 (1) Transmission Confidentiality and Integrity &#124; Cryptographic Protection](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-8.1) \n [SC-12 Cryptographic Key Establishment and Management](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-12) \n [SC-12 (2) Cryptographic Key Establishment and Management &#124; Symmetric Keys](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-12.2) \n [SC-12 (3) Cryptographic Key Establishment and Management &#124; Asymmetric Keys](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-12.3) \n [SC-13 Cryptographic Protection](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-13)  |
{: caption="Related controls in {{site.data.keyword.framework-fs_notm}}" caption-side="top"}

In addition, you must follow the guidance found in Appendix A: Cryptographic Requirements within the Control Implementation Overview Template for Application Providers Using {{site.data.keyword.vpc_full}}.

## Next steps
{: #shared-encryption-in-transit-next-steps}

* [Business continuity and disaster recovery](/docs/framework-financial-services?topic=framework-financial-services-shared-bcdr)
