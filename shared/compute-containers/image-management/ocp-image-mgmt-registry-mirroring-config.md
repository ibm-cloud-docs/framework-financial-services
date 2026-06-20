---
copyright:
  years: 2026
lastupdated: "2026-06-03"

keywords: registry mirroring, cri-o, daemonset, pull secrets, openshift nodes

subcollection: framework-financial-services

---

{{site.data.keyword.attribute-definition-list}}

# Configuring registry mirroring on {{site.data.keyword.openshiftshort}} nodes
{: #ocp-image-mgmt-registry-mirroring-config}

The CRI-O container engine on {{site.data.keyword.openshiftshort}} nodes uses configuration files to resolve the requests for pulling an image from a registry. The configuration can be updated to include an alternative location.
{: shortdesc}

## Understanding registry mirroring
{: #understanding-mirroring}

Consider the following example for the registry configuration on a node:

```text
[[registry]]
prefix = "quay.io/example"
insecure = false
blocked = false
location = "private.icr.io/acme-workloads"

[[registry.mirror]]
location = "private.de.icr.io/acme-workloads"
```
{: codeblock}

When a pull of `quay.io/example/image:latest` is requested by a deployment, the node will request the image from `private.de.icr.io/acme-workloads/image:latest` instead, thus avoiding the need for a direct connection to an external public registry (`quay.io`).

The access to {{site.data.keyword.registryshort}} requires valid IBM Cloud API key which can be provided at a namespace or deployment level as a pull secret in case of regular workload deployment manifests. However, for operator deployments and catalog mirroring, the pull secret must be at the cluster level and needs to be distributed to nodes via a DaemonSet.

Once the registry mirroring and pull secrets are configured on the nodes, you will not need to modify your deployment manifests for image references from external registries. Instead, the images with references to those registries will be pulled from specified {{site.data.keyword.registryshort}} locations.

## Before you begin
{: #before-you-begin}

IBM Cloud does not currently support updating the `ImageContentSourcePolicy`, `ImageDigestMirrorSet`, and `ImageTagMirrorSet`. Therefore, DaemonSets can be used to emulate the above-mentioned policies on the nodes.

Once the images are mirrored to the target registry ({{site.data.keyword.registryshort}}) using `oc-mirror` plugin, two DaemonSets need to be created on the cluster.

## Creating a DaemonSet for registry configuration update
{: #create-registry-daemonset}

### Creating the mapping file
{: #create-mapping-file}

Create a new `mapping.txt` file with source and mirror references for the registries and images that you copied to {{site.data.keyword.registryshort}}, using the following pattern:

```text
registry.redhat.io=private.de.icr.io/mirror-registry
quay.io=private.de.icr.io/mirror-registry
```
{: codeblock}

### Creating the ConfigMap
{: #create-configmap}

Create a new ConfigMap using the `mapping.txt` in the `kube-system` namespace using the following command:

```bash
oc create configmap registry-mapping --from-file mapping.txt -n kube-system
```
{: codeblock}

### Deploying the DaemonSet
{: #deploy-registry-daemonset}

Deploy a new DaemonSet for updating the mirror registry configuration to `registries.conf.d` directory in the node. Use the sample YAML in the [GitHub repository](https://github.com/IBM/fs-solution-guides-resources/blob/main/container-image-management/k8s-artifacts/registry-conf-daemonsets/registries-conf-daemonset.yaml){: external} for creating the DaemonSet.

## Creating a DaemonSet for updating node level {{site.data.keyword.registryshort}} pull secret
{: #create-pullsecret-daemonset}

### Creating the pull secret
{: #create-pull-secret}

Create a new secret using the API key credentials created earlier using the following command:

```bash
oc create secret docker-registry docker-auth-secret \
--docker-server=private.de.icr.io \
--docker-username=iamapikey \
--docker-password=<password> \
--namespace kube-system
```
{: codeblock}

### Deploying the DaemonSet
{: #deploy-pullsecret-daemonset}

Create a new DaemonSet for updating the {{site.data.keyword.registryshort}} pull secret in the node. The sample YAML is available in the [GitHub repository](https://github.com/IBM/fs-solution-guides-resources/blob/main/container-image-management/k8s-artifacts/registry-conf-daemonsets/pull-secret-daemonset.yaml){: external} that can be used for deployment.

## Updating DaemonSets
{: #updating-daemonsets}

DaemonSets need to be restarted by using the following command whenever there is an update to secret or ConfigMap created in the previous steps:

```bash
oc rollout restart ds <daemonset-name> -n <namespace>
```
{: codeblock}

## Next steps
{: #next-steps}

* Configure [operator mirroring and catalog sources](/docs/framework-financial-services?topic=framework-financial-services-ocp-image-mgmt-operator-mirroring)
* Set up [image acceptance policies](/docs/framework-financial-services?topic=framework-financial-services-ocp-image-mgmt-image-acceptance-policies)
