---

copyright:
  years: 2020, 2022
lastupdated: "2022-07-28"

keywords: 

subcollection: framework-financial-services

---

{{site.data.keyword.attribute-definition-list}}

# Getting started with {{site.data.keyword.cloud_notm}} for Financial Services
{: #about}

[{{site.data.keyword.cloud_notm}} for Financial Services™](https://www.ibm.com/cloud/financial-services){: external} designed, from its application-specific integrated circuits (ASICs) to its processes, to address cybersecurity and regulatory challenges, and deliver the benefits and flexibility of a public cloud in a secure and compliant environment.
This enables financial services institutions, independent software vendors (ISVs), and Software as a Service (SaaS) providers to host applications and workloads in the cloud with confidence and trust.

{{site.data.keyword.cloud_notm}} for Financial Services is designed to build trust and enable a transparent public cloud ecosystem with the features for security, compliance, and resiliency that financial institutions require. Financial institutions can confidently host their mission-critical applications in the cloud and transact quickly and efficiently. With a partner ecosystem of more than 90 ISV, SaaS, and fintech partners, {{site.data.keyword.cloud_notm}} for Financial Services offers a new generation of cloud for the enterprise. Financial institutions can now deploy on public cloud to enable innovation and deliver new outstanding customer experiences, while managing stringent industry regulations for sensitive data and complex workloads.

{{site.data.keyword.cloud_notm}} for Financial Services helps improve efficiency and enhance competitiveness. It enables delivery against regulatory
imperatives through a consistent set of embedded controls, defined as part of the {{site.data.keyword.framework-fs_full}}, which is designed to meet the needs of financial institution control stakeholders: chief risk officers (CROs), chief compliance officers (CCOs), and chief information security officers (CISOs). In addition,
it supports the strategic mandates of chief executive officers (CEOs) and chief information or chief technology officers (CIOs/CTOs).

For more information, see [Accelerate innovation through security and compliance with {{site.data.keyword.cloud_notm}} for Financial Services](https://www.ibm.com/downloads/cas/1OLRGDBA){: external}.

## {{site.data.keyword.framework-fs_notm}}
{: #ibm-cloud-framework-for-financial-services}

{{site.data.keyword.framework-fs_full}} is designed to help address the needs of financial services institutions with regulatory compliance, security, and resiliency during the initial deployment phase and with ongoing operations. The framework also helps to simplify the ability of financial institutions to transact with technology vendors who deliver software or SaaS applications, and who meet the requirements of the framework.

The {{site.data.keyword.framework-fs_notm}} consists of:

* A comprehensive set of controls designed to help address the security requirements and regulatory compliance obligations of financial institutions and cloud best practices. The cloud best practices include a shared responsibility model across financial institutions, application providers, and {{site.data.keyword.cloud_notm}}.
* Reference architectures and detailed control-by-control guidance for implementation and supporting evidence to help address the security and regulatory requirements of the financial industry.
* Tools and {{site.data.keyword.IBM_notm}} services to enable parties to efficiently and effectively monitor compliance, remediate issues, and generate evidence of compliance.
* Ongoing governance of the framework documentation that considers new and changing regulations, as well as bank and public cloud requirements.

### Interpreting the framework
{: #interpreting-guidance}

We refer to the entity that uses (consumes) an application as the _consumer_ and the entity that develops or deploys (provides) an application as the _application provider_. The consumer and application provider might be the same when the organization providing the application also consumes it (for example, a financial institution deploys an application for internal use by others in their company). Or, they might be different if a third-party technology vendor is providing an application for a financial institution to use.
{: note}

You should use the information in the framework if you are an application provider (whether from a financial institution or technology vendor) who wishes to meet the requirements or leverage the best practices of {{site.data.keyword.cloud_notm}} for Financial Services. However, the content has a different purpose depending on whether you represent an application provider from a technology vendor or an application provider within a financial institution.

Technology vendor
:   The information represents a set of _required_ controls and guidance if you want to pursue a designation of [{{site.data.keyword.cloud_notm}} for Financial Services Validated](#becoming-fs-validated) from {{site.data.keyword.IBM_notm}}. The Financial Services Validated designation signifies that you have successfully evidenced compliance to the controls of the {{site.data.keyword.framework-fs_notm}} and may improve your ability to market to financial institutions.

Financial institution
:   The information is a set of _recommended_ best practices. As a financial institution, you know your controls better than {{site.data.keyword.IBM_notm}} does. So, you have latitude in terms of following the best practices depending on what your internal controls dictate and the level of risk you are willing to take.

In addition, the framework is intended to cover the two types of applications that you might deliver as an application provider:

* Software as a service (SaaS) – You develop and operate the application that is deployed on the {{site.data.keyword.cloud_notm}} by using one of the reference architectures for the {{site.data.keyword.cloud_notm}} for Financial Services.
* Software – You develop the application software, and another organization deploys and operates the application on their infrastructure in their own {{site.data.keyword.cloud_notm}} account.

### Control requirements
{: #framework-control-requirements}

The technology-agnostic controls defined in the framework were built by the industry for the industry. The framework contains 565 control requirements that span 7 focus areas and 21 control families. The controls are based on [NIST 800-53 Rev 4](https://csrc.nist.gov/Projects/risk-management/sp800-53-controls/release-search#!/800-53?version=4.0){: external}, with enhancements based on feedback from leading industry partners.

See [{{site.data.keyword.framework-fs_full}} - Control Requirements](https://cloud.ibm.com/media/docs/downloads/framework-financial-services/IBM_Cloud_Framework_for_Financial_Services_-_Control_Requirements_v1.1.0.xlsx){: external} for a spreadsheet of the control requirements.

### Control implementation overview templates
{: #control-implementation-overview-templates}

The templates that are referenced in this section are available under NDA only. Work with your {{site.data.keyword.IBM_notm}} account representative to get access.
{: tip}



The framework's control implementation overview (CIO) templates provide detailed implementation and evidence guidance for all of the framework's control requirements. Each of the templates provides guidance specific to the underlying {{site.data.keyword.cloud_notm}} technology used in one of the [reference architectures](#framework-reference-architectures). They are referred to as "templates" because they allow you to assess how well your solution meets each of the control requirements. A completed CIO template is a requirement for any technology vendor seeking to be Financial Services Validated.



### Reference architectures and deployment guidance
{: #framework-reference-architectures}

[Reference architectures](/docs/framework-financial-services?topic=framework-financial-services-reference-architecture-overview) have been defined which can be used as a basis for meeting the security and regulatory requirements defined by the controls. The reference architectures each center on specific {{site.data.keyword.cloud_notm}} technologies, including:

{{site.data.content.reference-architectures-list-full-names}}

In addition, prescriptive [deployment and configuration guidance](/docs/framework-financial-services?topic=framework-financial-services-shared-deployment-setup-environment) is provided for the components in each of these reference architectures. The guidance is intended as an easier to consume supplement to the CIO templates that is more appropriate for architects and developers trying to build and implement applications for {{site.data.keyword.cloud_notm}} for Financial Services.

## Becoming {{site.data.keyword.cloud_notm}} for Financial Services Validated
{: #becoming-fs-validated}

{{site.data.keyword.cloud_notm}} for Financial Services Validated designates that an {{site.data.keyword.cloud_notm}} service or ecosystem partner service has evidenced compliance to the controls of the {{site.data.keyword.framework-fs_notm}} and can be used to build solutions that might themselves be validated. Technology vendors who are interested in being {{site.data.keyword.cloud_notm}} for Financial Services Validated are required to complete a CIO template for evaluation by {{site.data.keyword.IBM_notm}} and to comply with the requirements of the {{site.data.keyword.framework-fs_notm}}.

Through the shared responsibility model of the {{site.data.keyword.framework-fs_notm}} and the surrounding standardized processes, financial institutions and technology vendors get benefits such as:

* Less time that is spent by technology vendors demonstrating compliance and more time delivering innovative services.
* Reduction in the time and effort by financial institutions to ensure the compliance of third-party vendors, and more time spent delivering new, innovative services to their customers.
* Streamlined procurement, contracting, and onboarding within the ecosystem that leads to reduced time to market for all parties.

In addition, technology vendors (even those who are not yet {{site.data.keyword.cloud_notm}} for Financial Services Validated) are encouraged to [onboard to the {{site.data.keyword.cloud_notm}} catalog](/docs/framework-financial-services?topic=framework-financial-services-onboarding-to-catalog)

## Next steps
{: #next-steps}

If you plan to provide SaaS:

* Review [Best practices for software as a service](/docs/framework-financial-services?topic=framework-financial-services-best-practices), which represent a generic set of implementation principles that apply to SaaS applications on both reference architectures.
* Explore the [reference architectures](/docs/framework-financial-services?topic=framework-financial-services-reference-architecture-overview) and deployment guidance

If you plan to provide software that another organization will operate, review [Best practices for software](/docs/framework-financial-services?topic=framework-financial-services-best-practices-software).
