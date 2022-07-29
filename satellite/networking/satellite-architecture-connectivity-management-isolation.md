---

copyright:
  years: 2020, 2022
lastupdated: "2022-07-28"

keywords: 

subcollection: framework-financial-services

---

{{site.data.keyword.attribute-definition-list}}

# Ensuring isolation between {{site.data.keyword.satelliteshort}} management functions and workload functions
{: #satellite-architecture-connectivity-management-isolation}

A key aspect of the {{site.data.keyword.framework-fs_notm}} is to separate user workloads from system management functions and isolate security functions from nonsecurity functions. The network infrastructure of the {{site.data.keyword.satelliteshort}} location can be used to provide physical and logical separation between the {{site.data.keyword.satelliteshort}} management control plane and your workloads. 
{: shortdesc}

Network flow rule design should follow the {{site.data.keyword.framework-fs_notm}}'s [information flow guidelines](/docs/framework-financial-services?topic=framework-financial-services-best-practices#best-practices-boundary-protection) by using a "deny by default" approach.

## Before you begin

1. Complete the work for [account setup and management](/docs/framework-financial-services?topic=framework-financial-services-satellite-architecture-account-setup).
2. Complete [{{site.data.keyword.satelliteshort}} location setup](/docs/satellite?topic=satellite-locations).

## Identify network areas for control plane hosts and workload hosts
{: #management-workload}

1. Place control plane hosts into a separate network segment. Control plane hosts support various management and security-related components of the {{site.data.keyword.satelliteshort}} location. To facilitate effective network flow restrictions within the {{site.data.keyword.satelliteshort}} location, it is recommended to place control plane hosts into a separate network segment (whether physical or virtual) that can enable clear identification of source or destination of the network flows related to control plane functionality. The control plane hosts can be deployed to different physical locations, but the address space they are assigned to should provide an easy way to identify this group of hosts (for example, CIDR blocks).
2. Designate a separate network segment for each group of {{site.data.keyword.satelliteshort}} hosts assigned to {{site.data.keyword.openshiftshort}} workload clusters. Satellite hosts that are assigned to {{site.data.keyword.openshiftshort}} workload clusters (workload hosts) should use their own network segments that would enable network flow control and monitoring between workload hosts, control plane, and other components outside of the {{site.data.keyword.satelliteshort}} location. It is recommended to designate a separate network segment (virtual subnet, VLAN, or a similar entity) for each group of {{site.data.keyword.satelliteshort}} hosts assigned to the same workload cluster.
3. Ensure network flows between network segments that are allocated for control plane hosts and workload hosts are allowed without restrictions. Therefore, in general, the hosts within the control plane or same workload cluster need to have unrestricted network connections to each other, the network flow control is applied to traffic that crosses the boundaries of a particular group of hosts that are assigned to the control plane or same workload cluster. The [host network requirements](/docs/satellite?topic=satellite-reqs-host-network) provide detailed specifications for connectivity requirements between {{site.data.keyword.satelliteshort}} location hosts.
4. Make sure that the selected network infrastructure can provide performance compatible with the [host latency requirements](/docs/satellite?topic=satellite-host-latency-test).

## Design an addressing plan for control and workload segments
{: #addressing-plan}

1. Define network address range (subnet or CIDR block) for control plane hosts.
2. Define network address range (subnet or CIDR block) for the workload hosts of each {{site.data.keyword.openshiftshort}} cluster in the {{site.data.keyword.satelliteshort}} location.

## Configure network flow rules to restrict network traffic for control plane
{: #security-groups-control}

The following rules for control plane hosts must be implemented within the networking infrastructure (virtual or physical) provided by the operator of the {{site.data.keyword.satelliteshort}} location.

1. Allow inbound flows for control plane hosts (API endpoints, TCP 30000 - 32767) from management plane within your environment. For example, to support bastion or CI/CD tools.
2. Allow bidirectional flows between control plane hosts and workload hosts of the same {{site.data.keyword.satelliteshort}} location.
3. Allow outbound flows to workload hosts of the same {{site.data.keyword.satelliteshort}} location.
4. If {{site.data.keyword.satelliteshort}} Link [location endpoint](/docs/satellite?topic=satellite-link-location-cloud#link-location-endpoint) is configured to enable connection from {{site.data.keyword.cloud_notm}} to a resource within the {{site.data.keyword.satelliteshort}} location, allow the outbound flow from control plane hosts to the resource used in the location endpoint for each such endpoint.
5. If {{site.data.keyword.satelliteshort}} Link [cloud endpoints](/docs/satellite?topic=satellite-link-location-cloud#link-cloud-endpoint) are used by components other than workload hosts in the {{site.data.keyword.satelliteshort}} location (for example, to access {{site.data.keyword.cloud_notm}} IAM services from an application deployed outside of the {{site.data.keyword.satelliteshort}} location), allow inbound flow to the control plane hosts from the resource that requires access the cloud endpoint that uses the destination port listed in the endpoint address attribute.
6. Ensure that control plane hosts are configured to allow outbound access to {{site.data.keyword.cloud_notm}} public endpoints. For more information, see [Accessing external resources from the {{site.data.keyword.satelliteshort}} location](/docs/framework-financial-services?topic=framework-financial-services-satellite-architecture-connectivity-to-external).

## Configure network flow rules for workload hosts
{: #security-groups-workload}

The following rules for workload hosts must be implemented within the networking infrastructure (virtual or physical) provided by the operator of the {{site.data.keyword.satelliteshort}} location.

1. Allow inbound network flows to workload hosts from management resources within your environment, such as a bastion host in your management plane. It is recommended to use networking components capable of controlling Level 7 traffic to limit access to services such as {{site.data.keyword.openshiftshort}} web console based on the FQDN of the request. A combination of network rules and an HTTP proxy can be used as well.
2. Allow bidirectional network flows between workload hosts and control plane hosts of the same {{site.data.keyword.satelliteshort}} location.
3. Identify and allow network flows to other application resources within the environment that is required by the deployed workloads. For example, databases that are not deployed on the same workload service.
4. Identify and allow network flows to other services deployed in the {{site.data.keyword.satelliteshort}} location (for example, another {{site.data.keyword.openshiftshort}} cluster) if required by the workload application architecture.
5. Identify and allow the ingress network flows from the edge plane that is used to expose the workload services to application consumers (for example, load balancers).
6. Allow outbound network flows to service endpoints in management plane that is needed for logging and monitoring.
7. Ensure that workload hosts have outbound access to {{site.data.keyword.cloud_notm}} public endpoints. For more information, see [Accessing external resources](/docs/framework-financial-services?topic=framework-financial-services-satellite-architecture-connectivity-to-external).

## (Optional) Configure virtual network flow rules within {{site.data.keyword.openshiftshort}}
{: #security-groups-workload}

1. Configure virtual network flow rules within {{site.data.keyword.openshiftshort}}. In addition to the network flow controls implemented within the IaaS layer of the {{site.data.keyword.satelliteshort}} location, you can use virtual networking features of {{site.data.keyword.openshiftshort}} to control network flows within and into your cluster. You can use [Kubernetes network policies](/docs/openshift?topic=openshift-vpc-kube-policies) or [Calico network policies](https://projectcalico.docs.tigera.io/security/protect-hosts){: external} to define network flow restrictions for each workload deployed on your {{site.data.keyword.openshiftshort}} cluster.

## Related controls in {{site.data.keyword.framework-fs_notm}} 
{: #related-controls}

The following {{site.data.keyword.framework-fs_notm}} controls below are most related to this guidance. However, in addition to following the guidance here, do your own due diligence to ensure you meet the requirements.

| Family              | Control                                           |
|---------------------|---------------------------------------------------|
| Access Control (AC) | AC-20 Use of External Information Systems |
| Security Assessment and Authorization (CA) | CA-3 System Interconnections |
| System and Communications Protection (SC)  | SC-2 Application Partitioning \n SC-3 Security Function Isolation \n SC-5 Denial of Service Protection \n SC-7 Boundary Protection \n SC-7(4) Boundary Protection &#124; External Telecommunications Services \n SC-7 (5) Boundary Protection &#124; Deny By Default - Allow By Exception \n SC-7 (10) Boundary Protection &#124; Prevent Exfiltration \n SC-8 Transmission Confidentiality and Integrity \n SC-8 (1) Transmission Confidentiality and Integrity &#124; Cryptographic Protection \n SC-11 Trusted Path  |
{: caption="Table 1. Related controls in {{site.data.keyword.framework-fs_notm}}" caption-side="top"}

## Next steps
{: #next-steps}

* [Accessing external resources from the {{site.data.keyword.satelliteshort}} location](/docs/framework-financial-services?topic=framework-financial-services-satellite-architecture-connectivity-to-external)
