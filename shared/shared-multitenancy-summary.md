---

copyright:
  years: 2020, 2026
lastupdated: "2026-06-12"

keywords:

subcollection: framework-financial-services

---

{{site.data.keyword.attribute-definition-list}}

# Intra Organization Multitenancy Architecture Reference
{: #shared-multitenancy-summary}

As explained in [Organizing IBM Cloud accounts and resources](/docs/framework-financial-services?topic=framework-financial-services-shared-account-organization), each deployment of one of the reference architectures is single tenant, which means the deployment is intended for a single customer. However, there are some cases where you might need different business units in the same organization to be managed by the same operator.
{: shortdesc}

See [Best practices for multitenant management in IBM Cloud](/docs/pattern-intra-org-multitenancy?topic=pattern-intra-org-multitenancy-overview) to get started.

The guide referenced above provides different deployment models focused mostly on multitenancy for container-based solutions. A combination of these models can be used in a solution deployment, for example, critical and non-critical environments can have different models.

Also consider additional separation for different environment types, such as using different clusters and infrastructure for different environment types when using namespace isolation.


## Next steps
{: #next-steps}

* [Account setup in {{site.data.keyword.cloud_notm}}](/docs/framework-financial-services?topic=framework-financial-services-shared-account-setup)
