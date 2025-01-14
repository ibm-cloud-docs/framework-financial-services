---

copyright:
  years: 2020, 2024
lastupdated: "2024-11-19"

keywords:

subcollection: framework-financial-services

content-type: tutorial
services:
account-plan: paid
completion-time: 2h

---

{{site.data.keyword.attribute-definition-list}}

# Setting up an operational logging solution
{: #vpc-architecture-logging-operational-tutorial}
{: toc-content-type="tutorial"}
{: toc-services="openshift"}
{: toc-completion-time="2h"}

This tutorial shows you one way that can be used to meet the {{site.data.keyword.framework-fs_notm}} requirements that are related to [operational logging](/docs/framework-financial-services?topic=framework-financial-services-shared-logging-operational). This approach uses [{{site.data.keyword.openshiftshort}} Container Platform Elasticsearch, Fluentd, and Kibana EFK stack](https://docs.openshift.com/container-platform/4.6/logging/cluster-logging.html){: external} by installing the [Cluster Logging Operator](https://docs.openshift.com/container-platform/4.6/logging/cluster-logging.html){: external}. The log data remains within your environment so that you retain full control over it.
{: shortdesc}

We provide guidance, but you are solely responsible for installing, configuring, and operating {{site.data.keyword.IBM_notm}} third-party software in a way that satisfies {{site.data.keyword.framework-fs_notm}} requirements. In addition, {{site.data.keyword.IBM_notm}} does not provide support for third-party software.
{: important}

The final result is a general logging solution that can be used for:

* Logs from {{site.data.keyword.openshiftshort}} applications
* {{site.data.keyword.openshiftshort}} platform logs
* Logs from applications that are running on virtual server instances
* System logs from virtual server instances

## Logging solution architecture
{: #arch-overview}

The capability of the [{{site.data.keyword.openshiftshort}} Cluster Logging Operator](https://docs.openshift.com/container-platform/4.6/logging/cluster-logging.html){: external} allows administrators to aggregate logs across a {{site.data.keyword.openshiftshort}} cluster. These logs include application container logs, infrastructure component logs, and system audit logs. Administrators deploy cluster logging by deploying the Elasticsearch Operator and the Cluster Logging Operator. These operators deploy Elasticsearch servers for collecting and indexing logs, Fluentd for sending logs from worker nodes, and Kibana for searching and displaying the logs.

When using {{site.data.keyword.openshiftshort}} logging for financial service environments, you must install the Elasticsearch Operator and Cluster Logging Operator in every {{site.data.keyword.openshiftshort}} cluster that requires logging. The logs are retained by the Elasticsearch servers that are running in that cluster and stored on the attached VPC block storage (which must be [encrypted by using keys that are managed by {{site.data.keyword.hscrypto}}](/docs/framework-financial-services?topic=framework-financial-services-shared-encryption-at-rest)). The logs can be searched and viewed by using the Kibana server that is deployed in that cluster. The following diagram shows the Elasticsearch pods that are deployed to three zones within both the management and workload VPCs. Kibana is shown deployed to a single zone.

![{{site.data.keyword.cloud_notm}} for Financial Services reference architecture with logging ](../../images/logmon/roks-single-region-log-mon-v2.svg){: caption="Single-region VPC reference architecture using EFK for logging" caption-side="bottom"}

The {{site.data.keyword.openshiftshort}} pods that are running the Elasticsearch, Kibana, and Fluentd components are assigned to a worker pool that is dedicated to logging. A dedicated worker pool prevents logging operations from stealing resources from pods that are running other applications. Worker pools are not shown in the diagram.

Instances of the Elasticsearch server are configured to run in all three of the region's zones. The Elasticsearch cluster is configured to replicate data, which is stored in shards, to a second server in the cluster.

The logging stack that is running in each VPC can also be used to collect application logs from virtual server instances that are using Fluentd. You can collect logs from virtual server instances by using [Fluentd's Elasticsearch plug-in](https://docs.fluentd.org/output/elasticsearch){: external} to forward logs to the logging stack that is running in the {{site.data.keyword.openshiftshort}} cluster.

When you configure {{site.data.keyword.openshiftshort}} for both operational logging and operational monitoring, the worker nodes can be shared. You can use the same worker pool for both logging and monitoring. You can use the same taint tag to steer monitoring and logging pods to the shared worker pool.
{: note}

To implement your operational logging solution, you need to complete the following high-level steps:

1. Provision an instance of {{site.data.keyword.openshiftshort}}
2. Configure the worker pool in your {{site.data.keyword.openshiftshort}} cluster.
3. Install the Elasticsearch Operator within your {{site.data.keyword.openshiftshort}} cluster.
4. Install the Cluster Logging Operator within your {{site.data.keyword.openshiftshort}} cluster.
5. Create a cluster logging instance.
6. Set up virtual server instance logging with Fluentd.

## Before you begin
{: #openshift-cluster-logging-prerequisites}

* You have a [VPC](/docs/vpc?topic=vpc-getting-started) provisioned.
* Subnets are provisioned across three zones within a region.

## Provision {{site.data.keyword.openshiftlong}}
{: #setup-openshift}
{: step}

To capture logs from workloads that are running outside of {{site.data.keyword.openshiftshort}} (such as a virtual server instance), you need to provision an instance of {{site.data.keyword.openshiftlong_notm}} if you don't already have one.

1.  [Provision {{site.data.keyword.openshiftlong}}](/docs/openshift?topic=openshift-openshift_tutorial) within the workload VPC where you plan to install the monitoring service. Use the following configuration:
   * {{site.data.keyword.openshiftshort}} version: `4.6.x`
   * Worker zones: `User defined subnet in each zone of the region`
   * Worker nodes per zone: `1`
   * Flavor: `mx2.4x32 - 4 vCPU, 32 GB Memory`
   * Master service endpoint: `Private endpoint only`

## Provision a {{site.data.keyword.openshiftshort}} worker pool
{: #provision-worker-pool}
{: step}

1. Use a separate worker pool for the logging stack to keep the logging stack resources distinct from other workload resources. [Provision a new worker pool](/docs/openshift?topic=openshift-add_workers#vpc_pools) with the following configuration:
   * Worker zones: `User defined subnet in each zone of the region`
   * Worker nodes per zone: `1`
   * Flavor: `mx2.4x32 - 4 vCPU, 32 GB Memory`

2. After provisioning completes, [access the {{site.data.keyword.openshiftshort}} cluster](/docs/openshift?topic=openshift-access_cluster#vpc_private_se).
3. Taint the worker pool.  By providing a taint on the worker pool, it ensures that only the logging stack runs on the worker pool.  For more information on taints and tolerations, see the [{{site.data.keyword.openshiftshort}} documentation](https://docs.openshift.com/container-platform/4.6/nodes/scheduling/nodes-scheduler-taints-tolerations.html#nodes-scheduler-taints-tolerations-about_nodes-scheduler-taints-tolerations){: external}.  A taint can be set on a worker pool with the following `ibmcloud` CLI command:
   ```
   ibmcloud oc worker-pool taint set --worker-pool <WORKER_POOL> --cluster <CLUSTER> --taint KEY=VALUE:EFFECT
   ```

   An example taint setting of `logging-monitoring=node:NoExecute`, can be set by using the following ibmcloud cli command:
   ```
   ibmcloud oc worker-pool taint set --worker-pool <WORKER_POOL> --cluster <CLUSTER> --taint logging-monitoring=node:NoExecute
   ```

## Install the Elasticsearch Operator
{: #install-elasticsearch-operator}
{: step}

The Elasticsearch Operator creates and manages the Elasticsearch cluster that is used by {{site.data.keyword.openshiftshort}} cluster logging. It can be installed from the {{site.data.keyword.openshiftshort}} console.

1. Install the [Elasticsearch Operator](https://docs.openshift.com/container-platform/4.6/logging/cluster-logging-deploying.html#cluster-logging-deploy-console_cluster-logging-deploying){: external}.
1. Ensure that the following settings are selected or configured:
   * **All namespaces on the cluster** are selected under **Installation Mode**.
   * **openshift-operators-redhat** is selected under **Installed Namespace**.
   * **Enable operator recommended cluster monitoring on this namespace** is enabled.
   * **Automatic** is selected for **Automatic Approval Strategy**.
1. Ensure that **Elasticsearch Operator** is listed in all projects with a **Status of Succeeded**.

## Install the Cluster Logging Operator
{: #install-logging-operator}
{: step}

The Cluster Logging operator creates and manages the components of the logging stack.

1. Install the [Cluster Logging Operator provided by {{site.data.keyword.redhat_notm}}](https://docs.openshift.com/container-platform/4.6/logging/cluster-logging-deploying.html#cluster-logging-deploy-console_cluster-logging-deploying){: external}).
1. Ensure that the following settings are selected or configured:
   * The **A specific namespace on the cluster** is selected under **Installation Mode**.
   * The **Operator recommended namespace is openshift-logging** is selected under **Installed Namespace**.
   * **Enable operator recommended cluster monitoring on this namespace** is selected.
   * **Automatic** is selected for **Automatic Approval Strategy**.
1. Ensure that **Cluster Logging** is listed in the **openshift-logging** project with a **Status** of **Succeeded**.

## Create a cluster logging instance
{: #create-logging-instance}
{: step}

You can create a cluster logging instance to deploy pods for Elasticsearch and Kibana servers. It can also deploy Fluentd for collecting infrastructure logs from the worker nodes.

The AU-11 control requires a minimum of 90 days of online audit records and one (1) year's worth of records offline. The default retention periods are lengthened to 90 days in the following example YAML file to reflect this.
{: note}

1. Create a cluster logging instance that uses the [{{site.data.keyword.openshiftshort}} documentation](https://docs.openshift.com/container-platform/4.6/logging/cluster-logging-deploying.html#cluster-logging-deploy-console_cluster-logging-deploying){: external}.
   1. In the **YAML** field, replace the code with the following example code. The sample code configures the logging stack to complete the following tasks:
      * Run the logging stack on the provisioned worker pool.
      * Add a retention period of 90 days.
      * Add a 200 GB persistent volume to Elasticsearch.

      ```
      apiVersion: "logging.openshift.io/v1"
      kind: "ClusterLogging"
      metadata:
        name: "instance"
        namespace: "openshift-logging"
      spec:
        managementState: "Managed"
        logStore:
          type: "elasticsearch"
          retentionPolicy:
            application:
              maxAge: 90d
            infra:
              maxAge: 90d
            audit:
             maxAge: 90d
          elasticsearch:
            nodeCount: 3
            tolerations:
            - key: "logging-monitoring"
              operator: "Exists"
              effect: "NoExecute"
              tolerationSeconds: 600
            storage:
              storageClassName: "ibmc-vpc-block-general-purpose"
              size: 200G
            resources:
              requests:
                memory: "24Gi"
            proxy:
              resources:
                limits:
                  memory: 256Mi
                requests:
                   memory: 256Mi
            redundancyPolicy: "MultipleRedundancy"
        visualization:
          type: "kibana"
          kibana:
            replicas: 1
            tolerations:
            - key: "logging-monitoring"
              operator: "Exists"
              effect: "NoExecute"
              tolerationSeconds: 600
        curation:
          type: "curator"
          curator:
            schedule: "30 3 * * *"
        collection:
          logs:
            type: "fluentd"
            fluentd: {}
      ```
      {: codeblock}

   The `redundancyPolicy` is set to `MultipleRedundancy`, which causes Elasticsearch to replicate shards to half of the data nodes (in this case, 2). To replicate shards to all three worker nodes, set `redundancyPolicy` to `FullRedundancy`.

You might need to adjust the following items:

* Application retention policy
* Infrastructure retention policy
* Audit retention policy
* The size of the block storage created for each Elasticsearch server (200 GB by default) to hold the logs expected to accumulate over the retention period

Verify that the logging instance is created by completing the following tasks.

1. Switch to the **Workloads → Pods** page.
1. Select the **openshift-logging** project. A list of several pods displays. The list of pods looks similar to the following list and includes pods for cluster logging, Elasticsearch, Fluentd, and Kibana:
    * cluster-logging-operator-cb795f8dc-xkckc
    * elasticsearch-cdm-b3nqzchd-1-5c6797-67kfz
    * elasticsearch-cdm-b3nqzchd-2-6657f4-wtprv
    * elasticsearch-cdm-b3nqzchd-3-588c65-clg7g
    * fluentd-2c7dg
    * fluentd-9z7kk
    * fluentd-br7r2
    * fluentd-fn2sb
    * fluentd-pb2f8
    * fluentd-zqgqx
    * kibana-7fb4fd4cc9-bvt4p

### Starting Kibana
{: #launching-kibana}

You can start Kibana from the menu bar of the {{site.data.keyword.openshiftshort}} console.

1. Click the 3x3 matrix icon in the menu bar.
1. Select **Observability → Logging**.
2. The first time that you access Kibana you must use your {{site.data.keyword.cloud_notm}} credentials to log in.

## Set up virtual server instance logging with Fluentd
{: #setting-up-vsi-logging-with-fluentd}
{: step}

The EFK stack that is deployed in {{site.data.keyword.openshiftshort}} can also be used to aggregate application and infrastructure logs from virtual server instances. The following instructions describe how to install and configure Fluentd on a virtual server instance so that it can forward its logs to an Elasticsearch cluster.

### Preparing the Elasticsearch cluster to receive logs
{: #preparing-elasticsearch-cluster-receive-logs}

Fluentd should use a {{site.data.keyword.openshiftshort}} private endpoint when sending log data to the Elasticsearch cluster.
{: note}

To enable the Elasticsearch cluster to receive logs from a Fluentd client running outside of the {{site.data.keyword.openshiftshort}} cluster, you must create a route to the Elasticsearch service.

1. Navigate to the {{site.data.keyword.openshiftshort}} web console.
1. Select **Networking → Routes**.
1. Select **Project** openshift-logging
1. Click **Create Route**.
    1. Select an intuitive name, such as `elasticsearch`.
    1. Select **Elasticsearch** for the service.
    1. Select **9200 → restapi (TCP)** as the **Target Port**
    1. Select **Secure route**.
    1. Select **Re-encrypt** for **TLS Termination**.
    1. Select **None** for **Insecure Traffic**.
    1. Set the **Destination CA Certificate** to the ElasticSearch CA ([reference](https://docs.openshift.com/container-platform/4.1/logging/config/efk-logging-elasticsearch.html#efk-logging-elasticsearch-exposing_efk-logging-elasticsearch){: external}). You can retrieve the certificate with the following command:

        ```
        $ oc extract secret/elasticsearch --to=. --keys=admin-ca -n openshift-logging
        ```
        {: codeblock}

    1. The other fields, including the other certificate and key fields, can be left blank.
1. Click **Create**.
1. After the route is created, note the hostname for the route. You can find the hostname at following path: **Networking → Routes → Elasticsearch**. The hostname is in this format: **Host** `elasticsearch-openshift-logging.workaround-logging-caf539b8d718205a334907f986dcee2a-0000.us-south.containers.appdomain.cloud`.

### Retrieving the bearer token for Elasticsearch
{: retrieving-bearer-token-elasticsearch}

When forwarding logs to the Elasticsearch cluster, the Fluentd client authenticates to the Elasticsearch service by using a bearer token. You can retrieve the bearer token for the Elasticsearch Operator with the following commands:

```
$ oc project openshift-operators-redhat
$ oc sa get-token elasticsearch-operator
```
{: codeblock}

Save the bearer token to use in the next section.

### Installing Fluentd on a virtual server instance
{: installing-fluentd-vsi}

You can install Fluentd on a virtual server instance. For more information about installing on different operating systems, see the [instructions for installing Fluentd](https://docs.fluentd.org/installation){: external}. For Linux, use the `td-agent`.

Log forwarding to the {{site.data.keyword.openshiftshort}} cluster is done by the [Elasticsearch Fluentd plug-in](https://docs.fluentd.org/output/elasticsearch){: external}. The Elasticsearch plug-in is installed as part of the td-agent installation and does not require any additional installation steps. Log forwarding is configured in the `td-agent.conf` file.

The following example is a `td-agent.conf` file that forwards logs to an Elasticsearch server. You must make the appropriate changes where noted with `<variables>`.

```
<system>
  log_level debug
</system>

<source>
  @type tail
  path /var/log/td-agent/td-agent.log
  tag tail.messages
  <parse>
    @type none
  </parse>
</source>

<source>
  @type syslog
  port 5140
  tag system.local
</source>

<match **>
  @type copy
  <store>
    @type elasticsearch
    @id remote_elasticsearch
    host <elasticsearch route hostname>
    port 443
    logstash_format true
      
    ssl_verify true
    verify_es_version_at_startup false
    scheme https
    ssl_version TLSv1_2
      
    custom_headers {"Authorization":"Bearer <Bearer Token>"}
  </store>
</match>
```
{: codeblock}

For more information on the custom_headers configuration, see the Elasticsearch plug-in for [custom_headers](https://github.com/uken/fluent-plugin-elasticsearch#custom_headers){: external}.
{: note}

This sample `td-agent.conf` file includes two `<source>` sections:

1. The `@type tail` source plug-in captures log entries that are written to the Fluentd log file (`/var/log/td-agent/td-agent.log`) and forwards them.
1. The `@type syslog` source plug-in enables the local Fluentd client to retrieve records by using the syslog protocol on UDP or TCP.

These `<source>` sections can be modified and new ones added to collect both application and system log files and forward them. For more information, see [Fluentd input plug-ins](https://docs.fluentd.org/input){: external}.

Edit the file `/etc/td-agent/td-agent.conf` and replace its contents with the preceding example. Make sure to set the host to the Elasticsearch route hostname and the bearer token to the token that was retrieved in the previous section.

After the `td-agent.conf` file is updated, you can start the td-agent with the following command:

```
$ systemctl start td-agent.service
```
{: codeblock}

The td-agent starts forwarding log messages to the {{site.data.keyword.openshiftshort}} Elasticsearch cluster. You can view them there by using Kibana.

## Related controls in {{site.data.keyword.framework-fs_notm}}
{: #related-controls}

See the [related controls for operational logging](/docs/framework-financial-services?topic=framework-financial-services-shared-logging-operational#related-controls).
