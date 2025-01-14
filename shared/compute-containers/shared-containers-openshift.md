---

copyright:
  years: 2020, 2024
lastupdated: "2024-11-19"

keywords:

subcollection: framework-financial-services

---

{{site.data.keyword.attribute-definition-list}}

# Working with {{site.data.keyword.openshiftlong_notm}}
{: #shared-containers-openshift}

If you want to use containers in either either the VPC or {{site.data.keyword.satelliteshort}} reference architectures, you should use [{{site.data.keyword.openshiftlong_notm}}](/docs/openshift?topic=openshift-getting-started). {{site.data.keyword.openshiftshort}} is a managed offering to create your own {{site.data.keyword.openshiftshort}} cluster of compute hosts to deploy and manage containerized apps on {{site.data.keyword.cloud_notm}}. {{site.data.keyword.openshiftshort}} provides intelligent scheduling, self-healing, horizontal scaling, service discovery and load balancing, automated rollouts and rollbacks, and secret and configuration management for your apps. Combined with an intuitive user experience, built-in security and isolation, and advanced tools to secure, manage, and monitor your cluster workloads, you can rapidly deliver highly available and secure containerized apps in the public cloud.
{: shortdesc}



## Deploying {{site.data.keyword.openshiftshort}}
{: #deployment}

1. Install the CLI plugins for {{site.data.keyword.openshiftshort}}. For more information, see [Installing the {{site.data.keyword.openshiftshort}} CLI](/docs/openshift?topic=openshift-kubernetes-service-cli).

2. Setup the API for {{site.data.keyword.openshiftshort}}. For more information, see [Setting up the API](/docs/openshift?topic=openshift-cs_api_install).

3. Create your {{site.data.keyword.openshiftshort}} cluster. For more information, see [Creating a {{site.data.keyword.openshiftshort}} cluster in your VPC](/docs/openshift?topic=openshift-vpc_rh_tutorial).

4. Install [{{site.data.keyword.openshiftshort}} Service Mesh](/docs/solution-tutorials?topic=solution-tutorials-openshift-service-mesh) which is based on the open source [Istio](https://istio.io/){: external} project.

   Two of the most important reasons for using {{site.data.keyword.openshiftshort}} Service Mesh is to enable you to:

   * Encrypt network traffic between microservices running in your cluster. See [Data encryption in transit](/docs/framework-financial-services?topic=framework-financial-services-shared-encryption-in-transit) and [enable mTLS between containers](/docs/solution-tutorials?topic=solution-tutorials-openshift-service-mesh#openshift-service-mesh-secure_services) for more details.
   * Implement gateways to specify which traffic you want to enter or leave the mesh (and deny all traffic by default). You can use an egress gateway to control/allowlist all necessary endpoints and domains that your application needs to connect to. For examples, see [Expose the app with the Istio Ingress Gateway and Route](/docs/solution-tutorials?topic=solution-tutorials-openshift-service-mesh#openshift-service-mesh-ingress_gateway_route) and [Perform traffic management](/docs/solution-tutorials?topic=solution-tutorials-openshift-service-mesh#openshift-service-mesh-traffic_management)

5. Set up {{site.data.keyword.registryshort}} and Vulnerability Advisor. For more information, see [Container Registry and Vulnerability Advisor](/docs/framework-financial-services?topic=framework-financial-services-shared-development-processes#vpc-architecture-development-processes-registry-vulnerability-advisor).

6. Develop and deploy applications to your cluster. See the following for more details:
   * [Planning app deployments](/docs/openshift?topic=openshift-plan_deploy) for more details.
   * [Service Mesh on Red Hat OpenShift on IBM Cloud](/docs/solution-tutorials?topic=solution-tutorials-openshift-service-mesh)
   * [Deploying applications on {{site.data.keyword.openshiftshort}} Service Mesh](https://docs.openshift.com/container-platform/4.5/service_mesh/v1x/prepare-to-deploy-applications-ossm.html){: external}

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
| Identification and Authentication (IA) | [IA-2 (1) Identification and Authentication (Organizational Users) &#124; Network Access to Privileged Accounts](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-ia-2.1) |
| System and Communications Protection (SC) | [SC-7 (5) Boundary Protection &#124; Deny by Default / Allow by Exception](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-7.5) \n [SC-8 Transmission Confidentiality and Integrity](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-8) |
{: caption="Related controls in {{site.data.keyword.framework-fs_notm}}" caption-side="top"}






## Next steps
{: #next-steps}

If using the VPC reference architecture, see [Storage for VPC reference architecture](/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-storage).

If using the {{site.data.keyword.satelliteshort}} reference architecture, see [Storage for {{site.data.keyword.satelliteshort}} reference architecture](/docs/framework-financial-services?topic=framework-financial-services-satellite-architecture-storage).
