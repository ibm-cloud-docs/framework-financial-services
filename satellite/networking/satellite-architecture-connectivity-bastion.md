---

copyright:
  years: 2020, 2025
lastupdated: "2025-03-02"

keywords:

subcollection: framework-financial-services

---

{{site.data.keyword.attribute-definition-list}}

# Completing workload operator actions through a bastion host
{: #satellite-architecture-connectivity-bastion}

All interactive operator actions on the resources in {{site.data.keyword.satelliteshort}} location must be run through a bastion host in the management plane. A bastion host is a server that can be accessed through SSH, Windows Remote Desktop Protocol (RDP), `oc` (Red Hat OpenShift CLI), or `kubectl`, but only through a private secure connection. After set up, the bastion host allows a secure connection to {{site.data.keyword.openshiftshort}} clusters, {{site.data.keyword.satelliteshort}} APIs, or {{site.data.keyword.satelliteshort}} location hosts. Administrative tasks on the individual services or hosts are completed by using SSH, `oc`, or `kubectl` proxied through the bastion. In addition, session auditing must be enabled to record all privileged user actions.
{: shortdesc}

After connecting to your management plane through your designated connection method specific to your environment, a complete bastion solution should meet the following requirements.

{{site.data.content.bastion-host-requirements}}

## Access to {{site.data.keyword.openshiftshort}} management APIs
{: #openshit-api}

{{site.data.keyword.openshiftshort}} management tasks can be completed by using `oc` CLI tool and `kubectl`. If your bastion solution provides direct support for `kubectl` or `oc` sessions, it is recommended to configure such capabilities for the {{site.data.keyword.openshiftshort}} clusters that are deployed to your {{site.data.keyword.satelliteshort}} locations. Otherwise, you should consider setting up hosts in your management plane that can provide shell access with `oc` or `kubectl` CLI tools to complete {{site.data.keyword.openshiftshort}} management. Operators should access those hosts through SSH connecting through a bastion.

## Access to {{site.data.keyword.openshiftshort}} management UI
{: #openshit-api}

{{site.data.keyword.openshiftshort}} cluster that is deployed on a {{site.data.keyword.satelliteshort}} location provides a management UI (web console). You should consider UI access as an exception, preferring the operator actions should CLI or CI/CD automation for effective access control, reliability, and audit.

If the operators or administrators need to access this UI, the access should be facilitated by the bastion solution as well. In this scenario, a bastion solution would act as an HTTPS reverse proxy that the operator would use to access the {{site.data.keyword.openshiftshort}} management UI.

## Access to {{site.data.keyword.satelliteshort}} hosts through SSH
{: #openshit-api}

In general, the {{site.data.keyword.satelliteshort}} architecture discourages shell to the host OS, including SSH. Host OS updates and other maintenance (for example, backups) should be automated or completed by creating and attaching a new host instance. Login with a `root` account is explicitly blocked after a host is attached to a {{site.data.keyword.satelliteshort}} location.

If operator access to a {{site.data.keyword.satelliteshort}} host is required for troubleshooting purposes, SSH connection should be completed through a bastion that uses a nonroot account.



## Set up a bastion host
{: #setup}

You need to install and manage your own bastion solution within your management plane. A bastion solution can be implemented in a variety of ways. For one example that uses Teleport Enterprise Edition in the VPC reference architecture, see [Setting up a bastion host for secure connectivity](/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-connectivity-bastion-tutorial-teleport).

## Related controls in {{site.data.keyword.framework-fs_notm}}
{: #next-steps}

{{site.data.content.related-controls-disclaimer}}

| Family              | Control                                           |
|---------------------|---------------------------------------------------|
| Audit and Accountability (AU) | [AU-14 Session Audit](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-au-14)  |
| Access Control (AC) | [AC-6 (9) Least Privilege &#124; Log Use of Privileged Functions](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-ac-6.9) \n [AC-17 Remote Access](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-ac-17)  |
| Identification and Authentication (IA) | [IA-2 (1) Identification and Authentication (organizational Users) &#124; Multi-factor Authentication to Privileged Accounts](/docs/framework-financial-services-controls?topic=framework-financial-services-controls-ia-2.1) |
{: caption="Related controls in {{site.data.keyword.framework-fs_notm}} [FSv2.0]" caption-side="top"}
{: #related-controls-fsv2.0}
{: tab-title="FSv2.0"}
{: tab-group="RelatedControls-1"}
{: class="simple-tab-table"}

| Family              | Control                                           |
|---------------------|---------------------------------------------------|
| Audit and Accountability (AU) | [AU-14 Session Audit](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-au-14)  |
| Access Control (AC) | [AC-6 (9) Least Privilege &#124; Auditing Use of Privileged Functions](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-ac-6.9) \n [AC-17 Remote Access](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-ac-17)  |
| Identification and Authentication (IA) | [IA-2 (1) Identification and Authentication (Organizational Users) &#124; Network Access to Privileged Accounts](/docs/framework-financial-services-controls-fsv1-1?topic=framework-financial-services-controls-fsv1-1-ia-2.1) |
{: caption="Related controls in {{site.data.keyword.framework-fs_notm}} [FSv1.1]" caption-side="top"}
{: #related-controls-fsv1.1}
{: tab-title="FSv1.1"}
{: tab-group="RelatedControls-1"}
{: class="simple-tab-table"}


## Next steps
{: #next-steps}

- [Using {{site.data.keyword.cloud_notm}} services with {{site.data.keyword.satelliteshort}} Link](/docs/framework-financial-services?topic=framework-financial-services-satellite-architecture-connectivity-to-services)
