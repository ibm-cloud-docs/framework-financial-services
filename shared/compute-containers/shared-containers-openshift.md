---

copyright:
  years: 2020, 2022
lastupdated: "2022-07-28"

keywords: 

subcollection: framework-financial-services

---

{{site.data.keyword.attribute-definition-list}}

# Working with {{site.data.keyword.openshiftlong_notm}}
{: #shared-containers-openshift}

If you want to use containers in either either the VPC or {{site.data.keyword.satelliteshort}} reference architectures, you should use [{{site.data.keyword.openshiftlong_notm}}](/docs/openshift?topic=openshift-roks-overview). {{site.data.keyword.openshiftshort}} is a managed offering to create your own {{site.data.keyword.openshiftshort}} cluster of compute hosts to deploy and manage containerized apps on {{site.data.keyword.cloud_notm}}. {{site.data.keyword.openshiftshort}} provides intelligent scheduling, self-healing, horizontal scaling, service discovery and load balancing, automated rollouts and rollbacks, and secret and configuration management for your apps. Combined with an intuitive user experience, built-in security and isolation, and advanced tools to secure, manage, and monitor your cluster workloads, you can rapidly deliver highly available and secure containerized apps in the public cloud.
{: shortdesc}



## Deploying {{site.data.keyword.openshiftshort}}
{: #deployment}

1. Install the CLI plugins for {{site.data.keyword.openshiftshort}}. For more information, see [Installing the {{site.data.keyword.openshiftshort}} CLI]().

2. Setup the API for {{site.data.keyword.openshiftshort}}. For more information, see [Setting up the API](/docs/openshift?topic=openshift-cs_api_install).

3. Create your {{site.data.keyword.openshiftshort}} cluster. For more information, see [Creating a {{site.data.keyword.openshiftshort}} cluster in your VPC](/docs/openshift?topic=openshift-vpc_rh_tutorial).

4. Install {{site.data.keyword.redhat_notm}} {{site.data.keyword.openshiftshort}} Service Mesh by using the [{{site.data.keyword.redhat_notm}} service mesh operator](https://docs.openshift.com/container-platform/4.5/service_mesh/v1x/servicemesh-release-notes.html){: external}. For more information, see [Installing {{site.data.keyword.redhat_notm}} {{site.data.keyword.openshiftshort}} Service Mesh](https://docs.openshift.com/container-platform/4.5/service_mesh/v1x/installing-ossm.html){: external}.

   Based on the open source [Istio](https://istio.io/){: external} project, {{site.data.keyword.redhat_notm}} {{site.data.keyword.openshiftshort}} Service Mesh adds a transparent layer on existing distributed applications without requiring any changes to the service code. You add {{site.data.keyword.redhat_notm}} {{site.data.keyword.openshiftshort}} Service Mesh support to services by deploying a special sidecar proxy to relevant services in the mesh that intercepts all network communication between microservices. You configure and manage the Service Mesh using the control plane features. For more information, see[Understanding {{site.data.keyword.redhat_notm}} {{site.data.keyword.openshiftshort}} Service Mesh](https://docs.openshift.com/container-platform/4.5/service_mesh/v1x/ossm-architecture.html){: external}.

   Two of the most important reasons for using Service mesh is to enable you to:

   * Encrypt network traffic between microservices running in your cluster. See [Data encryption in transit](/docs/allowlist/framework-financial-services?topic=framework-financial-services-shared-encryption-in-transit) for more details.
   * Implement gateways to manage inbound and outbound traffic for your mesh to specify which traffic you want to enter or leave the mesh. For example, you can use an egress gateways to control/allowlist all necessary endpoints and domains that your application needs to connect to. You should deny all traffic by default. See [Traffic Management](https://docs.openshift.com/container-platform/4.5/service_mesh/v1x/ossm-traffic-manage.html#ossm-routing-gw_routing-traffic-v1x) for more details.

   The default {{site.data.keyword.cloud_notm}} configuration of the routers enables host networking, which is not compatible with the service mesh network policy. For the service mesh ingress to work, [apply a network policy](https://gist.githubusercontent.com/kitch/39c504a2ed9e381c2aadea436d5b52e4/raw/d8efa69f41d41425b16bb363a881a98d40d3708c/mesh-policy.yaml){: external}.
   {: tip}

5. Set up {{site.data.keyword.registryshort}} and Vulnerability Advisor. For more information, see [Container Registry and Vulnerability Advisor](/docs/allowlist/framework-financial-services?topic=framework-financial-services-shared-development-processes#vpc-architecture-development-processes-registry-vulnerability-advisor).

6. Develop and deploy applications to your cluster. See the following for more details:
   * [Planning app deployments](/docs/openshift?topic=openshift-plan_deploy) for more details.
   * [Deploying applications on {{site.data.keyword.redhat_notm}} {{site.data.keyword.openshiftshort}} Service Mesh](https://docs.openshift.com/container-platform/4.5/service_mesh/v1x/prepare-to-deploy-applications-ossm.html){: external}

## Related resources
{: #related-resources}



* [Security for {{site.data.keyword.openshiftshort}}](/docs/openshift?topic=openshift-security)
* [Scalable web application on {{site.data.keyword.openshiftshort}}](/docs/solution-tutorials?topic=solution-tutorials-scalable-webapp-openshift)
* [Deploy microservices with {{site.data.keyword.openshiftshort}}](/docs/solution-tutorials?topic=solution-tutorials-openshift-microservices)

## Related controls in {{site.data.keyword.framework-fs_notm}} 
{: #related-controls}

{{site.data.content.related-controls-disclaimer}}

| Family | Control |
|--------|---------|
| Identification and Authentication (IA) | IA-2 (1) Identification and Authentication (Organizational Users) &#124; Network Access to Privileged Accounts |
| System and Communications Protection (SC) | SC-7 (5) Boundary Protection &#124; Deny by Default / Allow by Exception \n SC-8	Transmission Confidentiality and Integrity |
{: caption="Table 1. Related controls in {{site.data.keyword.framework-fs_notm}}" caption-side="top"}






## Next steps
{: #next-steps}

If using the VPC reference architecture, see [Storage for VPC reference architecture](/docs/allowlist/framework-financial-services?topic=framework-financial-services-vpc-architecture-storage).

If using the {{site.data.keyword.satelliteshort}} reference architecture, see [Storage for {{site.data.keyword.satelliteshort}} reference architecture](/docs/allowlist/framework-financial-services?topic=framework-financial-services-satellite-architecture-storage).
