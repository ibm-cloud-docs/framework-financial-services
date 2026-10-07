---

copyright:
  years: 2021, 2026
lastupdated: "2026-10-06"

keywords:

subcollection: framework-financial-services

---

{{site.data.keyword.attribute-definition-list}}

# Release notes
{: #release-notes}

Use the release notes to learn about the latest changes to the documentation for the {{site.data.keyword.cloud_notm}} for Financial Services®.
{: shortdesc}

Looking for {{site.data.keyword.cloud_notm}} status, platform release notes, security bulletins, or maintenance notifications? See [{{site.data.keyword.cloud_notm}} status](https://cloud.ibm.com/status?selected=status){: external}.
{: note}

## 6 October 2026
{: #06-october-2026}

* Updated [FQDN based egress proxy in IBM Cloud VPC](/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-egress-proxy) guidance with recommended implementation using ALB with FQDN backends.

## 29 September 2026
{: #29-september-2026}

* Updated [VPC reference architecture overview](/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-about) with Edge VPC clarifications.

## 23 June 2026
{: #23-june-2026}

* Added guidance for [Cloud Logs best practices](/docs/framework-financial-services?topic=framework-financial-services-shared-logging-best-practices)

  Learn to use {{site.data.keyword.logs_full}} features while maintaining Financial Services compliance and reducing costs.

## 17 June 2026
{: #17-june-2026}

* Added guidance for [data loss prevention and perimeter acccess patterns](/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-connectivity-dlp)

  Learn about the data loss prevention strategies with a focus on perimeter access patterns for Independent Software Vendor (ISV) deployments in {{site.data.keyword.cloud_notm}} for Financial Services, including security enforcements, architectural decisions, and threat mitigations.

## 15 June 2026
{: #15-june-2026}

* Added guidance for [Direct Link for on prem connectivity](/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-connectivity-direct-link)

  {{site.data.keyword.dl_short}} can provide a secure and dedicated netowork connection between on-prem environment ("Bank") and ISV Cloud account for the ISV to access on-prem services (Bank information systems).

* Added link to [multi-tenant solution guidance](/docs/framework-financial-services?topic=framework-financial-services-shared-multitenancy-summary)

  The guide provides different deployment models focused mostly on multitenancy for container-based solutions.

## 10 June 2026
{: #10-june-2026}

* Added guidance for [egress proxy](/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-egress-proxy)

  FS controls require SG and ACL to not have 0.0.0.0/0 for egress rules. In high security environments, having an egress for 0.0.0.0/0 is not acceptable. The solution architecture must provide a way to access public services on the internet which do not publish IP Address from isolated secured environments on cloud and still be able to meet FS controls with minimal exceptions.

## 2 June 2026
{: #02-june-2026}

* Added guidance for [container image management](/docs/framework-financial-services?topic=framework-financial-services-ocp-image-mgmt-overview)

  This pattern presents a set of architectural alternatives for controlling access to container images used in workload deployments on {{site.data.keyword.openshiftshort}} clusters. The result is an approach that enables secure workload deployment with required controls over software supply chain elements.

## 24 March 2025
{: #24-march-2025}

* Updated IBM Cloud Framework with changes for FS 2.0.

## 30 April 2023
{: #30-april-2023}

* Updated [Infrastructure as Code (IaC) article](/docs/framework-financial-services?topic=framework-financial-services-shared-deploy-infrastructure-as-code) to point at new deployable architectures.

## 24 April 2023
{: #24-april-2023}

* Removed recommendation that limited deployments to a subset of available multizone regions.

## 31 March 2023
{: #31-march-2023}

* Integrated references to supplemental guidance for [enterprise account architecture](/docs/enterprise-account-architecture?topic=enterprise-account-architecture-about) for large enterprises.

## 03 March 2023
{: #03-march-2023}

* Updated list of multizone regions which are {{site.data.keyword.cloud_notm}} for Financial Services Validated to include Toronto (`ca-tor`) and Sydney (`au-syd`).

## 23 January 2023
{: #23-january-2023}

* Updated list of services which are designated as {{site.data.keyword.cloud_notm}} for Financial Services Validated to include {{site.data.keyword.compliance_long}}, {{site.data.keyword.contdelivery_full}}, {{site.data.keyword.secrets-manager_full}}, and {{site.data.keyword.cloud}} Client {{site.data.keyword.vpn_vpc_short}}.

## 30 September 2022
{: #30-september-2022}

* Added contextual links into [{{site.data.keyword.framework-fs_full}} - Control Requirements](/docs/framework-financial-services-controls) to make it easier to understand how the control requirements related to implementation guidance.

## 24 August 2022
{: #24-august-2022}

* Fixed a couple of links and addressed some formatting issues.



## 28 July 2022
{: #28-july-2022}

* Added extensive content for the new [{{site.data.keyword.satellitelong}} reference architecture](/docs/framework-financial-services?topic=framework-financial-services-satellite-architecture-about) as well as deployment and configuration guidance for that architecture. During this process, there were numerous organizational changes that were made due to the overlap between the {{site.data.keyword.satellitelong}} reference architecture and the {{site.data.keyword.vpc_short}} reference architecture.
* Enabled link to spreadsheet of [control requirements](/docs/framework-financial-services?topic=framework-financial-services-about#framework-control-requirements) in {{site.data.keyword.framework-fs_notm}}.
* Added article [Deploy infrastructure as code for reference architectures](/docs/framework-financial-services?topic=framework-financial-services-shared-deploy-infrastructure-as-code) about Terraform automation for {{site.data.keyword.cloud_notm}} for Financial Services.
* Enhanced content on {{site.data.keyword.compliance_full}} and {{site.data.keyword.cloud_notm}} for Financial Services profile. See [Compliance monitoring](/docs/framework-financial-services?topic=framework-financial-services-shared-monitoring-compliance).
* Added [site map](/docs/framework-financial-services?topic=framework-financial-services-sitemap).
* Made many editorial updates.

## 17 March 2022
{: #17-march-2022}

* Clarify guidance around using {{site.data.keyword.cloud_notm}} Financial Services Validated services depending on whether you are a financial institution or technology vendor. Add guidance about using {{site.data.keyword.atracker_full_notm}} in regions where event routing is not available.

## 03 February 2022
{: #03-february-2022}

* Added information on an approved variation of the VPC reference architecture to allow public internet access to the workload VPC. Added related tutorials on setting up a virtual network firewall in a new edge/transit VPC.

## 14 October 2021
{: #14-october-2021}

* Revamped [Best practices for software as a service](/docs/framework-financial-services?topic=framework-financial-services-best-practices). Added clarifications and detail. Also, added some new best practices derived from the Control Implementation Overview Template documents.

## 07 April 2021
{: #07-april-2021}

* Initial release.
