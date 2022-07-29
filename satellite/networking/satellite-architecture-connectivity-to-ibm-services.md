---

copyright:
  years: 2020, 2022
lastupdated: "2022-07-28"

keywords: 

subcollection: framework-financial-services

---

{{site.data.keyword.attribute-definition-list}}

# Connectivity to {{site.data.keyword.cloud_notm}} services with Satellite Link
{: #satellite-architecture-connectivity-to-services}

When using {{site.data.keyword.cloud_notm}} services from workload components in your {{site.data.keyword.satelliteshort}} location, you should use [{{site.data.keyword.satelliteshort}} Link](/docs/satellite?topic=satellite-link-location-cloud) functionality to facilitate secure access to the {{site.data.keyword.cloud_notm}} services. {{site.data.keyword.satelliteshort}} Link endpoints provide a connection over a secure tunnel directly to private service endpoints.
{: shortdesc}

Inside a {{site.data.keyword.satelliteshort}} location, this private access can be accomplished by using a [{{site.data.keyword.satelliteshort}} Link endpoint](/docs/satellite?topic=satellite-link-cloud-create#link-cloud) to map a TCP port on the control plane hosts to the {{site.data.keyword.cloud_notm}} service private endpoint. The Link endpoint is a virtualized function that scales horizontally, is redundant and highly available, and spans all zones of your {{site.data.keyword.satelliteshort}} location. 

Private service endpoints for {{site.data.keyword.cloud_notm}} services should be used when you define cloud endpoints for your location.
{: important}

## Creating {{site.data.keyword.satelliteshort}} Link endpoints
{: #create-satellite-link-endpoints}

- Create {{site.data.keyword.satelliteshort}} Link endpoints for the {{site.data.keyword.cloud_notm}} services that you need to consume from your {{site.data.keyword.satelliteshort}} location. For more information, see [Creating and managing link endpoints](/docs/satellite?topic=satellite-link-cloud-create).

Cloud endpoints for your location use a TCP port on the control plane hosts. If the endpoint is supposed to be used by a component outside of your {{site.data.keyword.openshiftshort}} cluster deployed on your {{site.data.keyword.satelliteshort}} location, you need to allow network connectivity from the origin of that component to the port on the control plane.
{: important}

## Related controls in {{site.data.keyword.framework-fs_notm}} 
{: #related-controls}

The following {{site.data.keyword.framework-fs_notm}} controls are most related to this guidance. However, in addition to following the guidance here, do your own due diligence to ensure you meet the requirements.


| Family              | Control                                           |
|---------------------|---------------------------------------------------|
| Access Control (AC) | AC-20 Use of External Information Systems |
| Security Assessment and Authorization (CA) | CA-3 System Interconnections |
| System and Communications Protection (SC)  | SC-5 Denial of Service Protection \n SC-7 Boundary Protection \n SC-7(4) Boundary Protection &#124; External Telecommunications Services \n SC-7 (5) Boundary Protection &#124; Deny By Default - Allow By Exception \n SC-7 (10) Boundary Protection &#124; Prevent Exfiltration |
{: caption="Table 1. Related controls in {{site.data.keyword.framework-fs_notm}}" caption-side="top"}

## Next steps
{: #next-steps}

- [Consumer connectivity to workload resources](/docs/framework-financial-services?topic=framework-financial-services-satellite-architecture-connectivity-workload)
