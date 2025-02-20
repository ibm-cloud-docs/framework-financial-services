---

copyright:
  years: 2020, 2025
lastupdated: "2025-02-20"

keywords:

subcollection: framework-financial-services

---

{{site.data.keyword.attribute-definition-list}}

# Consumer connectivity to workload resources
{: #satellite-architecture-connectivity-workload}

Consumers of the workloads and services that are deployed to your {{site.data.keyword.satelliteshort}} location might need access to one or more of the following resources:

- Applications and microservices that are deployed on your {{site.data.keyword.openshiftshort}} cluster
- {{site.data.keyword.openshiftshort}} Link cloud endpoints providing access to {{site.data.keyword.cloud_notm}} services.



Configure your network infrastructure so that access to workloads and services that are not related to security or management functions is provided through an edge plane that runs outside of your {{site.data.keyword.satelliteshort}} location or some other facility that can control network flows to your {{site.data.keyword.satelliteshort}} hosts. Make sure that services and workloads related to security and management activities can be accessed only from the management plane that runs outside of your {{site.data.keyword.satelliteshort}} location.

## Edge plane with web application firewall
{: #consumer-edge}

The edge plane should be used to enhance boundary protection for the workloads and services that are deployed in the {{site.data.keyword.satelliteshort}} location and the {{site.data.keyword.satelliteshort}} control plane. The workload consumer connectivity to the edge plane is your responsibility and depends on the requirements for the workload consumers and the existing connectivity options for your network infrastructure.

We recommend that you use a Web Application Firewall (WAF) in your edge plane for any service or workload that uses the HTTP protocol (including REST APIs). A WAF filters and monitors consumer traffic and can prevent attacks that exploit the vulnerabilities of web applications and services.
{: important}

## Exposing your {{site.data.keyword.openshiftshort}} workloads
{: #consumer-openshift}

{{site.data.keyword.openshiftshort}} provides several options for enabling external access to applications deployed on a cluster:

- Ingress controller
- Load balancer service
- External IP
- NodePort

For more information, see [Exposing apps in Satellite clusters](/docs/openshift?topic=openshift-sat-expose-apps), [About Ingress](/docs/openshift?topic=openshift-ingress-about-roks4), and [Ingress traffic configuration](https://docs.openshift.com/container-platform/4.10/networking/configuring_ingress_cluster_traffic/overview-traffic.html){: external}.

## Related controls in {{site.data.keyword.framework-fs_notm}}
{: #related-controls}

The following {{site.data.keyword.framework-fs_notm}} controls are most related to this guidance. However, in addition to following the guidance here, do your own due diligence to ensure you meet the requirements.

| Family              | Control                                           |
|---------------------|---------------------------------------------------|
| System and Communications Protection (SC)  | [SC-5 Denial of Service Protection](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-5) \n [SC-7 Boundary Protection](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-7) \n [SC-7(4) Boundary Protection &#124; External Telecommunications Services](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-7.4) \n [SC-7 (5) Boundary Protection &#124; Deny By Default - Allow By Exception](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-7.5) \n [SC-7 (10) Boundary Protection &#124; Prevent Exfiltration](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-7.10) \n [SC-8 Transmission Confidentiality and Integrity](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-8) \n [SC-8 (1) Transmission Confidentiality and Integrity &#124; Cryptographic Protection](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-8.1) \n [SC-11 Trusted Path](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-sc-11)  |
{: caption="Related controls in {{site.data.keyword.framework-fs_notm}} [FSv2.0]" caption-side="top"}
{: #related-controls-fsv2.0}
{: tab-title="FSv2.0"}
{: tab-group="RelatedControls-1"}
{: class="simple-tab-table"}


| Family              | Control                                           |
|---------------------|---------------------------------------------------|
| System and Communications Protection (SC)  | [SC-5 Denial of Service Protection](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-sc-5) \n [SC-7 Boundary Protection](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-sc-7) \n [SC-7(4) Boundary Protection &#124; External Telecommunications Services](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-sc-7.4) \n [SC-7 (5) Boundary Protection &#124; Deny By Default - Allow By Exception](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-sc-7.5) \n [SC-7 (10) Boundary Protection &#124; Prevent Exfiltration](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-sc-7.10) \n [SC-8 Transmission Confidentiality and Integrity](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-sc-8) \n [SC-8 (1) Transmission Confidentiality and Integrity &#124; Cryptographic Protection](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-sc-8.1) \n [SC-11 Trusted Path](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-sc-11)  |
{: caption="Related controls in {{site.data.keyword.framework-fs_notm}} [FSv1.1]" caption-side="top"}
{: #related-controls-fsv1.1}
{: tab-title="FSv1.1"}
{: tab-group="RelatedControls-1"}
{: class="simple-tab-table"}


## Next steps
{: #next-steps}

- [Working with {{site.data.keyword.openshiftlong_notm}}](/docs/framework-financial-services?topic=framework-financial-services-shared-containers-openshift)
