---
copyright:
  years: 2026
lastupdated: "2026-06-09"

keywords: icr, container registry, configuration, namespaces, access control, vpe

subcollection: framework-financial-services

---

{{site.data.keyword.attribute-definition-list}}

# Configuring IBM Cloud Container Registry
{: #ocp-image-mgmt-icr-configuration}

Implementation guidance for control CM-6 - Configuration Settings requires that all container images are stored in authorized managed image registries. IBM Cloud Container Registry with private namespaces should be used for the workloads deployed on your {{site.data.keyword.openshiftshort}} cluster.
{: shortdesc}

## Before you begin
{: #before-you-begin}

Ensure that your cluster VPC infrastructure already implements the boundary protection measures and outbound traffic restrictions:

* Public gateways should be deployed only on VPC subnets where the workloads deployed on cluster worker nodes require outbound access to allow-listed specific external endpoints. Direct access to external registries should not be allowed from cluster worker nodes.
* No routes in the VPC that would allow unrestricted egress to public networks from cluster nodes.
* ACL for cluster subnets do not allow unrestricted connections outside of VPC address space and IBM Cloud services (no CIDR 0.0.0.0/0).

## Creating and organizing {{site.data.keyword.registryshort}} namespaces
{: #create-namespaces}

When [creating new private namespaces in {{site.data.keyword.registryshort}}](/docs/Registry?topic=Registry-registry_setup_cli_namespace&interface=ui#registry_setup_cli_namespace_plan){: external}, assign them to resource groups based on their security or workload classification. This will help to organize IAM access policies that may be using resource groups for qualification.

* Avoid using Default resource group or resource groups used for purposes not related to the images to be stored in the namespace.
* Consider creating separate namespace(s) for operator images if you are planning to mirror them.

## Setting up staging and production namespaces
{: #staging-production}

Create separate {{site.data.keyword.registryshort}} namespaces using appropriate naming conventions for Staging vs. Production image sets and use separate mirroring processes to update them.

1. Ensure that the images in Staging namespaces are scanned for vulnerabilities and tested for update impact before mirroring them to production namespaces.
2. Set up separate mirroring processes for copying images to Staging namespace from the source and then mirroring from Staging to Production based on validation results using a separate Image Set configuration.

## Configuring access control
{: #access-control}

### Creating Service IDs for image pulling
{: #service-ids-pull}

Create IBM Cloud IAM Service IDs with [limited access](/docs/Registry?topic=Registry-iam_access#service_id){: external} (for example, Reader service access) scoped to [specific namespaces](/docs/Registry?topic=Registry-iam_access#access_resources){: external} to be used in pull secret configuration.

This will ensure that minimally required access is granted for pulling the images by the cluster. The default cluster pull secrets include credentials for a cluster's IBM Cloud IAM Service ID with reader access to your {{site.data.keyword.registryshort}} namespaces.

### Creating Service IDs for image mirroring
{: #service-ids-push}

Create separate IBM Cloud IAM Service IDs for mirroring process that will be used to push images to your private namespaces in {{site.data.keyword.registryshort}}.

1. Assign Writer service role, optionally scoped to specific namespaces.
2. These API keys should only be used in the processes that perform the image mirroring, typically outside of the {{site.data.keyword.openshiftshort}} cluster.

## Setting up network access
{: #network-access}

### Creating Virtual Private Endpoints
{: #create-vpe}

Create [Virtual Private Endpoints](/docs/Registry?topic=Registry-registry_vpe&interface=ui){: external} for controlled access to {{site.data.keyword.registryshort}} from the compute resources in your VPCs.

### Configuring Context Based Restrictions
{: #configure-cbr}

Create [Context Based Restrictions for {{site.data.keyword.registryshort}}](/docs/Registry?topic=Registry-registry-cbr&interface=ui){: external} to enforce access over private endpoints and specific source networks.

## Using {{site.data.keyword.registryshort}} with OpenShift
{: #using-icr-openshift}

Once you have the {{site.data.keyword.registryshort}} namespaces and proper access control configured, you can set up your {{site.data.keyword.openshiftshort}} cluster to use {{site.data.keyword.registryshort}} instead of the internal registry.

For more information, see [Using IBM Cloud Container Registry with OpenShift](/docs/openshift?topic=openshift-registry#openshift_iccr){: external}.

For the deployments where you can modify the container image reference, you will need to update the image location in the deployment manifest to point to the image that you pushed to your private {{site.data.keyword.registryshort}} namespace.

## Managing images in {{site.data.keyword.registryshort}}
{: #managing-images}

The mirroring process will not remove images from the target {{site.data.keyword.registryshort}} namespace even if they are not in the Image Set configuration for mirroring. Therefore, you will need to manually remove images that are no longer used. You should also remove obsolete image versions with identified vulnerabilities.

## Next steps
{: #next-steps}

* Learn how to [copy images to {{site.data.keyword.registryshort}} with oc-mirror CLI](/docs/framework-financial-services?topic=framework-financial-services-ocp-image-mgmt-copying-images-oc-mirror)
* Configure [registry mirroring on {{site.data.keyword.openshiftshort}} nodes](/docs/framework-financial-services?topic=framework-financial-services-ocp-image-mgmt-registry-mirroring-config)
