---
copyright:
  years: 2026
lastupdated: "2026-06-03"

keywords: portieris, image policies, image security, admission controller, signature verification

subcollection: framework-financial-services

---

{{site.data.keyword.attribute-definition-list}}

# Configuring image acceptance policies
{: #ocp-image-mgmt-image-acceptance-policies}

Portieris is a Kubernetes admission controller that verifies container images before deploying them to the cluster. You can create image security policies for each namespace or at the cluster level and enforce different rules for different images or namespaces.
{: shortdesc}

If an image doesn't meet the defined policy requirements, the resource that contains the pod is not deployed to the cluster.

## Installing Portieris
{: #install-portieris}

Portieris can be installed by toggling the Image security enforcement option to 'Enabled' from the cluster overview page in IBM Cloud Console or using CLI by following the [documentation](/docs/containers?topic=containers-images#portieris-image-sec){: external}.

## Understanding policy types
{: #policy-types}

For system namespaces (except default namespace) which are automatically created during cluster creation, Image acceptance policies are automatically defined and controlled by Portieris and cannot be overridden by the users.

For custom or new namespaces created by users in the cluster, Portieris Image Acceptance Policies can be enforced to ensure that only whitelisted images are deployed into the pods of the cluster. Portieris defines two custom resource types for policy:

Cluster image policy resources
:   `ClusterImagePolicy` resources are configured at the cluster level and are not specific to any namespace. They take effect whenever an image policy resource, `ImagePolicy`, is not defined in the namespace where the workload is deployed. If a matching policy is not found for an image, deployment is denied.

Image policy resources
:   `ImagePolicy` resources are configured in the target namespace and define Portieris' behavior in that namespace. If image policy resources exist in a namespace, the policies from those image policy resources are used exclusively. If a match doesn't exist for a workload image in `ImagePolicy`, cluster image policy resources, `ClusterImagePolicy`, are not examined.

## Creating a default deny-all cluster policy
{: #create-deny-all-policy}

By default, `ClusterImagePolicy` would allow all the images to new or custom namespaces, and it should be changed to deny all by default so that no random images can be deployed to the cluster.

The following example shows a default policy which denies all the deployment to the cluster. This would be applicable to all the new or custom namespaces:

```yaml
apiVersion: portieris.cloud.ibm.com/v1
kind: ClusterImagePolicy
metadata:
  name: block-all-images
spec:
   repositories: []
```
{: codeblock}

If debug pods need to be run, the debug pod image also has to be whitelisted in the `ClusterImagePolicy`. Debug pod images can be referenced as `quay.io/openshift-release-dev/ocp-v4.0-art-dev*`.

## Creating namespace-specific image policies
{: #create-namespace-policies}

Images are matched against the set of policies defined. If a policy doesn't match the workload image, the deployment of a container is denied. This ensures that only whitelisted images are allowed and denies all other deployment to cluster. The policy should match the original image location, even if the image is pulled from a mirror.

The following example allows any image from a specific repository only and denies deployment from any other repository:

```yaml
apiVersion: portieris.cloud.ibm.com/v1
kind: ImagePolicy
metadata:
  name: test-policy-portieris
  namespace: test-namespace
spec:
   repositories:
    - name: "private.de.icr.io/mirror-registry/*"
    - name: "quay.io/keycloak/keycloak*"
    - name: "registry.redhat.io/ubi-minimal:latest"
      policy:
```
{: codeblock}

## Verifying image signatures
{: #verify-signatures}

To verify image signatures using GPG public keys, you will need to create a secret containing the GPG public key and reference it in the `ImagePolicy` specification:

```yaml
apiVersion: portieris.cloud.ibm.com/v1
kind: ImagePolicy
metadata:
  name: test-policy-portieris
  namespace: test-namespace
spec:
   repositories:
     - name: 'icr.io/appcafe/open-liberty:kernel-slim-java17-openj9-ubi-amd64'
       policy:
         simple:
           requirements:
             - keySecret: open-liberty-signed-image
               type: signedBy
```
{: codeblock}

In this example, an OpenLiberty image is used that is signed by the key provided on the OpenLiberty [project page](https://openliberty.io/docs/latest/verify-signatures-for-container-images-in-open-liberty.html#_before_you_begin){: external}. The opaque secret `open-liberty-signed-image` needs to be created in the same namespace and have a key "key" with data containing the GPG public key as a text.

## Example: Allowing Sysdig agent for Security and Compliance Center Workload Protection
{: #example-sysdig-policy}

The following example shows how to allow installation of Sysdig agent for Security and Compliance Center Workload Protection:

```yaml
apiVersion: portieris.cloud.ibm.com/v1
kind: ImagePolicy
metadata:
  name: allow-sysdig-policy
  namespace: ibm-observe
spec:
  repositories:
    - name: 'icr.io/ext/sysdig/*'
```
{: codeblock}

## Next steps
{: #next-steps}

* Set up [vulnerability scanning](/docs/framework-financial-services?topic=framework-financial-services-ocp-image-mgmt-vulnerability-scanning)
