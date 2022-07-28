---

copyright:
  years: 2020, 2022
lastupdated: "2022-07-28"

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
| Access Control (AC) | AC-16 Security Attributes |
| System and Communications Protection (SC) | SC-16 Transmission of Security Attributes |
{: caption="Table 1. Related controls in {{site.data.keyword.framework-fs_notm}}" caption-side="top"}

## Next steps
{: #shared-tagging-resources-next-steps}

If using the VPC reference architecture, then see:

* [VPC networking overview](/docs/allowlist/framework-financial-services?topic=framework-financial-services-vpc-architecture-connectivity-overview)

If using the {{site.data.keyword.satelliteshort}} reference architecture, then see:

* [{{site.data.keyword.satelliteshort}} networking overview](/docs/allowlist/framework-financial-services?topic=framework-financial-services-satellite-architecture-connectivity-overview)
