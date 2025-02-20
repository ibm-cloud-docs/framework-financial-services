---

copyright:
  years: 2020, 2025
lastupdated: "2025-02-20"

keywords:

subcollection: framework-financial-services

---

{{site.data.keyword.attribute-definition-list}}

# Development processes and software integrity
{: #shared-development-processes}

Development processes and ensuring system and software integrity are key parts of the {{site.data.keyword.framework-fs_notm}}. You must manage changes to the information system by using a defined system development life cycle (SDLC) that incorporates information security considerations.
{: shortdesc}

You should ensure that security engineering principles are applied in the design, development, implementation, and modification of the system. During the ongoing development, implementation, operations, maintenance, and continuous monitoring of the overarching information system lifecycle, you must develop, implement, operate, and maintain the system in accordance with the security processes and technical requirements that are detailed in the {{site.data.keyword.framework-fs_notm}} and the accompanying operational plans and policies.

You must ensure that developers:

* Perform configuration management during system/component/service development, implementation, and operation.
* Document change approvals and security impact analyses for all system changes.
* Perform unit, integration, system, regression testing/evaluation.
* Perform static and dynamic code analysis.
* Use development testing processes that provide coverage for the entire component and explicitly reviews, evaluates, and tests all security functions.
* Control consumer data so that it is never placed into nonproduction environments. Consumer data must not be consumed or used for the purposes of testing services.
* Enable integrity verification of software components to detect unauthorized changes to system software, firmware, and information:
    * All applicable software components must be cryptographically signed by the manufacturer or developer to ensure you can perform integrity checks and verify that the component that is deployed in the system is the same component that the manufacturer or developer evaluated and certified.
    * Deploy only signed and approved builds within the environment.
* Ensure only the approved, tested changes/code gets promoted to production and no vulnerabilities are introduced.
* Document and evidence the execution of the system/service and security testing/scanning along with the results.
* Track security flaws and flaw resolution within the system and report findings to designated personnel.

## {{site.data.keyword.openshiftshort}}
{: #openshift-software-integrity}

When using {{site.data.keyword.openshiftlong_notm}} to host workloads, you must use {{site.data.keyword.registryshort}} and the Vulnerability Advisor component it contains. {{site.data.keyword.openshiftlong_notm}} provides a multi-tenant, highly available, scalable, and encrypted private image registry that is hosted and managed by {{site.data.keyword.IBM}}. You can use {{site.data.keyword.registryshort}} by setting up your own image namespace and pushing container images to your namespace. By using {{site.data.keyword.registryshort}}, only users with access to your {{site.data.keyword.cloud_notm}} account can access your images.

When you push images to {{site.data.keyword.registryshort}}, you benefit from the built-in Vulnerability Advisor features that scan for potential security issues and vulnerabilities. Vulnerability Advisor checks for vulnerable packages in specific Docker base images, and known vulnerabilities in app configuration settings. When vulnerabilities are found, information about the vulnerability is provided. You can use this information to resolve security issues so that containers are not deployed from vulnerable images.

Any issues that are found by Vulnerability Advisor result in a verdict that indicates that it is not advisable to deploy this image. If you choose to deploy the image, any containers that are deployed from the image include known issues that might be used to attack or otherwise compromise the container. The verdict is adjusted based on any exemptions that you specified. This verdict can be used by Portieris to prevent the deployment of nonsecure images in {{site.data.keyword.registryshort}}. [Portieris](https://github.com/IBM/portieris){: external} is a Kubernetes admission controller for the enforcement of image security policies. You can create image security policies for each Kubernetes namespace, or at the cluster level, and enforce different rules for different images.

Fixing the security and configuration issues that are reported by Vulnerability Advisor can help you to secure your {{site.data.keyword.cloud_notm}} infrastructure.

See the following for more information on {{site.data.keyword.registryshort}} and how to set it up:

* [About {{site.data.keyword.registryshort}}](/docs/Registry?topic=Registry-registry_overview)
* [Setting up {{site.data.keyword.registryshort}} as a private registry on {{site.data.keyword.openshiftlong_notm}}](/docs/Registry?topic=Registry-registry_rhos)
* [Managing image security with Vulnerability Advisor](/docs/Registry?topic=Registry-va_index&interface=ui)
* [Signing images for trusted content](/docs/Registry?topic=Registry-registry_trustedcontent)
* [{{site.data.keyword.registryshort}} and Vulnerability Advisor workflow tutorial](/docs/Registry?topic=Registry-registry_tutorial_workflow)

## Virtual server instances



There is not a Financial Services Validated solution for scanning virtual server images. You need to install your own software solution.
{: important}

## Related controls in {{site.data.keyword.framework-fs_notm}}
{: #related-controls}

{{site.data.content.related-controls-disclaimer}}

| Family              | Control                                           |
|---------------------|---------------------------------------------------|
| System and Information Integrity (SI) | [SI-7 Software & Information Integrity](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-si-7) |
| System and Services Acquisition (SA) | [SA-3 System Development Life Cycle](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sa-3) \n [SA-8 Security Engineering Principles](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sa-8) \n [SA-10 Developer Configuration Management](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sa-10) \n [SA-10 (1) Developer Configuration Management &#124; Software and Firmware Integrity Verification](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sa-10.1) \n [SA-11 Developer Security Testing and Evaluation](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sa-11) \n [SA-15 Development Process, Standards, and Tools](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sa-15) \n [SA-15 (9) Development Process, Standards, and Tools &#124; Use of Live Data](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sa-15.9) |
| Risk Assessment (RA) | [RA-5 Vulnerability Scanning](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-ra-5)  |
{: caption="Related controls in {{site.data.keyword.framework-fs_notm}} [FSv2.0]" caption-side="top"}
{: #related-controls-fsv2.0}
{: tab-title="FSv2.0"}
{: tab-group="RelatedControls-1"}
{: class="simple-tab-table"}


| Family              | Control                                           |
|---------------------|---------------------------------------------------|
| System and Information Integrity (SI) | [SI-7 Software & Information Integrity](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-si-7) |
| System and Services Acquisition (SA) | [SA-3 System Development Life Cycle](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-sa-3) \n [SA-8 Security Engineering Principles](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-sa-8) \n [SA-10 Developer Configuration Management](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-sa-10) \n [SA-10 (1) Developer Configuration Management &#124; Software and Firmware Integrity Verification](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-sa-10.1) \n [SA-11 Developer Security Testing and Evaluation](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-sa-11) \n [SA-15 Development Process, Standards, and Tools](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-sa-15) \n [SA-15 (9) Development Process, Standards, and Tools &#124; Use of Live Data](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-sa-15.9) |
| Risk Assessment (RA) | [RA-5 Vulnerability Scanning](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-ra-5)  |
{: caption="Related controls in {{site.data.keyword.framework-fs_notm}} [FSv1.1]" caption-side="top"}
{: #related-controls-fsv1.1}
{: tab-title="FSv1.1"}
{: tab-group="RelatedControls-1"}
{: class="simple-tab-table"}


## Next steps
{: #next-steps}

* [Responsibilities for operating services in your deployment](/docs/framework-financial-services?topic=framework-financial-services-shared-responsibilities)
