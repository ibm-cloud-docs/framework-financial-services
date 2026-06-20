---
copyright:
  years: 2026
lastupdated: "2026-06-03"

keywords: oc-mirror, image mirroring, code engine, vsi, container images

subcollection: framework-financial-services

---

{{site.data.keyword.attribute-definition-list}}

# Copying images to {{site.data.keyword.registryshort}} with oc-mirror CLI
{: #ocp-image-mgmt-copying-images-oc-mirror}

The `oc-mirror` plugin can be used for mirroring images to private {{site.data.keyword.registryshort}} from external registries. You can run the `oc-mirror` plugin using either a VSI or as a Code Engine job.
{: shortdesc}

## Before you begin
{: #before-you-begin}

Create a new IBM Cloud IAM Service ID that is granted Reader and Writer access to your {{site.data.keyword.registryshort}} private namespaces that will be used for the mirrored images. In the following examples, `container-image-mirror-service` is used as the Service ID name.

## Option 1: Running oc-mirror plugin using a VSI
{: #option-vsi}

To use the `oc-mirror` plugin to mirror registry images, you must install the plugin. If you are mirroring image sets in a fully disconnected environment, ensure that you install the `oc-mirror` plugin on the host with internet access and the host in the disconnected environment with access to the mirror registry.

The credentials for the {{site.data.keyword.registryshort}} access will be stored in a local file.

### Installing the oc-mirror plugin
{: #install-plugin-vsi}

1. Download and install the `oc-mirror` plugin following the instructions in the [Red Hat documentation](https://docs.redhat.com/en/documentation/openshift_container_platform/4.12/html/disconnected_installation_mirroring/installing-mirroring-disconnected#installation-oc-mirror-installing-plugin_installing-mirroring-disconnected){: external}.
2. Make sure you have `ibmcloud` CLI, `oc` CLI, and `jq` tool installed. The command examples are for bash shell.

### Setting up authentication
{: #setup-auth-vsi}

Run the following commands to create a new API key for the IBM Cloud IAM Service ID, pull the Red Hat credentials from the cluster to a local file `image-pull-secret.json`, and add the {{site.data.keyword.registryshort}} credentials (API key created) to it. You need to log in to IBM Cloud with `ibmcloud` CLI before running the commands. Update the values in the export commands as needed to match your environment.

```bash
export SERVICE_ID_NAME=container-image-mirror-service
export REGISTRY=private.de.icr.io
export CLUSTER_NAME=workload-cluster

apikey=$(ibmcloud iam service-api-key-create oc-mirror-icr-access-key-vsi \
"$SERVICE_ID_NAME" -d 'Credentials for ICR access from VSI' --output json \
| jq -r '.apikey')

ibmcloud oc cluster config -c $CLUSTER_NAME --admin

oc get secret -n openshift-config pull-secret \
-o template='{{index .data ".dockerconfigjson"}}' | base64 --decode \
| jq --arg registry "$REGISTRY" \
--arg auth "$(echo -n iamapikey:$apikey | base64 -w0)" \
'.auths[$registry].auth = $auth' > image-pull-secret.json
```
{: codeblock}

If you need to access other registries not included in your cluster's pull secret, edit the `image-pull-secret.json` file to add the authentication values for these registries. For more information, see [Adding mirror registry authentication details](https://docs.redhat.com/en/documentation/red_hat_openshift_container_storage/4.5/html/preparing_to_deploy_in_a_disconnected_environment/adding-mirror-registry-authentication-details_rhocs){: external}.

### Creating ImageSetConfiguration
{: #create-imageset-vsi}

Create an `ImageSetConfiguration` YAML file with the list of operators and images to be mirrored. The following example shows a sample `ImageSetConfiguration`. For more details on operator versions and channels to be mirrored, see [Defining and configuring your operators for use with oc-mirror](https://developers.redhat.com/learning/learn:openshift:master-operator-mirroring-oc-mirror/resource/resources:defining-and-configuring-your-operators-use-oc-mirror){: external}. Save the file as `mirror-imageset.yaml`.

```yaml
kind: ImageSetConfiguration
apiVersion: mirror.openshift.io/v2alpha1
mirror:
  platform:
  operators:
    - catalog: registry.redhat.io/redhat/redhat-operator-index:v4.20
      packages:
        - name: volsync-product
        - name: cluster-observability-operator
    - catalog: quay.io/operatorhubio/catalog:latest
      packages:
        - name: keycloak-operator
  additionalImages:
  - name: quay.io/keycloak/keycloak:26.5.6
  - name: registry.redhat.io/ubi9-minimal:latest
```
{: codeblock}

In the Image Set configuration, you will need to list the catalog images for the catalogs you intend to use. For example, the OperatorHub.io catalog is published on `quay.io/operatorhubio/catalog:latest`. Under each catalog entry, you can list the operators (as packages) that you need to mirror. You can configure multiple catalog sources in the single Image Set.

Some operators require additional images to be pulled as they do not provide complete operator bundles. Those images should be listed under `additionalImages`.

### Running oc-mirror
{: #run-oc-mirror-vsi}

Run the `oc-mirror` v2 command to start the mirroring to target registry. Make sure to add `umask 0022` to skip cache issues for index image. Update the values in the export commands as needed to match your environment. Make sure the namespace used as `TARGET_NAMESPACE` has been created as described in [Configuring IBM Cloud Container Registry](/docs/framework-financial-services?topic=framework-financial-services-ocp-image-mgmt-icr-configuration).

```bash
export REGISTRY=private.de.icr.io
export TARGET_NAMESPACE=ocp-operator-mirror

oc-mirror --config mirror-imageset.yaml --workspace file://oc-mirror-v2 \
docker://$REGISTRY/$TARGET_NAMESPACE --v2 --authfile image-pull-secret.json
```
{: codeblock}

The `oc-mirror` plugin automatically creates the index images with the subset of operators in the target registry and CatalogSource in the `oc-mirror-v2` directory. The CatalogSource content will be used later on the cluster to allow operator installation.

## Option 2: Running oc-mirror plugin as a Code Engine job
{: #option-code-engine}

### Creating a Code Engine project
{: #create-ce-project}

Create a new serverless project for Code Engine using the following command:

```bash
ibmcloud ce project create --name <project-name>
```
{: pre}

### Creating a registry secret
{: #create-registry-secret}

Create a new registry secret by generating a new API key for the IBM Cloud IAM Service ID configured in the prerequisites section:

```bash
export SERVICE_ID_NAME=container-image-mirror-service
export REGISTRY=private.de.icr.io

apikey=$(ibmcloud iam service-api-key-create oc-mirror-icr-access-key-ce \
"$SERVICE_ID_NAME" -d 'Credentials for ICR access from CE' --output json \
| jq -r '.apikey')

ibmcloud ce registry create --name icr-secret \
--server $REGISTRY --username iamapikey --password-from-file <(echo -n "$apikey")
```
{: codeblock}

### Setting up the build configuration
{: #setup-build-config}

Set up the build configuration in the Code Engine project by using the following command. The Dockerfile used for the image build is available in the [GitHub repository](https://github.com/IBM/fs-solution-guides-resources/tree/main/container-image-management/oc-mirror-ce){: external}.

It is recommended to have this Dockerfile in a private registry and update the references in the following command based on the private repository path. SSH key also needs to be added as an additional parameter to the command while using the private repository.

```bash
ibmcloud ce build create --name oc-mirror-cli-image-build \
--build-type git \
--source https://github.com/IBM/fs-solution-guides-resources.git \
--context-dir container-image-management/oc-mirror-ce \
--dockerfile ce-image.dockerfile \
--image $REGISTRY/oc-mirror-tools/oc-mirror-tool:latest
```
{: codeblock}

### Building the custom image
{: #build-custom-image}

Run the following build command to create the custom image and push it to {{site.data.keyword.registryshort}}:

```bash
ibmcloud ce buildrun submit --build oc-mirror-cli-image-build
```
{: codeblock}

### Creating authentication secret
{: #create-auth-secret}

Run the following commands to create a new API key for the IBM Cloud IAM Service ID, pull the Red Hat credentials from the cluster to a local file `image-pull-secret.json`, and add the {{site.data.keyword.registryshort}} credentials (API key created) to it. Update the values in the export commands as needed to match your environment.

```bash
export SERVICE_ID_NAME=container-image-mirror-service
export REGISTRY=private.de.icr.io
export CLUSTER_NAME=workload-cluster

apikey=$(ibmcloud iam service-api-key-create oc-mirror-icr-pull-secret-key-ce \
"$SERVICE_ID_NAME" -d 'Credentials for ICR access from CE' --output json \
| jq -r '.apikey')

ibmcloud oc cluster config -c $CLUSTER_NAME --admin

icr_auth=$(oc get secret -n openshift-config pull-secret \
-o template='{{index .data ".dockerconfigjson"}}' | base64 --decode \
| jq --arg registry "$REGISTRY" \
--arg auth "$(echo -n iamapikey:$apikey | base64 -w0)" \
'.auths[$registry].auth = $auth')

ibmcloud ce secret create --name oc-mirror-auth \
--from-file image-pull-secret.json=<(echo -n "$icr_auth")
```
{: codeblock}

### Creating ConfigMap for ImageSetConfiguration
{: #create-configmap}

Create the `ImageSetConfiguration` YAML file containing the images to be mirrored (see step 3 in Option 1) and create a new ConfigMap from the `ImageSetConfiguration` YAML using the following command. For more details on `ImageSetConfiguration` YAML structure and parameters, see [Defining and configuring your operators for use with oc-mirror](https://developers.redhat.com/learning/learn:openshift:master-operator-mirroring-oc-mirror/resource/resources:defining-and-configuring-your-operators-use-oc-mirror){: external}.

```bash
ibmcloud ce configmap create \
  --name oc-mirror-config \
  --from-file=mirror-imageset.yaml
```
{: codeblock}

### Creating the Code Engine job
{: #create-ce-job}

Create a new job and pass the secret, ConfigMap, and arguments using the following command:

```bash
export REGISTRY=private.de.icr.io
export TARGET_NAMESPACE=ocp-operator-mirror

ibmcloud ce job create \
  --name oc-mirror-job \
  --image $REGISTRY/oc-mirror-tools/oc-mirror-tool:latest \
  --registry-secret icr-secret \
  --cpu 2 \
  --memory 8G \
  --ephemeral-storage 4G \
  --mount-secret /auth=oc-mirror-auth \
  --mount-configmap /workspace=oc-mirror-config \
  --command oc-mirror \
  --argument --config=/workspace/imageset.yaml \
  --argument --workspace=file:///oc-mirror-v2 \
  --argument --authfile=/auth/config.json \
  --argument docker://$REGISTRY/$TARGET_NAMESPACE \
  --argument --v2
```
{: codeblock}

### Setting up persistent storage
{: #setup-persistent-storage}

1. Create a new Cloud Object Storage bucket and HMAC credentials to be used for Code Engine for enabling the persistent storage by following the [documentation](/docs/cloud-object-storage?topic=cloud-object-storage-uhc-hmac-credentials-main){: external}. The bucket contains the results of the mirroring process such as generated CatalogSource files as well as some temporary files created by `oc-mirror` during registry replication.
2. Take a note of the Access Key ID and Secret Access Key from the credentials created in the previous step and create a new HMAC secret in the Code Engine project.
3. Create a new persistent data store in the Persistent data stores section on the Code Engine project page. Use a descriptive name like `oc-mirror-cos-workdir`, select COS instance and bucket used in step 1 and an HMAC secret created in step 2.
4. In the Volume Mounts section in Code Engine job created, add the persistent data stores and the mount path in the COS bucket (`/oc-mirror-v2`). This is the location where `oc-mirror` plugin stores all the cache files and generated YAML files during mirroring.

### Running the job
{: #run-ce-job}

Run the job created using the following command:

```bash
ibmcloud ce jobrun submit --job oc-mirror-job
```
{: codeblock}

The `oc-mirror` plugin automatically creates the index images with the subset of operators in the target registry and CatalogSource to be deployed to the cluster in the `oc-mirror-v2` directory inside the COS bucket.

## Next steps
{: #next-steps}

* Configure [registry mirroring on {{site.data.keyword.openshiftshort}} nodes](/docs/framework-financial-services?topic=framework-financial-services-ocp-image-mgmt-registry-mirroring-config)
* Set up [operator mirroring and catalog sources](/docs/framework-financial-services?topic=framework-financial-services-ocp-image-mgmt-operator-mirroring)
