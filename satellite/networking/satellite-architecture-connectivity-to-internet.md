---

copyright:
  years: 2020, 2025
lastupdated: "2025-02-20"

keywords:

subcollection: framework-financial-services

---

{{site.data.keyword.attribute-definition-list}}

# Accessing external resources from the {{site.data.keyword.satelliteshort}} location
{: #satellite-architecture-connectivity-to-external}

In general, the {{site.data.keyword.framework-fs_notm}} does not recommend connecting to hosts on the public internet nor accepting connections from the public internet. When it is necessary to do so, you need to use proper facilities, such as load balancers, public gateways, or proxy servers, that usually would be deployed outside of the {{site.data.keyword.satelliteshort}} location network.

However, deployment of {{site.data.keyword.satelliteshort}} hosts requires connection to certain external resources to enable the attachment and assignment processes for the hosts as well as ongoing operation of {{site.data.keyword.satelliteshort}} control plane and workload clusters. These connections need to be properly configured, monitored, and audited.
{: shortdesc}






## External resources for control plane hosts
{: #control-plane}

Control plane hosts require the following non-HTTP connectivity:

* Red Hat NTP service (unless configured with local or private NTP pool)
* Control plane master connections (TCP ports 30000 - 32767)

The connectivity to the following endpoints must also be allowed, but can be potentially facilitated through an HTTP or HTTPS proxy for flow control and auditing:

* Satellite Link tunnel server endpoint
* {{site.data.keyword.cloud_notm}} general APIs and Container services
* IAM REST APIs
* LaunchDarkly service
* {{site.data.keyword.cos_short}} for etcd backup
* Attachment / assignment endpoint
* {{site.data.keyword.satelliteshort}} Config and Link APIs
* RHEL container registry
* {{site.data.keyword.registrylong_notm}}
* {{site.data.keyword.cloud_notm}} monitoring and log analysis

For more information, see [host networking requirements](/docs/satellite?topic=satellite-reqs-host-network#reqs-host-network-firewall-outbound).



## External resources for workload hosts
{: #workload}

Workload hosts require the following non-HTTP connectivity:

* Red Hat NTP service (unless configured with local or private NTP pool)

The connectivity to the following endpoints must also be allowed, but can be potentially facilitated through an HTTP or HTTPS proxy for flow control and auditing:

* {{site.data.keyword.cloud_notm}} general APIs and Container services
* IAM REST APIs
* LaunchDarkly service
* Attachment / assignment endpoint
* {{site.data.keyword.satelliteshort}} Config and Link APIs
* {{site.data.keyword.registrylong_notm}} 
* RHEL container registry
* {{site.data.keyword.cloud_notm}} monitoring and log analysis

For more information, see more [host networking requirements](/docs/satellite?topic=satellite-reqs-host-network#reqs-host-network-firewall-outbound)



## Related controls in {{site.data.keyword.framework-fs_notm}}
{: #related-controls}

The following {{site.data.keyword.framework-fs_notm}} controls are most related to this guidance. However, in addition to following the guidance here, do your own due diligence to ensure you meet the requirements.


| Family              | Control                                           |
|---------------------|---------------------------------------------------|
| Access Control (AC) | [AC-20 Use of External Information Systems](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-ac-20) |
| Security Assessment and Authorization (CA) | [CA-3 System Interconnections](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-ca-3) |
| System and Communications Protection (SC)  | [SC-5 Denial of Service Protection](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-5)   \n [SC-7 Boundary Protection](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-7)\n [SC-7(4) Boundary Protection &#124; External Telecommunications Services](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-7.4)\n [SC-7 (5) Boundary Protection &#124; Deny By Default - Allow By Exception](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-7.5)\n [SC-7 (10) Boundary Protection &#124; Prevent Exfiltration](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-7.10) |
{: caption="Related controls in {{site.data.keyword.framework-fs_notm}} [FSv2.0]" caption-side="top"}
{: #related-controls-fsv2.0}
{: tab-title="FSv2.0"}
{: tab-group="RelatedControls-1"}
{: class="simple-tab-table"}


| Family              | Control                                           |
|---------------------|---------------------------------------------------|
| Access Control (AC) | [AC-20 Use of External Information Systems](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-ac-20) |
| Security Assessment and Authorization (CA) | [CA-3 System Interconnections](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-ca-3) |
| System and Communications Protection (SC)  | [SC-5 Denial of Service Protection](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-sc-5)   \n [SC-7 Boundary Protection](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-sc-7)\n [SC-7(4) Boundary Protection &#124; External Telecommunications Services](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-sc-7.4)\n [SC-7 (5) Boundary Protection &#124; Deny By Default - Allow By Exception](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-sc-7.5)\n [SC-7 (10) Boundary Protection &#124; Prevent Exfiltration](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-sc-7.10) |
{: caption="Related controls in {{site.data.keyword.framework-fs_notm}} [FSv1.1]" caption-side="top"}
{: #related-controls-fsv1.1}
{: tab-title="FSv1.1"}
{: tab-group="RelatedControls-1"}
{: class="simple-tab-table"}


## Next steps
{: #next-steps}

* [Completing operator actions through a bastion host](/docs/framework-financial-services?topic=framework-financial-services-satellite-architecture-connectivity-bastion)
