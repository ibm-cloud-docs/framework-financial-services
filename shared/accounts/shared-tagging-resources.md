---

copyright:
  years: 2020, 2025
lastupdated: "2025-03-02"

keywords:

subcollection: framework-financial-services

---

{{site.data.keyword.attribute-definition-list}}

# Tagging {{site.data.keyword.cloud_notm}} resources and managing access with tags
{: #shared-tagging-resources}

All {{site.data.keyword.cloud_notm}} resources in your account should be tagged with security attributes that you define. Apply user tags to organize your resources and easily find them later, to help you with identifying specific team usage or cost allocation, and to control access to your resources without requiring updates to your IAM policies.
{: shortdesc}

For more information, see:

* [Working with tags](/docs/account?topic=account-tag) (through the Global Search and Tagging platform service)
* [Controlling access to resources by using tags](/docs/account?topic=account-access-tags-tutorial)

## Example tagging scheme
{: #shared-tagging-resources-example-scheme}

The {{site.data.keyword.framework-fs_notm}} doesn't require a specific set of tags that must be used, but they should be meaningful for your application. For illustration, you might use a scheme such as the one outlined.

In this example, it is suggested that you use one the following impact levels from [Federal Information Processing Standards Publication 199](https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.199.pdf){: external}:

* `fips199-high`: The loss of confidentiality, integrity, or availability could be expected to have a severe or catastrophic adverse effect on organizational operations, organizational assets, or individuals.
* `fips199-moderate`: The loss of confidentiality, integrity, or availability could be expected to have a serious adverse effect on organizational operations, organizational assets, or individuals.
* `fips199-low`: The loss of confidentiality, integrity, or availability could be expected to have a limited adverse effect on organizational operations, organizational assets, or individuals.

In addition, for services that store data, it is suggested that you use one or more of these tags corresponding to different kinds of data:

* `audit`: Audit logs or archives
* `public`: Publicly accessible, such as Cloud Object Storage buckets that hold public documentation or samples
* `consumer-owned-data`: Data owned by the consumer
* `consumer-metadata`: Metadata owned by the consumer
* `regulated-data`: Data that is regulated

## Related controls in {{site.data.keyword.framework-fs_notm}}
{: #shared-tagging-resources-related-controls}

{{site.data.content.related-controls-disclaimer}}

| Family              | Control                                           |
|---------------------|---------------------------------------------------|
| Access Control (AC) | [AC-16 Security and Privacy Attributes](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-ac-16) |
| System and Communications Protection (SC) | [SC-16 Transmission of Security and Privacy Attributes](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-16) |
{: caption="Related controls in {{site.data.keyword.framework-fs_notm}} [FSv2.0]" caption-side="top"}
{: #related-controls-fsv2.0}
{: tab-title="FSv2.0"}
{: tab-group="RelatedControls-1"}
{: class="simple-tab-table"}


| Family              | Control                                           |
|---------------------|---------------------------------------------------|
| Access Control (AC) | [AC-16 Security Attributes](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-ac-16) |
| System and Communications Protection (SC) | [SC-16 Transmission of Security Attributes](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-sc-16) |
{: caption="Related controls in {{site.data.keyword.framework-fs_notm}} [FSv1.1]" caption-side="top"}
{: #related-controls-fsv1.1}
{: tab-title="FSv1.1"}
{: tab-group="RelatedControls-1"}
{: class="simple-tab-table"}


## Next steps
{: #shared-tagging-resources-next-steps}

If using the VPC reference architecture, then see:

* [VPC networking overview](/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-connectivity-overview)

If using the {{site.data.keyword.satelliteshort}} reference architecture, then see:

* [{{site.data.keyword.satelliteshort}} networking overview](/docs/framework-financial-services?topic=framework-financial-services-satellite-architecture-connectivity-overview)
