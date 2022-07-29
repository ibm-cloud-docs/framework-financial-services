---

copyright:
  years: 2020, 2022
lastupdated: "2022-07-28"

keywords: 

subcollection: framework-financial-services

---

{{site.data.keyword.attribute-definition-list}}

# Set up environment for deployment and configuration 
{: #shared-deployment-setup-environment}

Automation is an important part of any cloud solution, and it's even more so in regulated industries. You want to ensure that your deployment, operations, and management procedures are secure and repeatable. Manual activities can be error prone and lead to the introduction of vulnerabilities. So, as you work with the [reference architectures](/docs/framework-financial-services?topic=framework-financial-services-reference-architecture-overview) for the {{site.data.keyword.cloud_notm}} for Financial Services, you will want to take advantage of the tools that {{site.data.keyword.cloud_notm}} provides to make automation possible. These include:
{: shortdesc}

- [{{site.data.keyword.cloud_notm}} Command Line Interface (CLI)](/docs/cli?topic=cli-getting-started)
- [Application programming interfaces (APIs)](/docs?tab=api-docs)
- [Terraform](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-about)

The remainder of this topic describes how to set up these tools for the reference architectures.

Use private routes to {{site.data.keyword.cloud_notm}} service endpoints to enhance control and security over your data when you use the services that make up the reference architectures. Private routes are not accessible or reachable over the internet. By using them, you can protect your data from threats from the public network and logically extend your private network. For more information, see [Secure access to services by using service endpoints](/docs/account?topic=account-service-endpoints-overview) and [Securing your connection when you use the {{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-service-connection).
{: important}

## Set up CLI environment for the reference architectures 
{: #shared-deployment-setup-environment-cli}

The CLI offers a powerful set of commands to work with {{site.data.keyword.cloud_notm}} resources. Most of the {{site.data.keyword.cloud_notm}} services that are part of the reference architectures have [specific plug-ins](/docs/cli?topic=cli-plug-ins) that you can use to extend the base CLI experience. 

The following table provides links for how to set up CLI extensions for each service that has one.

| Category | VPC reference architecture | {{site.data.keyword.satelliteshort}} reference architecture | Optional for both |
|----------|-------------------|-------------------|-------------------|
| Core  | - [VPC infrastructure services](/docs/vpc?topic=vpc-set-up-environment) [^cli-tabletext] | - [{{site.data.keyword.satelliteshort}}](/docs/satellite?topic=satellite-setup-cli) |  |
| Containers  | - [{{site.data.keyword.openshiftshort}}](/docs/openshift?topic=openshift-openshift-cli) \n - [{{site.data.keyword.registryshort}}](/docs/container-registry-cli-plugin?topic=container-registry-cli-plugin-containerregcli) | - [{{site.data.keyword.openshiftshort}}](/docs/openshift?topic=openshift-openshift-cli) [^cli-tabletext-satellite-enabled-openshift] \n - [{{site.data.keyword.registryshort}}](/docs/container-registry-cli-plugin?topic=container-registry-cli-plugin-containerregcli) |  |
| Networking  | - [VPC infrastructure services](/docs/satellite?topic=satellite-setup-cli) \n - [{{site.data.keyword.dl_short}}](/docs/dl?topic=dl-cli-plugin-dl-cli) \n - [{{site.data.keyword.tg_short}}](/docs/tg-cli-plugin?topic=tg-cli-plugin-transit-gateway-cli)  |  |  |
| Storage  | - [{{site.data.keyword.block_storage_is_short}}](/docs/vpc?topic=vpc-set-up-environment) \n - [{{site.data.keyword.cos_short}}](/docs/cloud-object-storage?topic=cloud-object-storage-cli-plugin-ic-cos-cli) | - [{{site.data.keyword.cos_short}}](/docs/cloud-object-storage?topic=cloud-object-storage-cli-plugin-ic-cos-cli) |  |
| Security  | - [{{site.data.keyword.hscrypto}}](/docs/hs-crypto?topic=hs-crypto-cli-plugin-hpcs-cli-plugin)  | - [{{site.data.keyword.hscrypto}}](/docs/hs-crypto?topic=hs-crypto-cli-plugin-hpcs-cli-plugin)  | - {{site.data.keyword.appid_short_notm}} [^cli-tabletext-no-cli-information-app-id] |
| Logging and monitoring  | - {{site.data.keyword.atracker_short}} [^cli-tabletext-no-cli-information-atracker]  \n - {{site.data.keyword.compliance_short}} [^cli-tabletext-no-cli-information-scc] \n - Flow Logs for VPC [^cli-tabletext-no-cli-information-flow-logs]  | - {{site.data.keyword.atracker_short}} \n - {{site.data.keyword.compliance_short}} |  |
| Integration  |  |  | - [{{site.data.keyword.messagehub}}](/docs/EventStreams?topic=EventStreams-cli#cli) |
| Databases  |  |  | - [{{site.data.keyword.ihsdbaas_mongodb_full}}](/docs/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-install-dbaas-cli-plugin) \n - [{{site.data.keyword.ihsdbaas_postgresql_full}}](/docs/hyper-protect-dbaas-for-postgresql?topic=hyper-protect-dbaas-for-postgresql-install-dbaas-cli-plugin) |
{: caption="Table 2. CLI information for services in reference architectures" caption-side="top"}

[^cli-tabletext]: {{site.data.content.vpc-infrastructure-services-content}}

[^cli-tabletext-satellite-enabled-openshift]: {{site.data.content.satellite-enabled-openshift}}

[^cli-tabletext-no-cli-information-app-id]: No specific CLI information is available for {{site.data.keyword.appid_short_notm}}.

[^cli-tabletext-no-cli-information-atracker]: No specific CLI information is available for {{site.data.keyword.atracker_short}} event routing.

[^cli-tabletext-no-cli-information-scc]: No specific CLI information is available for {{site.data.keyword.compliance_short}}.

[^cli-tabletext-no-cli-information-flow-logs]: No specific CLI information is available for Flow Logs.

## Set up API environment for the reference architectures 
{: #shared-deployment-setup-environment-api}

The following table provides links about the APIs that can be used for each service in the reference architectures.

| Category | VPC reference architecture | {{site.data.keyword.satelliteshort}} reference architecture | Optional for both |
|----------|-------------------|-------------------|-------------------|
| Core  | - [VPC infrastructure services](/docs/vpc?topic=vpc-set-up-environment&interface=api) [^api-tabletext] | - [{{site.data.keyword.satelliteshort}}](https://containers.cloud.ibm.com/global/swagger-global-api/#/satellite-cluster){: external} |  |
| Containers  | - [{{site.data.keyword.openshiftshort}}](/docs/openshift?topic=openshift-cs_api_install) \n - [{{site.data.keyword.registryshort}}](/apidocs/container-registry){: external} | - [{{site.data.keyword.openshiftshort}}](/docs/openshift?topic=openshift-cs_api_install) [^api-tabletext-satellite-enabled-openshift] \n - [{{site.data.keyword.registryshort}}](/apidocs/container-registry){: external} |  |
| Networking  | - [VPC infrastructure services](/docs/vpc?topic=vpc-set-up-environment&interface=api) \n - [{{site.data.keyword.dl_short}}](/apidocs/direct_link){: external} \n - [{{site.data.keyword.tg_short}}](/apidocs/transit-gateway){: external}  |  |  |
| Storage  | - [{{site.data.keyword.block_storage_is_short}}](/docs/vpc?topic=vpc-set-up-environment) \n - [{{site.data.keyword.cos_short}}](/docs/cloud-object-storage?topic=cloud-object-storage-compatibility-api) | - [{{site.data.keyword.cos_short}}](/docs/cloud-object-storage?topic=cloud-object-storage-compatibility-api) |  |
| Security  | - [{{site.data.keyword.hscrypto}}](/apidocs/hs-crypto){: external}  | - [{{site.data.keyword.hscrypto}}](/apidocs/hs-crypto){: external}  | - [{{site.data.keyword.appid_short_notm}}](https://us-south.appid.cloud.ibm.com/swagger-ui/#/){: external} |
| Logging and monitoring  | - [{{site.data.keyword.atracker_short}}](/apidocs/atracker/atracker-v2){: external} \n - [{{site.data.keyword.compliance_short}}](/docs/security-compliance?topic=security-compliance-api-setup) \n - [Flow Logs for VPC](/docs/vpc?topic=vpc-set-up-environment&interface=api)  | - [{{site.data.keyword.atracker_short}}](/apidocs/atracker/atracker-v2){: external} \n - [{{site.data.keyword.compliance_short}}](/docs/security-compliance?topic=security-compliance-api-setup) |  |
| Integration  |  |  | - [{{site.data.keyword.messagehub}}](/docs/EventStreams?topic=EventStreams-admin_api) |
| Databases  |  |  | - [{{site.data.keyword.ihsdbaas_mongodb_full}}](/docs/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-api-setup) \n - [{{site.data.keyword.ihsdbaas_postgresql_full}}](/docs/hyper-protect-dbaas-for-postgresql?topic=hyper-protect-dbaas-for-postgresql-api-setup) |
{: caption="Table 3. API information for services in reference architectures" caption-side="top"}

[^api-tabletext]: {{site.data.content.vpc-infrastructure-services-content}}

[^api-tabletext-satellite-enabled-openshift]: {{site.data.content.satellite-enabled-openshift}}


## Set up Terraform environment for the reference architectures 
{: #shared-deployment-setup-environment-terraform}

The following table contains links about the Terraform capabilities for each service in the reference architectures.

| Category | VPC reference architecture | {{site.data.keyword.satelliteshort}} reference architecture | Optional for both |
|----------|-------------------|-------------------|-------------------|
| Core  | - [VPC infrastructure services](/docs/vpc?topic=vpc-terraform-setup)) [^terraform-tabletext] | - [{{site.data.keyword.satelliteshort}}](/docs/openshift?topic=openshift-terraform-setup) |  |
| Containers  | - [{{site.data.keyword.openshiftshort}}](/docs/openshift?topic=openshift-terraform-setup) \n - [{{site.data.keyword.registryshort}}](/docs/Registry?topic=Registry-registry_terraform-setup&interface=ui) | - [{{site.data.keyword.openshiftshort}}](/docs/openshift?topic=openshift-terraform-setup) [^terraform-tabletext-satellite-enabled-openshift] \n - [{{site.data.keyword.registryshort}}](/docs/Registry?topic=Registry-registry_terraform-setup&interface=ui) |  |
| Networking  | - [VPC infrastructure services](/docs/vpc?topic=vpc-terraform-setup) \n - [{{site.data.keyword.dl_short}}](/docs/dl?topic=dl-terraform-setup-dl) \n - [{{site.data.keyword.tg_short}}](/docs/transit-gateway?topic=transit-gateway-terraform-setup-tgw)  |  |  |
| Storage  | - [{{site.data.keyword.block_storage_is_short}}](/docs/vpc?topic=vpc-terraform-setup) \n - [{{site.data.keyword.cos_short}}](/docs/cloud-object-storage?topic=cloud-object-storage-about-terraform) | - [{{site.data.keyword.cos_short}}](/docs/cloud-object-storage?topic=cloud-object-storage-about-terraform) |  |
| Security  | - [{{site.data.keyword.hscrypto}}](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/hpcs){: external}  | - [{{site.data.keyword.hscrypto}}](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/hpcs){: external}  | - [{{site.data.keyword.appid_short_notm}}](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/appid_action_url){: external} |
| Logging and monitoring  | - {{site.data.keyword.atracker_short}} [^terraform-tabletext-no-cli-information-atracker]  \n - [{{site.data.keyword.compliance_short}}](/docs/security-compliance?topic=security-compliance-api-setup) \n - [Flow Logs for VPC](/docs/vpc?topic=vpc-terraform-setup)  | - {{site.data.keyword.atracker_short}} \n - [{{site.data.keyword.compliance_short}}](/docs/security-compliance?topic=security-compliance-terraform-setup) |  |
| Integration  |  |  | - [{{site.data.keyword.messagehub}}](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-provider-template#event-stream-snippet) |
| Databases  |  |  | - [{{site.data.keyword.ihsdbaas_mongodb_full}}](/docs/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-terraform-setup) \n - [{{site.data.keyword.ihsdbaas_postgresql_full}}](/docs/hyper-protect-dbaas-for-postgresql?topic=hyper-protect-dbaas-for-postgresql-terraform-setup) |
{: caption="Table 3. Terraform information for services in reference architectures" caption-side="top"}

[^terraform-tabletext]: {{site.data.content.vpc-infrastructure-services-content}}

[^terraform-tabletext-satellite-enabled-openshift]: {{site.data.content.satellite-enabled-openshift}}

[^terraform-tabletext-no-cli-information-atracker]: No specific Terraform information is available for {{site.data.keyword.atracker_short}} event routing.

## Next steps
{: #next-steps}

* [Deploy infrastructure as code for reference architectures](/docs/framework-financial-services?topic=framework-financial-services-shared-deploy-infrastructure-as-code)
