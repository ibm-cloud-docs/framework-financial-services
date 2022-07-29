---

copyright:
  years: 2020, 2022
lastupdated: "2022-07-29"

keywords: 

subcollection: framework-financial-services

---

{{site.data.keyword.attribute-definition-list}}

# Compliance monitoring
{: #shared-monitoring-compliance}



You are required to continuously monitor for possible security flaws and changes in baseline configurations for which you should take corrective action. {{site.data.content.service-description-scc}}
{: shortdesc}

Even though {{site.data.keyword.compliance_short}} is not yet Financial Services Validated, it's on the road map and we recommend its use. 
{: note}

## Using {{site.data.keyword.compliance_short}}
{: #using-security-compliance-center}

{{site.data.keyword.compliance_short}} provides a number of [pre-defined profiles](/docs/security-compliance?topic=security-compliance-predefined-profiles). Each profile is a collection of [controls](#x2018434){: term}, and each control has one or more [goals](#x2117978){: term}. Goals are pre-defined automated tests used to evaluate your posture against a control.

Running a scan against a specific profile does not ensure regulatory compliance. The scan is intended to provide a point in time statement of your current posture for a specific group of resources.
{: important}

The {{site.data.keyword.cloud_notm}} for Financial Services profile provides a tailored set of goals that are mapped to the [{{site.data.keyword.framework-fs_notm}} control requirements](/docs/framework-financial-services?topic=framework-financial-services-about#framework-control-requirements). This profile should always be used when leveraging the [VPC reference architecture](/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-about).

In addition, if you are using {{site.data.keyword.openshiftshort}} (whether in the VPC reference architecture or the [{{site.data.keyword.satelliteshort}} reference architecture](/docs/framework-financial-services?topic=framework-financial-services-satellite-architecture-about)), then you should leverage the [{{site.data.keyword.openshiftshort}} Compliance Operator (OSCO)](https://github.com/openshift/compliance-operator){: external} via the [OSCO integration](/docs/security-compliance?topic=security-compliance-setup-osco) with SCC.

To start evaluating your resources, see the tutorial series: [Monitor IBM Cloud resources](/docs/security-compliance?topic=security-compliance-ibm-credential).

## Compliance best practices for Financial Services Validated services
{: #related-resources}



The following table provides references to additional information for managing security and compliance for each service in the reference architecture.



| Category | VPC reference architecture | {{site.data.keyword.satelliteshort}} reference architecture | Optional for both |
|----------|-------------------|-------------------|-------------------|
| Core  | - [VPC infrastructure services](/docs/vpc?topic=vpc-manage-security-compliance) [^tabletext] | - [{{site.data.keyword.satelliteshort}}](/docs/satellite?topic=satellite-compliance#platform-compliance) | |
| Containers  | - [{{site.data.keyword.openshiftshort}}](/docs/openshift?topic=openshift-manage-security-compliance) \n - [{{site.data.keyword.registryshort}}](/docs/openshift?topic=openshift-manage-security-compliance) | - [{{site.data.keyword.openshiftshort}}](/docs/openshift?topic=openshift-manage-security-compliance) [^tabletext-satellite-enabled-openshift] \n - [{{site.data.keyword.registryshort}}](/docs/openshift?topic=openshift-manage-security-compliance) |  |
| Networking | - [VPC infrastructure services](/docs/vpc?topic=vpc-manage-security-compliance) \n - [{{site.data.keyword.dl_short}}](/docs/dl?topic=dl-manage-security-compliance) \n - [{{site.data.keyword.tg_short}}](/docs/transit-gateway?topic=transit-gateway-manage-security-compliance) |  |  |
| Storage  | - [{{site.data.keyword.block_storage_is_short}}](/docs/vpc?topic=vpc-manage-security-compliance) \n - [{{site.data.keyword.cos_short}}](/docs/cloud-object-storage?topic=cloud-object-storage-manage-security-compliance) | - [{{site.data.keyword.cos_short}}](/docs/cloud-object-storage?topic=cloud-object-storage-manage-security-compliance) |  |
| Security  | - [{{site.data.keyword.hscrypto}}](/docs/hs-crypto?topic=hs-crypto-manage-security-compliance) | - [{{site.data.keyword.hscrypto}}](/docs/hs-crypto?topic=hs-crypto-manage-security-compliance) | - [{{site.data.keyword.appid_short_notm}}](/docs/appid?topic=appid-manage-security-compliance) |
| Logging and monitoring   | - {{site.data.keyword.atracker_short}} \n - {{site.data.keyword.compliance_short}} [^tabletext-not-yet-validated]  \n - [Flow Logs for VPC](/docs/vpc?topic=vpc-manage-security-compliance)  | - {{site.data.keyword.atracker_short}} \n - {{site.data.keyword.compliance_short}} | |
| Integration  | |   | - {{site.data.keyword.messagehub}} |
| Databases  |  |  | - [{{site.data.keyword.ihsdbaas_mongodb_full}}](/docs/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-manage-security-compliance) \n - [{{site.data.keyword.ihsdbaas_postgresql_full}}](/docs/hyper-protect-dbaas-for-postgresql?topic=hyper-protect-dbaas-for-postgresql-manage-security-compliance) | |
{: caption="Table 1. Managing security and compliance for {{site.data.keyword.cloud_notm}} services in the reference architectures" caption-side="top"}

[^tabletext]: {{site.data.content.vpc-infrastructure-services-content}}

[^tabletext-satellite-enabled-openshift]: {{site.data.content.satellite-enabled-openshift}}

[^tabletext-not-yet-validated]: {{site.data.content.not-yet-validated}}

For more information, see:

* [Managing security and compliance in {{site.data.keyword.cloud_notm}}](/docs/overview?topic=overview-manage-security-compliance) - provides details on security and compliance within platform services.

## Related controls in {{site.data.keyword.framework-fs_notm}} 
{: #related-controls}

{{site.data.content.related-controls-disclaimer}}

| Family              | Control                                           |
|---------------------|---------------------------------------------------|
| Change Management (CM) | CM-2 (2) Baseline Configuration &#124; Automation Support for Accuracy and Currency \n CM-6 Configuration Settings \n CM-6 (1) Configuration Settings &#124; Automated Management, Application, and Verification   |
| Maintenance (MA) | MA-2 Controlled Maintenance   |
| System and Information Integrity (SI) | SI-2 (2) Flaw Remediation &#124; Central Management \n SI-6 Security Functionality Verification   |
| Security Assessment and Authorization (CA) | CA-7 Continuous Monitoring    |
{: caption="Table 2. Related controls in {{site.data.keyword.framework-fs_notm}}" caption-side="top"}

## Next steps
{: #next-steps}

* [Operational monitoring](/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-monitoring-operational)
