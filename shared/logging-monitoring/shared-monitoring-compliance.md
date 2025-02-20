---

copyright:
  years: 2020, 2025
lastupdated: "2025-02-20"

keywords:

subcollection: framework-financial-services

---

{{site.data.keyword.attribute-definition-list}}

# Compliance monitoring
{: #shared-monitoring-compliance}



You are required to continuously monitor for possible security flaws and changes in baseline configurations for which you should take corrective action. With [{{site.data.keyword.compliance_short}}](/docs/security-compliance?topic=security-compliance-getting-started) you can embed security checks into your every day workflows to help monitor for security and compliance. By monitoring for risks, you can identify security vulnerabilities and quickly work to mitigate the impact and fix the issue. By using {{site.data.keyword.compliance_short}} along with [external integrations](/security-compliance/integrations) (such as, OpenShift Compliance Operator (OSCO), Tanium, NeuVector, and so on), you can build a robust approach for monitoring for security and compliance issues.
{: shortdesc}

## Using {{site.data.keyword.compliance_short}}
{: #using-security-compliance-center}

{{site.data.keyword.compliance_short}} provides a number of [pre-defined profiles](/docs/security-compliance?topic=security-compliance-predefined-profiles). Each profile is a collection of [controls](#x2018434){: term}, and each control has one or more [goals](#x2117978){: term}. Goals are pre-defined automated tests used to evaluate your posture against a control.

Running a scan against a specific profile does not ensure regulatory compliance. The scan is intended to provide a point in time statement of your current posture for a specific group of resources.
{: important}

The {{site.data.keyword.cloud_notm}} for Financial Services profile provides a tailored set of goals that are mapped to the [{{site.data.keyword.framework-fs_notm}} control requirements](/docs/framework-financial-services?topic=framework-financial-services-about#framework-control-requirements). This profile should always be used when leveraging the [VPC reference architecture](/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-about).

In addition, if you are using {{site.data.keyword.openshiftshort}} (whether in the VPC reference architecture or the [{{site.data.keyword.satelliteshort}} reference architecture](/docs/framework-financial-services?topic=framework-financial-services-satellite-architecture-about)), then you should leverage the [{{site.data.keyword.openshiftshort}} Compliance Operator (OSCO) (OSCO)](https://github.com/openshift/compliance-operator){: external} via the [OSCO integration](/docs/security-compliance?topic=security-compliance-osco-scan) with SCC.

To start evaluating your resources, see the [Getting started with {{site.data.keyword.compliance_short}}](/docs/security-compliance?topic=security-compliance-getting-started)

## Compliance best practices for Financial Services Validated services
{: #related-resources}



The following table provides references to additional information for managing security and compliance for each service in the reference architecture.



| Category | VPC reference architecture | {{site.data.keyword.satelliteshort}} reference architecture | Optional for both |
|----------|-------------------|-------------------|-------------------|
| Core  | - [VPC infrastructure services](/docs/vpc?topic=vpc-responsibilities-vpc#security-compliance) [^tabletext] | - [{{site.data.keyword.satelliteshort}}](/docs/satellite?topic=satellite-compliance#platform-compliance) | |
| Containers  | - [{{site.data.keyword.openshiftshort}}](/docs/openshift?topic=openshift-responsibilities_iks#security-compliance) \n - [{{site.data.keyword.registryshort}}](/docs/Registry?topic=Registry-registry_responsibilities#security-compliance) | - [{{site.data.keyword.openshiftshort}}](/docs/openshift?topic=openshift-satellite-responsibilities#security-compliance-sat) [^tabletext-satellite-enabled-openshift] \n - [{{site.data.keyword.registryshort}}](/docs/Registry?topic=Registry-registry_responsibilities#security-compliance) |  |
| Networking | - [VPC infrastructure services](/docs/vpc?topic=vpc-responsibilities-vpc#security-compliance) \n - [{{site.data.keyword.dl_short}}](/docs/dl?topic=dl-dl-responsibilities#security-compliance) \n - [{{site.data.keyword.tg_short}}](/docs/transit-gateway?topic=transit-gateway-tg-responsibilities#security-rich-environment) |  |  |
| Storage  | - [{{site.data.keyword.block_storage_is_short}}](/docs/vpc?topic=vpc-responsibilities-vpc#security-compliance) \n - [{{site.data.keyword.cos_short}}](/docs/cloud-object-storage?topic=cloud-object-storage-responsibilities#responsibilities-security) | - [{{site.data.keyword.cos_short}}](/docs/cloud-object-storage?topic=cloud-object-storage-responsibilities#responsibilities-security) |  |
| Security  | - [{{site.data.keyword.hscrypto}}](/docs/hs-crypto?topic=hs-crypto-manage-security-compliance) | - [{{site.data.keyword.hscrypto}}](/docs/hs-crypto?topic=hs-crypto-manage-security-compliance) | - [{{site.data.keyword.appid_short_notm}}](/docs/appid?topic=appid-faq#faq-keycloak) |
| Logging and monitoring   | - {{site.data.keyword.atracker_short}} \n - {{site.data.keyword.compliance_short}} \n - [{{site.data.keyword.fl_full}}](/docs/vpc?topic=vpc-responsibilities-vpc#security-compliance)  | - {{site.data.keyword.atracker_short}} \n - {{site.data.keyword.compliance_short}} | |
| Integration  | |   | - {{site.data.keyword.messagehub}} |
{: caption="Managing security and compliance for {{site.data.keyword.cloud_notm}} services in the reference architectures" caption-side="top"}

[^tabletext]: {{site.data.content.vpc-infrastructure-services-content}}

[^tabletext-satellite-enabled-openshift]: {{site.data.content.satellite-enabled-openshift}}

For more information, see:

* [Understanding compliance in {{site.data.keyword.cloud_notm}}](/docs/overview?topic=overview-compliance) - provides details on compliance within platform services.

## Related controls in {{site.data.keyword.framework-fs_notm}}
{: #related-controls}

{{site.data.content.related-controls-disclaimer}}

| Family              | Control                                           |
|---------------------|---------------------------------------------------|
| Change Management (CM) | [CM-2 (2) Baseline Configuration &#124; Automation Support for Accuracy and Currency](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-cm-2.2) \n [CM-6 Configuration Settings](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-cm-6) \n [CM-6 (1) Configuration Settings &#124; Automated Management, Application, and Verification](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-cm-6.1)   |
| Maintenance (MA) | [MA-2 Controlled Maintenance](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-ma-2)   |
| System and Information Integrity (SI) | [SI-2 (2) Flaw Remediation &#124; Central Management](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-si-2.2) \n [SI-6 Security Functionality Verification](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-si-6)   |
| Security Assessment and Authorization (CA) | [CA-7 Continuous Monitoring](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-ca-7)    |
{: caption="Related controls in {{site.data.keyword.framework-fs_notm}} [FSv2.0]" caption-side="top"}
{: #related-controls-fsv2.0}
{: tab-title="FSv2.0"}
{: tab-group="RelatedControls-1"}
{: class="simple-tab-table"}


| Family              | Control                                           |
|---------------------|---------------------------------------------------|
| Change Management (CM) | [CM-2 (2) Baseline Configuration &#124; Automation Support for Accuracy and Currency](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-cm-2.2) \n [CM-6 Configuration Settings](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-cm-6) \n [CM-6 (1) Configuration Settings &#124; Automated Management, Application, and Verification](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-cm-6.1)   |
| Maintenance (MA) | [MA-2 Controlled Maintenance](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-ma-2)   |
| System and Information Integrity (SI) | [SI-2 (2) Flaw Remediation &#124; Central Management](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-si-2.2) \n [SI-6 Security Functionality Verification](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-si-6)   |
| Security Assessment and Authorization (CA) | [CA-7 Continuous Monitoring](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-ca-7)    |
{: caption="Related controls in {{site.data.keyword.framework-fs_notm}} [FSv1.1]" caption-side="top"}
{: #related-controls-fsv1.1}
{: tab-title="FSv1.1"}
{: tab-group="RelatedControls-1"}
{: class="simple-tab-table"}


## Next steps
{: #next-steps}

* [Operational monitoring](/docs/framework-financial-services?topic=framework-financial-services-shared-monitoring-operational)
