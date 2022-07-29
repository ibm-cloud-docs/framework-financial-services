---

copyright:
  years: 2020, 2022
lastupdated: "2022-04-12"

keywords: 

subcollection: framework-financial-services

---

{{site.data.keyword.attribute-definition-list}}

# Operational logging
{: #shared-logging-operational}

Operational logs are an important complement to audit logs for ensuring your application runs smoothly. Proper operational logging can help you determine whether you need to fail over to an alternative storage or processing site. In addition, operational logging can help you determine whether operations have returned to normal after a system disruption.
{: shortdesc}

Audit logs contain [auditable events](/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-logging-audit-provider#vpc-architecture-logging-audit-provider-events), while operational logs contain everything else that might be logged. For example, an operational log might contain entries that reflect what's happening as a program executes, such as functions getting called or stack traces getting thrown. While this kind of log data is key to keeping a system running smoothly, it's typically not sent to a SIEM.

Operational logs can be split into two categories:

* Application - Log data generated by software components that you deploy and manage within your deployment. This might be from your own code, or other software components like databases or message queues.
* Platform - Log data from {{site.data.keyword.cloud_notm}} service instances that is not contained in the [audit the data sent to {{site.data.keyword.atracker_short}}](/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-logging-audit).

## Application logging
{: #application}

You need to install your own software solution for capturing application log data within your VPC. There are various ways an operational logging solution can be implemented. See [Setting up an operational logging solution](/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-logging-operational-tutorial) for one example that uses the {{site.data.keyword.openshiftshort}} Container Platform Elasticsearch, Fluentd, and Kibana EFK stack.

## Platform logging
{: #platform}

### VPC network traffic
{: #solutions-network-flow-logs}

If you are using virtual server instances, you are required to use [Flow Logs for VPC](/docs/vpc?topic=vpc-flow-logs) to enable the collection, storage, and presentation of information about the Internet Protocol (IP) traffic going to and from network interfaces on virtual server instances within your VPC.

Flow logs can help with a number of tasks, including:

* Troubleshooting why specific traffic isn't reaching an instance, which helps to diagnose restrictive security group rules
* Recording the metadata network traffic that is reaching your instance
* Determining source and destination traffic from the network interfaces
* Adhering to compliance regulations
* Assisting with root cause analysis

See [Creating a flow log collector](/docs/vpc?topic=vpc-ordering-flow-log-collector) for more details. You must configure flow logs to go to a properly secured {{site.data.keyword.cos_full_notm}} bucket, [encrypted with KYOK](/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-encryption-at-rest).

### {{site.data.keyword.openshiftshort}} and virtual server instances
{: #solutions-other}

Platform logs generated from {{site.data.keyword.openshiftshort}} and virtual server instances can be collected by using your own software solution or by using the previously mentioned solution for [Setting up an operational logging solution](/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-logging-operational-tutorial).

### Other {{site.data.keyword.cloud_notm}} services
{: #solutions-other}

There is not a Financial Services Validated solution for capturing operational platform logs from other {{site.data.keyword.cloud_notm}} services. These are usually collected by [sending them to {{site.data.keyword.loganalysislong_notm}}](/docs/log-analysis?topic=log-analysis-cloud_services). However, if seeking the Financial Services Validated designation, you should _not_ send platform logs to {{site.data.keyword.loganalysislong_notm}}.
{: important}

## Related controls in {{site.data.keyword.framework-fs_notm}} 
{: #related-controls}

{{site.data.content.related-controls-disclaimer}}

| Family              | Control                                           |
|---------------------|---------------------------------------------------|
| Contingency Planning (CP) | CP-2 (3) Contingency Plan &#124; Resume Essential Missions / Business Functions \n CP-6 Alternate Storage Site \n CP-7	Alternate Processing Site \n CP-10 Information System Recovery and Reconstitution  |
| System and Information Integrity (SI) | SI-11 Error Handling    |
{: caption="Table 1. Related controls in {{site.data.keyword.framework-fs_notm}}" caption-side="top"}

## Next steps
{: #next-steps}

* [Compliance monitoring](/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-monitoring-compliance)