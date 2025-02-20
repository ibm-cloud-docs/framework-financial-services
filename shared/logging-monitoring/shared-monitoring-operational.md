---

copyright:
  years: 2020, 2025
lastupdated: "2025-02-20"

keywords:

subcollection: framework-financial-services

---

{{site.data.keyword.attribute-definition-list}}



# Operational monitoring
{: #shared-monitoring-operational}

Operational monitoring for gauging system health is a important complement to [monitoring for security and compliance](/docs/framework-financial-services?topic=framework-financial-services-shared-monitoring-compliance). Proper operational monitoring can help you determine whether you need to fail over to an alternative storage or processing site. In addition, operational monitoring can help you determine whether operations have returned to normal after a system disruption. Operational metrics include measurements for CPU usage, memory usage, or API response times.
{: shortdesc}

Since {{site.data.keyword.mon_full_notm}} is not Financial Services Validated, the next two sections describe "bring your own" software solutions for {{site.data.keyword.openshiftshort}} and {{site.data.keyword.vsi_is_short}}.

{{site.data.content.fs-validated-disclaimer-important}}

## {{site.data.keyword.openshiftshort}}
{: #openshift}

You need to install your own software solution for monitoring in {{site.data.keyword.openshiftshort}} within your own VPC. There are various ways an operational monitoring solution can be implemented. See [Setting up an operational monitoring solution](/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-monitoring-operational-tutorial) for one example that uses {{site.data.keyword.openshiftshort}} [Prometheus and Grafana](https://docs.openshift.com/container-platform/4.6/monitoring/monitoring-overview.html){: external}.

## {{site.data.keyword.vsi_is_short}}
{: #virtual-servers}

You need to install your own software solution for monitoring in virtual server instances. See [Setting up an operational monitoring solution](/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-monitoring-operational-tutorial) for one example of sending virtual server instance metrics with [Prometheus Node Exporter](https://prometheus.io/docs/guides/node-exporter/){: external} to {{site.data.keyword.openshiftshort}} [Prometheus and Grafana](https://docs.openshift.com/container-platform/4.6/monitoring/monitoring-overview.html){: external}.

## Related controls in {{site.data.keyword.framework-fs_notm}}
{: #related-controls}

{{site.data.content.related-controls-disclaimer}}

| Family              | Control                                           |
|---------------------|---------------------------------------------------|
| Contingency Planning (CP) | [CP-2 (3) Contingency Plan &#124; Resume Essential Missions / Business Functions](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-cp-2.3) \n [CP-6 Alternate Storage Site](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-cp-6) \n [CP-7	Alternate Processing Site](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-cp-7) \n [CP-10 Information System Recovery and Reconstitution](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-cp-10)  |
{: caption="Related controls in {{site.data.keyword.framework-fs_notm}} [FSv2.0]" caption-side="top"}
{: #related-controls-fsv2.0}
{: tab-title="FSv2.0"}
{: tab-group="RelatedControls-1"}
{: class="simple-tab-table"}


| Family              | Control                                           |
|---------------------|---------------------------------------------------|
| Contingency Planning (CP) | [CP-2 (3) Contingency Plan &#124; Resume Essential Missions / Business Functions](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-cp-2.3) \n [CP-6 Alternate Storage Site](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-cp-6) \n [CP-7	Alternate Processing Site](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-cp-7) \n [CP-10 Information System Recovery and Reconstitution](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-cp-10)  |
{: caption="Related controls in {{site.data.keyword.framework-fs_notm}} [FSv1.1]" caption-side="top"}
{: #related-controls-fsv1.1}
{: tab-title="FSv1.1"}
{: tab-group="RelatedControls-1"}
{: class="simple-tab-table"}


## Next steps
{: #next-steps}

* [Data encryption at rest](/docs/framework-financial-services?topic=framework-financial-services-shared-encryption-at-rest)
