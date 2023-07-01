---

copyright:
  years: 2020, 2022
lastupdated: "2022-07-01"

keywords: 

subcollection: framework-financial-services

---

{{site.data.keyword.attribute-definition-list}}

# Audit logging of application provider events and SIEM
{: #shared-logging-audit-provider}

In addition, to [audit logging of {{site.data.keyword.cloud_notm}} events](/docs/framework-financial-services?topic=framework-financial-services-shared-logging-audit), you also need to consider auditing of events your applications might generate. Auditable events from all sources need to be consolidated into a security information and event management (SIEM) system.
{: shortdesc}

## Auditable events
{: #events}

You should audit the following events that occur within the system (in all environments, such as dev, test, prod):

* Access, downloading, revisions to Confidential information
* Access to or update to any financial transaction data
* Any changes to customer accounts
* Login or logoff event, success or failure
* Changes to a system that impact business logic or work flow, examples include but are not limited to, changing call routing, interest rates that are offered for a class of customer, a website flow a customer uses
* Any changes to customer systems that include but not limited to: software install or uninstall, configuration changes, directory or file moves, adds, or deletes, configuration changes, port turn ups, MAC’s, routing changes, and so on.
* Each command action taken, both successful and unsuccessful, by a privileged account that includes but not limited to:
    * All changes, additions, or deletions to any account such as lock, unlock, password reset, permission changes,
    * Modification to system time / time-synchronization configuration,
    * Log files: access to logs files; initialization, stopping or pausing of logging, log modification, changes to access permission on log files,
    * Files and system level objects creation and deletion,
    * Attempts to perform unauthorized functions or access data that the user is not authorized to access,
    * Modification of security rules,
    * Application and related systems start-ups and shut-downs
* For privileged accounts:
    * Attempts to execute privilege elevation
    * System errors relevant to security events, including but not limited to SQL errors that indicate an SQL injection, fuzzing, multiple failed logins, failed configuration change, failed/disabled anti-virus software, service failures
    * Web access events in extended log format

The events that are collected should comply with the Cloud Auditing Data Federation (CADF) standard. And, audit events must contain at a minimum:

* Type of event
* When the event occurred
* Where the event occurred
* Source of the event
* Outcome (success or failure) of the event
* Identity of any user/subject/device associated with the event
* Port and protocol, where available
* Session duration, identification, and modification where available
* Authentication method

## Storing events in a SIEM
{: #siem}

A SIEM system provides the ability to gather security data from information system components and presents that data as actionable information through a single interface. Events of the type in the previous section should be stored in a SIEM. You need to install your own software solution within your VPC for SIEM. 

## Related {{site.data.keyword.cloud_notm}} for Financial Services controls
{: #related-controls}

{{site.data.content.related-controls-disclaimer}}

| Family              | Control                                           |
|---------------------|---------------------------------------------------|
| Access Control (AC) | [AC-2 Account Management](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-ac-2) \n [AC-2 (1) Account Management &#124; Automated System Account Management](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-ac-2.1) \n [AC-2 (4) Account Management &#124; Automated Audit Actions](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-ac-2.4) \n [AC-2 (7) Account Management &#124; Privileged User Accounts](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-ac-2.7) |
| Audit and Accountability (AU) | [AU-3 Content of Audit Records](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-au-3) \n [AU-4 Audit Log Storage Capacity](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-au-4) \n [AU-5 Response to Audit Processing Failures](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-au-5) \n [AU-6 Audit Record Review, Analysis. and Reporting](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-au-6) \n [AU-6 (1) Audit Record Review, Analysis. and Reporting &#124; Automated Process Integration](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-au-6.1) \n [AU-7 Audit Record Reduction and Report Generation](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-au-7) \n [AU-10 Non-repudiation](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-au-10) \n [AU-11 Audit Record Retention](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-au-11) |
{: caption="Table 1. Related controls in {{site.data.keyword.framework-fs_notm}}" caption-side="top"}

## Next steps
{: #next-steps}

* [Operational logging](/docs/framework-financial-services?topic=framework-financial-services-shared-logging-operational)
