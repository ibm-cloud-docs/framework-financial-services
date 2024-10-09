---

copyright:
  years: 2020, 2024
lastupdated: "2024-10-09"

keywords: 

subcollection: framework-financial-services

---

{{site.data.keyword.attribute-definition-list}}

# Access management in {{site.data.keyword.cloud_notm}}
{: #shared-account-access-management}



After deciding how to [organize your accounts and resources](/docs/framework-financial-services?topic=framework-financial-services-shared-account-organization), you will need to properly manage access for deployments of the reference architectures that will help achieve [separation of duties and least privilege](/docs/framework-financial-services?topic=framework-financial-services-best-practices#best-practices-least-privilege).
{: shortdesc}

## Managing access with access groups
{: #access-groups}

An [access group](/docs/account?topic=account-groups) is a grouping of user and service IDs to which the same IAM access can be granted. You can assign a single policy to the group instead of assigning the same access multiple times per individual user or service ID. A logical way to assign access is by creating one access group per wanted level of access.

Always use access groups for managing access. IAM policies should never be attached directly to users.
{: important}

For more information:

* [IAM access](/docs/account?topic=account-userroles) for definitions of platform management roles and service access roles.
* [Giving access to resources in resource groups](/docs/account?topic=account-rgs_manage_access)
* [Best practices for assigning access](/docs/account?topic=account-account_setup) for more details about working with IAM.

You must determine what set of access groups works well for your situation. Whatever structure you choose, repeat it for every account (whether a stand-alone account or part of an enterprise).

### Example access group structure
{: #example-structure}

For illustration, the following table shows one possible setup that provides for a reasonable separation of duties.



| Access Group | Description |
| --- | --- |
| cloud-organization-admins | Responsible for organizing the structure of the resources used by the organization. |
| cloud-network-admins | Responsible for creating networks, VPCs, load balancers, subnets, firewall rules, and network devices. |
| cloud-security-admins | Responsible for establishing and managing security policies for the entire organization, including access management and organization constraint policies. |
| cloud-billing-admins | Responsible for setting up billing accounts and monitoring their usage. |
| cloud-devops | DevOps practitioners create or manage end-to-end pipelines that support continuous integration and delivery, monitoring, and system provisioning. |
| cloud-developers | Developers are responsible for designing, coding, and testing applications. |
{: caption="Access groups" caption-side="bottom"}

After the access groups are created, you can assign roles to them.



Never assign roles directly to users. Assign roles to access groups only.
{: tip}

For more information, see:

* [IAM roles and actions](/docs/account?topic=account-iam-service-roles-actions) for a list of applicable roles for every {{site.data.keyword.cloud_notm}} service
* [Access management in {{site.data.keyword.cloud_notm}}](/docs/account?topic=account-cloudaccess)

## Invite users to your account
{: #invite-users}

After you have access groups in place, then you can start inviting users to your account and assigning them to the appropriate access group depending on their job responsibilities. For more information, see [Inviting users to an account](/docs/account?topic=account-iamuserinv).





## Access groups with dynamic rules
{: #dynamic-rules}

If you're using an external identity provider, dynamic rules allow you to automatically add federated users to an access group based on SAML assertions. When a user logs in with a federated ID, the data that is provided by your identity provider dynamically maps the user to an access group based on the rules that you set. This can dramatically ease the administration of your account.

For more information see:

1. [Creating dynamic rules for access groups](/docs/account?topic=account-rules)
2. [Control access to cloud resources](https://developer.ibm.com/tutorials/use-iam-access-groups-to-effectively-manage-access-to-your-cloud-resources/){: external} - See the section "Background information about access groups and dynamic rules"

## Considerations for enterprises
{: #enterprise}

If you're using an enterprise, these resources provide additional information:

* [Setting up an enterprise](/docs/account?topic=account-enterprise-tutorial)

## Required permissions for services in reference architecture
{: #required-permissions}

The following table provides references to additional information for managing access with IAM for each service in the reference architectures.

| Category | VPC reference architecture | {{site.data.keyword.satelliteshort}} reference architecture | Optional for both |
|----------|-------------------|-------------------|-------------------|
| Core  | - [VPC infrastructure services](/docs/vpc?topic=vpc-resource-authorizations-required-for-api-and-cli-calls) [^tabletext] | - [{{site.data.keyword.satelliteshort}}](/docs/satellite?topic=satellite-iam) |  |
| Containers  | - [{{site.data.keyword.openshiftshort}}](/docs/openshift?topic=openshift-access_reference) \n - [{{site.data.keyword.registryshort}}](/docs/Registry?topic=Registry-iam) | - [{{site.data.keyword.openshiftshort}}](/docs/satellite?topic=satellite-iam#iam-roles-clusters) [^tabletext-satellite-enabled-openshift] \n - [{{site.data.keyword.registryshort}}](/docs/Registry?topic=Registry-iam) |  |
| Networking | - [VPC infrastructure services](/docs/vpc?topic=vpc-resource-authorizations-required-for-api-and-cli-calls) \n - [{{site.data.keyword.dl_short}}](/docs/dl?topic=dl-iam) \n - [{{site.data.keyword.tg_short}}](/docs/transit-gateway?topic=transit-gateway-iam)| |  |
| Storage  | - [{{site.data.keyword.block_storage_is_short}}](/docs/vpc?topic=vpc-resource-authorizations-required-for-api-and-cli-calls) \n - [{{site.data.keyword.cos_short}}](/docs/cloud-object-storage?topic=cloud-object-storage-iam) | - [{{site.data.keyword.cos_short}}](/docs/cloud-object-storage?topic=cloud-object-storage-iam) |  |
| Security  | - [{{site.data.keyword.hscrypto}}](/docs/hs-crypto?topic=hs-crypto-manage-access) | - [{{site.data.keyword.hscrypto}}](/docs/hs-crypto?topic=hs-crypto-manage-access)  | - [{{site.data.keyword.appid_short_notm}}](/docs/appid?topic=appid-service-access-management) |
| Logging and monitoring  | - [{{site.data.keyword.atracker_short}}](/docs/activity-tracker?topic=activity-tracker-iam) \n - [{{site.data.keyword.compliance_short}}](/docs/security-compliance?topic=security-compliance-access-management) \n - [{{site.data.keyword.fl_full}}](/docs/vpc?topic=vpc-resource-authorizations-required-for-api-and-cli-calls) | - [{{site.data.keyword.atracker_short}}](/docs/activity-tracker?topic=activity-tracker-iam) \n [{{site.data.keyword.compliance_short}}](/docs/security-compliance?topic=security-compliance-access-management) |  |
| Integration  | | | - [{{site.data.keyword.messagehub}}](/docs/EventStreams?topic=EventStreams-security) |
{: caption="Managing access for {{site.data.keyword.cloud_notm}} services in the reference architectures" caption-side="top"}

[^tabletext]: {{site.data.content.vpc-infrastructure-services-content}}

[^tabletext-satellite-enabled-openshift]: {{site.data.content.satellite-enabled-openshift}}

## Related controls in {{site.data.keyword.framework-fs_notm}} 
{: #related-controls}

{{site.data.content.related-controls-disclaimer}}

| Family              | Control                                           |
|---------------------|---------------------------------------------------|
| Access Control (AC) | [AC-3 Access Enforcement](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-ac-3) \n [AC-5 Separation of Duties](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-ac-5) \n [AC-6 Least Privilege](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-ac-6) |
{: caption="Related controls in {{site.data.keyword.framework-fs_notm}}" caption-side="top"}

## Next steps
{: #next-steps}

* [Handling and securing secrets](/docs/framework-financial-services?topic=framework-financial-services-shared-secrets)
