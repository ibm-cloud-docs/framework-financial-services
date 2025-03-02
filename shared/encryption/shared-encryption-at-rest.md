---

copyright:
  years: 2020, 2025
lastupdated: "2025-03-02"

keywords:

subcollection: framework-financial-services

---

{{site.data.keyword.attribute-definition-list}}

# Data encryption at rest
{: #shared-encryption-at-rest}

Protecting data against unauthorized disclosure, modification or destruction throughout the data lifecycle is of paramount importance in the {{site.data.keyword.cloud_notm}} for Financial Services. Cryptographic controls must be in place in all regions and availability zones to protect the confidentiality and integrity of data. Effective key management enables you to keep control of your own master and root encryption keys, that protect the keys used for data at rest encryption.
{: shortdesc}

## Data encryption at rest in {{site.data.keyword.cloud_notm}}
{: #shared-encryption-at-rest-ibm-cloud}

Within {{site.data.keyword.cloud_notm}}, you must encrypt data at rest using encryption keys that you manage. For systems having a high security categorization you must use [{{site.data.keyword.hscrypto}}](/docs/hs-crypto?topic=hs-crypto-overview) to manage encryption keys. {{site.data.keyword.hscrypto}} is a single tenant key management service and dedicated hardware security module (HSM) partition. {{site.data.keyword.hscrypto}} is the only service in the cloud industry that's built on FIPS 140-2 Level 4-certified hardware.

With {{site.data.keyword.hscrypto}} you can take ownership of the cloud HSM to fully manage your encryption keys and to perform cryptographic operations by using the [Keep Your Own Key (KYOK)](/docs/hs-crypto?topic=hs-crypto-understand-concepts#kyok-concept) concept. This means you have full control and authority over encryption keys. No one except you (not even {{site.data.keyword.IBM_notm}}) has access to your encryption keys.

Some organizations require the use of a dedicated HSM where the device can be initialized by their cryptography specialists. We offer [{{site.data.keyword.hscrypto}} with Bring Your Own HSM](/docs/hs-crypto?topic=hs-crypto-introduce-bring-your-own-hsm) to enable those organizations to manage their HSM over the network at a remote location. This solution does uses the {{site.data.keyword.hscrypto}} key manager that has been Financial Services Validated but as IBM has no control over the Bring Your Own HSM, this solution as a whole isn't Financial Services Validated.

Where you have systems not requiring a [high security categorization](/docs/framework-financial-services?topic=framework-financial-services-system-security-categorization), you have the option of using {{site.data.keyword.keymanagementservicefull}} for your key management. {{site.data.keyword.keymanagementserviceshort}} is a multi-tenant cloud service using the [Bring Your Own Key (BYOK)](/docs/key-protect?topic=key-protect-manage-secrets-ibm-cloud#key-features) concept. Many capabilities are common between {{site.data.keyword.hscrypto}} and {{site.data.keyword.keymanagementserviceshort}} but the underlying security implementation is different as shown in Table 1.

| | {{site.data.keyword.hscrypto}} | {{site.data.keyword.hscrypto}} with Bring Your Own HSM [^kms-byohsm] | {{site.data.keyword.keymanagementserviceshort}} |
|----|----|------|----|
| Cloud REST API | X | X | X |
| Multi-Tenant Key Manager | | | X |
| Provider-managed HSM | | | X |
| Single-Tenant Key Manager | X | X | |
| Key Manager in Secure Enclave | X | X | |
| FIPS 140-2 Level 4 HSM | X | | |
| Client-managed dedicated HSM partition | X | | |
| HSM Smartcard key generation ceremony | X | | |
| EP11 (PKCS#11) API | X | | |
| MZR Availability | [Selected](https://cloud.ibm.com/docs/hs-crypto?topic=hs-crypto-regions) | [Selected](https://cloud.ibm.com/docs/hs-crypto?topic=hs-crypto-regions) | All |
| Support for {{site.data.keyword.satelliteshort}} location | | | X |
| UKO Key Generation [^kms-uko] | X | | |
{: caption="Key Features for {{site.data.keyword.cloud_notm}} Key Management}

Note 1: Capability depends on functionality of Bring Your Own HSM.

[^kms-byohsm]: HSM capabilities depend on the technology provided for the Bring Your Own HSM component.

[^kms-uko]: UKO only supports {{site.data.keyword.hscrypto}} using an {{site.data.keyword.cloud_notm}} HSM.

{{site.data.keyword.hscrypto}} and {{site.data.keyword.keymanagementserviceshort}} are both Financial Services Validated with the same level of security assurance. The strength of the security in {{site.data.keyword.hscrypto}}, as a single-tenant service with smartcards for the master key ceremony, is higher than {{site.data.keyword.keymanagementserviceshort}} as a multi-tenant service.

Whether you use {{site.data.keyword.hscrypto}} or {{site.data.keyword.keymanagementserviceshort}}, you must ensure that:

* Keys are created for a single purpose, and dedicated encryption keys are unique per consumer.
* Keys have a lifecycle that is defined and are rotated periodically based on {{site.data.keyword.framework-fs_notm}} controls.
* Recovery functions, if used, are accessible only by authorized personnel.



### Setting up {{site.data.keyword.hscrypto}}
{: #setup-hpcs}

For the provisioning and configuration of an instance of {{site.data.keyword.hscrypto}} see [Getting started with {{site.data.keyword.hscrypto}}](/docs/hs-crypto?topic=hs-crypto-get-started) for specific instructions.

For the highest level of security, you should use smart cards when initializing {{site.data.keyword.hscrypto}}. See [Initializing service instances by using smart cards and the Management Utilities](/docs/hs-crypto?topic=hs-crypto-initialize-hsm-management-utilities) for more details.
{: tip}

For more background information and considerations, see:

* [Components and concepts](/docs/hs-crypto?topic=hs-crypto-understand-concepts)
* [Bringing your encryption keys to the cloud](/docs/hs-crypto?topic=hs-crypto-importing-keys)
* [Master key rotation](/docs/hs-crypto?topic=hs-crypto-master-key-rotation-intro)
* [Root key rotation](/docs/hs-crypto?topic=hs-crypto-root-key-rotation-intro)

Continue with completing integration of {{site.data.keyword.hscrypto}} with the {{site.data.keyword.cloud_notm}} services that are part of your deployment. {{site.data.keyword.block_storage_is_short}} and {{site.data.keyword.cos_short}} are common services that need data encrypted at rest, but there is an extended set of services that integrate. The {{site.data.keyword.hscrypto}} documentation includes a [catalog](/docs/hs-crypto?topic=hs-crypto-integrate-services) of {{site.data.keyword.cloud_notm}} services to integrate.

The master key within the {{site.data.keyword.hscrypto}} HSM and the root keys contained within {{site.data.keyword.hscrypto}} key manager need rotating with a new key at the frequency determined by your policy. If you don't have a policy, start with a 12 month rotation period. Consult the [master key rotation process](/docs/hs-crypto?topic=hs-crypto-master-key-rotation-intro) and  [root key rotation process](/docs/hs-crypto?topic=hs-crypto-root-key-rotation-intro) for specific instructions.

For more background information and considerations, see:

* [Components and concepts](/docs/hs-crypto?topic=hs-crypto-understand-concepts)
* [Bringing your encryption keys to the cloud](/docs/hs-crypto?topic=hs-crypto-importing-keys)

### Setting up {{site.data.keyword.hscrypto}} with Bring Your Own HSM
{: #setup-hpcs-byohsm}

As an alternative to using the in-built FIPS 140-2 Level 4 Cloud HSM for {{site.data.keyword.hscrypto}}, there is an option to Bring Your Own Hardware Security Module (BYOHSM).  Details on the integration and the configuration is contained in the [Introducing Bring Your Own HSM](/docs/hs-crypto?topic=hs-crypto-introduce-bring-your-own-hsm) documentation.

The Bring Your Own HSM option provides you with the the benefit of being able to physically initialize your own HSM device. We understands for some organizations this is a mandatory requirement and this solution will meet that requirement.

However to operational characteristics of {{site.data.keyword.hscrypto}} changes. Here are a few things to keep in mind when selecting to use the Bring Your Own HSM option:

1. The Bring Your Own HSM may be FIPS 140-2 Level 3, which is a lower security level than the HSM used as default by {{site.data.keyword.hscrypto}}.
2. With the standard {{site.data.keyword.hscrypto}} solution there is negligible latency between the {{site.data.keyword.hscrypto}} key manager and Cloud HSM as they're within the same system. Ensure you assess any impact from increased latency between the HPCS key manager and your Bring Your Own HSM.
3. With the additional network hops between the {{site.data.keyword.hscrypto}} instance and the Bring Your Own HSM there may be a reduction in workload resilience. Review and understand the impact this change may have to your workload resiliency.
4. Ensure you identify an effective cold start sequence for HPCS connecting to your Bring Your Own HSM and the services that depend on {{site.data.keyword.hscrypto}} key management. For example, if the the connection of HPCS to the Bring Your Own HSM is through a firewall appliance using block storage with a key from {{site.data.keyword.hscrypto}}, you may get into a situation where the Bring Your Own HSM can't communicate from the firewall not being able to decrypt the block storage. This is otherwise known as the "keys locked in the car" scenario.

For more background information and considerations, see:

* [Introducing Bring Your Own HSM](/docs/hs-crypto?topic=hs-crypto-introduce-bring-your-own-hsm)
* [Setting up Bring Your Own HSM](/docs/hs-crypto?topic=hs-crypto-deploy-hsm-for-byohsm)
* [Managing your keys with Bring Your Own HSM in {{site.data.keyword.cloud_notm}} {{site.data.keyword.hscrypto}}](/docs/hs-crypto?topic=hs-crypto-tutorial-byohsm)

### Setting up {{site.data.keyword.keymanagementserviceshort}}
{: #setup-keyprotect}

For the provisioning and configuration an instance of {{site.data.keyword.keymanagementserviceshort}}, consult the [Getting started tutorial](/docs/key-protect?topic=key-protect-getting-started-tutorial) for specific instructions.

Continue with completing integration of {{site.data.keyword.keymanagementserviceshort}} with the {{site.data.keyword.cloud_notm}} services that are part of your deployment. {{site.data.keyword.block_storage_is_short}} and {{site.data.keyword.cos_short}} are common services that need data encrypted at rest, but there is an extended set of services that integrate. The {{site.data.keyword.keymanagementserviceshort}} documentation includes a [catalog](/docs/key-protect?topic=key-protect-integrate-services) of {{site.data.keyword.cloud_notm}} services to integrate.

The root keys contained within {{site.data.keyword.keymanagementserviceshort}} need rotating with a new key at the frequency determined by your policy. If you don't have a policy, start with a 12 month rotation period. Consult the [root key rotation process](/docs/key-protect?topic=key-protect-key-rotation) for specific instructions.

For more background information and considerations, see:

* [About {{site.data.keyword.keymanagementserviceshort}}](/docs/key-protect?topic=key-protect-about)
* [Bringing your encryption keys to the cloud](/docs/key-protect?topic=key-protect-importing-keys)

### Data encryption at rest in {{site.data.keyword.satelliteshort}} location
{: #shared-encryption-at-rest-satellite-location}

In an {{site.data.keyword.cloud_notm}} {{site.data.keyword.satelliteshort}} location, the cloud services must use {{site.data.keyword.keymanagementserviceshort}} on {{site.data.keyword.satelliteshort}} to connect to a user owned and managed hardware security module (HSM) for the encryption of data at rest. Consult [About Key Protect on Satellite](/docs/key-protect?topic=key-protect-satellite-about) for specific information on the dedicated service.

{{site.data.content.encryption-at-rest-in-satellite-location-important}}





## Related controls in {{site.data.keyword.framework-fs_notm}}
{: #related-controls}

{{site.data.content.related-controls-disclaimer}}

| Family              | Control                                           |
|---------------------|---------------------------------------------------|
| System and Communications Protection (SC) | [SC-12 Cryptographic Key Establishment and Management](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-12) \n [SC-12 (2) Cryptographic Key Establishment and Management &#124; Symmetric Keys](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-12.2) \n [SC-12 (3) Cryptographic Key Establishment and Management &#124; Asymmetric Keys](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-12.3) \n [SC-13 Cryptographic Protection](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-13) \n [SC-28 Protection of Information at Rest](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-28) \n [SC-28 (1) Protection of Information at Rest &#124; Cryptographic Protection](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-28.1)  |
{: caption="Related controls in {{site.data.keyword.framework-fs_notm}} [FSv2.0]" caption-side="top"}
{: #related-controls-fsv2.0}
{: tab-title="FSv2.0"}
{: tab-group="RelatedControls-1"}
{: class="simple-tab-table"}


| Family              | Control                                           |
|---------------------|---------------------------------------------------|
| System and Communications Protection (SC) | [SC-12 Cryptographic Key Establishment and Management](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-sc-12) \n [SC-12 (2) Cryptographic Key Establishment and Management &#124; Symmetric Keys](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-sc-12.2) \n [SC-12 (3) Cryptographic Key Establishment and Management &#124; Asymmetric Keys](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-sc-12.3) \n [SC-13 Cryptographic Protection](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-sc-13) \n [SC-28 Protection of Information At Rest](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-sc-28) \n [SC-28 (1) Protection of Information at Rest &#124; Cryptographic Protection](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-sc-28.1)  |
{: caption="Related controls in {{site.data.keyword.framework-fs_notm}} [FSv1.1]" caption-side="top"}
{: #related-controls-fsv1.1}
{: tab-title="FSv1.1"}
{: tab-group="RelatedControls-1"}
{: class="simple-tab-table"}


In addition, you must follow the guidance found in Appendix A: Cryptographic Requirements within the "Control Implementation Overview Template for Application Providers Using {{site.data.keyword.vpc_full}}".

## Next steps
{: #next-steps}

* [Data encryption in transit](/docs/framework-financial-services?topic=framework-financial-services-shared-encryption-in-transit)
