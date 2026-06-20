---
copyright:
  years: 2026
lastupdated: "2026-06-09"

keywords: planning, security, compliance, container image security, ibm cloud image management, image management considerations

subcollection: framework-financial-services

---

{{site.data.keyword.attribute-definition-list}}

# Key considerations for evaluating image management options
{: #ocp-image-mgmt-planning-considerations}

Review key considerations for evaluating container image management strategies on IBM Cloud, including security, compliance, and auditability requirements.
{: shortdesc}

## Security and compliance
{: #security-compliance}

Solution infrastructure deployed on IBM Cloud following IBM Cloud for Financial Services framework must adhere to the regulations and security controls that govern secure software supply chain practices, software integrity, boundary protection, and outbound traffic control policies, as applicable to container image management:

Container images should only be stored on authorized managed registries
:   The use of unauthorized registries to store images must be forbidden (except for the tools responsible to internalize images). See [SI-7](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-si-7){: external} and [CM-6](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-cm-6){: external}.

Network access to external resources on public network should be restricted to a predefined allow-list
:   See [SI-4 (4)](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-si-4.4){: external}, [SC-7](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-7){: external}, and [SC-7 (5)](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-7.5){: external}.

Container images should be checked for integrity and vulnerabilities
:   See [SI-7](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-si-7){: external} and [CM-6](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-cm-6){: external}.

## Auditability
{: #auditability}

The container image management process should provide a clear picture of the image inventory deployed within the IBM Cloud solution infrastructure and the state of their vulnerability exposure.

## Mitigating security concerns
{: #security-concerns}

Security risks such as access to unauthorized container registries, deployment of container images without proper security checks, or newly discovered vulnerabilities must be mitigated. Specific access controls should be applied to the processes that facilitate mirroring of external registries and making the container images available to the deployed clusters in IBM Cloud.

## Secure software development
{: #secure-development}

For container images built by the solution operators, a secure software development environment such as IBM [DevSecOps with Continuous Delivery](/docs/devsecops?topic=devsecops-devsecops_intro){: external} is recommended to ensure end-to-end security of the custom software components.

## Decision matrix
{: #decision-matrix}

Use the following table to help determine the best approach for your workload:

| Criteria | Questions to ask | Recommended option |
| -------- | ---------------- | ------------------ |
| Containerized workload deployment process | Does your workload consist of custom container images with full control over deployment manifests? | Publish your images to private {{site.data.keyword.registryshort}} namespace, modify image references in deployment manifests to point to {{site.data.keyword.registryshort}}. You may not need registry mirroring configuration on cluster nodes. |
| Use of public container images for deployments | Do you use container images from public registries in your own deployment manifests? | Set up image mirroring process for the target images, modify image references in deployment manifests to point to {{site.data.keyword.registryshort}}. You may not need registry mirroring configuration on cluster nodes. |
| Use of {{site.data.keyword.openshiftshort}} operators from public marketplaces | Do you need to install operators from public marketplaces? | Set up image mirroring process for the target operators, configure registry mirroring and registry credentials on cluster nodes using DaemonSets. |
| Access to image registries on private networks | Do you deploy workloads using images from registries on private networks such as enterprise internal registries? | Set up image mirroring process running within a private network that also has a secure connection to IBM Cloud and mirror the required images to a private namespace in {{site.data.keyword.registryshort}}. Modify image references in deployment manifests to point to {{site.data.keyword.registryshort}}. You may not need registry mirroring configuration on cluster nodes. |
{: caption="Decision matrix for image management approaches" caption-side="bottom"}

## Next steps
{: #next-steps}

* Start implementing your image management strategy with [{{site.data.keyword.registryshort}} configuration](/docs/framework-financial-services?topic=framework-financial-services-ocp-image-mgmt-icr-configuration)
