---
copyright:
  years: 2026
lastupdated: "2026-06-03"

keywords: operators, catalog source, operator mirroring, openshift operators

subcollection: framework-financial-services

---

{{site.data.keyword.attribute-definition-list}}

# Configuring operator mirroring and catalog sources
{: #ocp-image-mgmt-operator-mirroring}

Since operators are deployed via images, the solution to deploy operators originating from external registries involves the same approach as image mirroring with a few additions.
{: shortdesc}

## Understanding operator mirroring
{: #understanding-operator-mirroring}

Without a direct access to external catalogs, catalog index images are re-created using `oc-mirror` command line tool. Configuring CatalogSource resources with the repackaged index images will make the selected operators visible in the {{site.data.keyword.openshiftshort}} marketplace view and enable their installation.

To facilitate the proper operator images deployment on an isolated cluster, you need to have the registry configuration and node pull secret credentials update as described in [Configuring registry mirroring on {{site.data.keyword.openshiftshort}} nodes](/docs/framework-financial-services?topic=framework-financial-services-ocp-image-mgmt-registry-mirroring-config).

## Creating CatalogSource resources
{: #create-catalogsource}

The `oc-mirror` plugin automatically creates the YAML file for deploying the CatalogSource in the directory `/oc-mirror-v2/working-dir/cluster-resources`. The following example shows a sample CatalogSource YAML created by `oc-mirror`:

```yaml
apiVersion: operators.coreos.com/v1alpha1
kind: CatalogSource
metadata:
  annotations:
    createdAt: Friday, 30-Jan-26 06:15:05 UTC
    createdBy: oc-mirror v2
    oc-mirror_version: 4.20.0-202512150315.p2.gf4775a2.assembly.stream.el9-f4775a2
  name: cs-redhat-operator-index-v4-20
  namespace: openshift-marketplace
spec:
  image: private.de.icr.io/mirror-registry/redhat/redhat-operator-index:v4.20
  sourceType: grpc
status: {}
```
{: codeblock}

## Applying the CatalogSource
{: #apply-catalogsource}

This YAML can be applied to the cluster using the following command, which would list the mirrored operators in the OperatorHub:

```bash
oc apply -f <file-name>
```
{: codeblock}

Operators can then be installed following the normal operator installation process.

## Verifying operator availability
{: #verify-operators}

After applying the CatalogSource, you can verify that the operators are available in the {{site.data.keyword.openshiftshort}} console:

1. Navigate to **Operators** > **OperatorHub** in the {{site.data.keyword.openshiftshort}} console.
2. Search for the operators you mirrored.
3. Verify that the operators appear in the list and can be installed.

## Next steps
{: #next-steps}

* Configure [image acceptance policies](/docs/framework-financial-services?topic=framework-financial-services-ocp-image-mgmt-image-acceptance-policies)
* Set up [vulnerability scanning](/docs/framework-financial-services?topic=framework-financial-services-ocp-image-mgmt-vulnerability-scanning)
