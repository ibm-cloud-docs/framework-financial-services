---

copyright:
  years: 2020, 2024
lastupdated: "2024-10-09"

keywords:

subcollection: framework-financial-services

content-type: tutorial
services:
account-plan: paid
completion-time: 2h

---

{{site.data.keyword.attribute-definition-list}}



# Setting up an operational monitoring solution
{: #vpc-architecture-monitoring-operational-tutorial}
{: toc-content-type="tutorial"}
{: toc-services="openshift, vpc"}
{: toc-completion-time="2h"}


This tutorial shows you one way that can be used to meet the {{site.data.keyword.framework-fs_notm}} requirements that are related to [operational monitoring](/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-monitoring-operational-tutorial) by using [Prometheus and Grafana](https://docs.openshift.com/container-platform/4.5/monitoring/cluster_monitoring/about-cluster-monitoring.html){: external} on {{site.data.keyword.openshiftshort}}. This approach can be used to can gain insight into the health and performance of the provisioned infrastructure and the workloads that are running on the infrastructure -- all while keeping your monitoring data safe within your environment.
{: shortdesc}

We provide guidance here, but you are solely responsible for installing, configuring, and operating {{site.data.keyword.IBM_notm}} third-party software in a way that satisfies {{site.data.keyword.framework-fs_notm}} requirements. In addition, {{site.data.keyword.IBM_notm}} does not provide support for third-party software.
{: important}

## Monitoring solution architecture
{: #arch-overview}

The architecture diagram shows a monitoring deployment within a {{site.data.keyword.openshiftshort}} cluster for a single region. The architecture enables gathering metrics for {{site.data.keyword.openshiftshort}} applications and virtual server instances within your VPCs. The label "Prom" in the workload clusters represents Prometheus.

![{{site.data.keyword.cloud_notm}} for Financial Services reference architecture with operational monitoring](../../images/logmon/roks-single-region-log-mon-v2.svg){: caption="Single-region {{site.data.keyword.cloud_notm}} for Financial Services reference architecture with operational monitoring" caption-side="bottom"}

When you configure {{site.data.keyword.openshiftshort}} for both operational logging and operational monitoring, the worker nodes can be shared. You can use the same worker pool for both logging and monitoring. You can use the same taint tag to steer monitoring and logging pods to the shared worker pool.
{: tip}

{{site.data.keyword.openshiftshort}} provides a built-in monitoring stack. This stack is used to set up monitoring and default alerting. For more information, see [Understanding the monitoring stack](https://docs.openshift.com/container-platform/4.6/monitoring/understanding-the-monitoring-stack.html){: external}

To implement your operational monitoring solution, you need to complete the following high-level steps:

1. Provision an instance of {{site.data.keyword.openshiftshort}}.
2. Configure the worker pool in your {{site.data.keyword.openshiftshort}} cluster.
3. Configuring the {{site.data.keyword.openshiftshort}} monitoring stack.
4. Configure monitoring for a user-defined {{site.data.keyword.openshiftshort}} project.
5. Configure monitoring for a VPC virtual server instance.
6. Set up a custom Grafana dashboard.

## Before you begin
{: #monitoring-prereqs}

* You have a [VPC](/docs/vpc?topic=vpc-getting-started) provisioned.
* Subnets are provisioned across 3 zones within a region.

## Provision {{site.data.keyword.openshiftlong}}
{: #step1-setup-openshift}
{: step}

To capture metrics from workloads running outside of {{site.data.keyword.openshiftshort}} (such as a virtual server instance), you need to provision an instance of {{site.data.keyword.openshiftlong_notm}} if you don't already have one.

1.  [Provision {{site.data.keyword.openshiftlong}}](/docs/openshift?topic=openshift-openshift_tutorial) within the workload VPC where you plan to install the monitoring service. Use the following configuration:
   * {{site.data.keyword.openshiftshort}} version: `4.6.x`
   * Worker zones: `User defined subnet in each zone of the region`
   * Worker nodes per zone: `1`
   * Flavor: `mx2.4x32 - 4 vCPU, 32 GB Memory`
   * Master service endpoint: `Private endpoint only`

## Provision a {{site.data.keyword.openshiftshort}} worker pool
{: #step2-provision-worker-pool}
{: step}

1. Use a separate worker pool for the monitoring stack to keep the monitoring stack resources distinct from other workload resources. [Provision a new worker pool](/docs/openshift?topic=openshift-add_workers#vpc_pools) with the following configuration:
   * Worker zones: `User defined subnet in each zone of the region`
   * Worker nodes per zone: `1`
   * Flavor: `mx2.4x32 - 4 vCPU, 32 GB Memory`

1. After provisioning completes, [access the {{site.data.keyword.openshiftshort}} cluster](/docs/openshift?topic=openshift-access_cluster#vpc_private_se).
1. Taint the worker pool.  By providing a taint on the worker pool, it ensures that only the monitoring stack runs on the worker pool.  For more information on taints and tolerations, see the [{{site.data.keyword.openshiftshort}} documentation](https://docs.openshift.com/container-platform/4.6/nodes/scheduling/nodes-scheduler-taints-tolerations.html#nodes-scheduler-taints-tolerations-about_nodes-scheduler-taints-tolerations){: external}.  A taint can be set on a worker pool with the following `ibmcloud` CLI command:
   ```
   ibmcloud oc worker-pool taint set --worker-pool <WORKER_POOL> --cluster <CLUSTER> --taint KEY=VALUE:EFFECT
   ```

   An example taint setting of `logging-monitoring=node:NoExecute`, can be set by using the following ibmcloud cli command:
   ```
   ibmcloud oc worker-pool taint set --worker-pool <WORKER_POOL> --cluster <CLUSTER> --taint logging-monitoring=node:NoExecute
   ```

## Configure the {{site.data.keyword.openshiftshort}} monitoring stack
{: #step3-configure-monitoring-stack}
{: step}

1. For conceptual information about the monitoring stack, see [Understanding the monitoring stack](https://docs.openshift.com/container-platform/4.6/monitoring/understanding-the-monitoring-stack.html){: external}.
2. To configure the monitoring stack with supported options, see [Maintenance and support for monitoring](https://docs.openshift.com/container-platform/4.6/monitoring/configuring-the-monitoring-stack.html#maintenance-and-support_configuring-the-monitoring-stack){: external}.
   1. Create and edit the `cluster-monitoring-config` ConfigMap in the *openshift-monitoring* project with the following configuration.  For more information, see the [Configuring the monitoring stack](https://docs.openshift.com/container-platform/4.6/monitoring/configuring-the-monitoring-stack.html#configuring-the-monitoring-stack_configuring-the-monitoring-stack){: external}.  The sample code configures the monitoring stack to complete the following tasks:
     * Run the monitoring stack on the provisioned worker pool.
     * Add a retention period of 1 year.
     * Add a 100 GB persistent volume to Prometheus.

      ```
      data:
        config.yaml: |
          enableUserWorkload: true
          prometheusOperator:
            tolerations:
            - key: "logging-monitoring"
              operator: "Equal"
              value: "node"
              effect: "NoExecute"
          prometheusK8s:
            retention: 1y
            volumeClaimTemplate:
              spec:
                storageClassName: ibmc-vpc-block-retain-general-purpose
                volumeMode: Filesystem
                resources:
                   requests:
                     storage: 100Gi
            tolerations:
            - key: "logging-monitoring"
              operator: "Equal"
              value: "node"
              effect: "NoExecute"
          alertmanagerMain:
            tolerations:
            - key: "logging-monitoring"
              operator: "Equal"
              value: "node"
              effect: "NoExecute"
          kubeStateMetrics:
            tolerations:
            - key: "logging-monitoring"
              operator: "Equal"
              value: "node"
              effect: "NoExecute"
          openshiftStateMetrics:
            tolerations:
            - key: "logging-monitoring"
              operator: "Equal"
              value: "node"
              effect: "NoExecute"
          telemeterClient:
            tolerations:
            - key: "logging-monitoring"
              operator: "Equal"
              value: "node"
              effect: "NoExecute"
          k8sPrometheusAdapter:
            tolerations:
            - key: "logging-monitoring"
              operator: "Equal"
              value: "node"
              effect: "NoExecute"
          thanosQuerier:
            tolerations:
            - key: "logging-monitoring"
              operator: "Equal"
              value: "node"
              effect: "NoExecute"
      ```
      {: codeblock}

   2.  Create the `user-workload-monitoring-config` ConfigMap in the *openshift-user-workload-monitoring* project with the following configuration.  For more information, see the [Configuring the monitoring stack](https://docs.openshift.com/container-platform/4.6/monitoring/configuring-the-monitoring-stack.html#configuring-the-monitoring-stack_configuring-the-monitoring-stack){: external}.  The sample code configures the monitoring stack to complete the following tasks:
      * Run the user workload monitoring stack on the provisioned worker pool.
      * Add a retention period of 1 year.
      * Add a 100 GB persistent volume to Prometheus.

      ```
      data:
        config.yaml: |
          prometheus:
            retention: 1y
            volumeClaimTemplate:
              spec:
                storageClassName: ibmc-vpc-block-retain-general-purpose
                volumeMode: Filesystem
                resources:
                  requests:
                    storage: 100Gi
            tolerations:
            - key: "logging-monitoring"
              operator: "Equal"
              value: "node"
              effect: "NoExecute"
      ```
      {: codeblock}

## Configure monitoring for a user-defined {{site.data.keyword.openshiftshort}} project
{: #step4-configure-monitoring-user-defined-project}
{: step}

If you are running your applications within {{site.data.keyword.openshiftshort}}, you can configure the monitoring stack to monitor your user-defined project.

1. [Create a ServiceMonitor](https://docs.openshift.com/container-platform/4.6/monitoring/managing-metrics.html#specifying-how-a-service-is-monitored_managing-metrics){: external} for each of your projects where you want to collect metrics.<br>

   If you do not have an application available, you can use the [sample service](https://docs.openshift.com/container-platform/4.6/monitoring/managing-metrics.html#deploying-a-sample-service_managing-metrics){: external} to test and verify.
   {: note}

2. [Create and manage alerts](https://docs.openshift.com/container-platform/4.6/monitoring/managing-alerts.html#managing-alerts){: external} for your project.

## Step 5: Configure monitoring for a virtual server instance
{: #step5-configure-monitoring-virtual-server-instance}

You can use the {{site.data.keyword.openshiftshort}} monitoring stack to gather metrics for your virtual server instances in your VPC.  Complete the following steps to set up monitoring for virtual server instances.

### Linux
{: #step5-configure-monitoring-virtual-server-instance-linux}

1. To monitor Linux host metrics, you can install [Prometheus Node Exporter](https://github.com/prometheus/node_exporter){: external} on the virtual server to expose a wide range of metrics.
     1. Log in to your virtual server instance by using SSH.
     2. Issue the following commands:
      ```
      cd /tmp
      curl -LO https://github.com/prometheus/node_exporter/releases/download/v1.1.2/node_exporter-1.1.2.linux-amd64.tar.gz
      tar -xvf node_exporter-1.1.2.linux-amd64.tar.gz
      mv node_exporter-1.1.2.linux-amd64/node_exporter /usr/local/bin/
      ```
      {: codeblock}

     1. Create a custom node exporter service file in `/etc/systemd/system/node_exporter.service` with the following content:
      ```
      [Unit]
      Description=Node Exporter
      After=network.target

      [Service]
      User=node_exporter
      Group=node_exporter
      Type=simple
      ExecStart=/usr/local/bin/node_exporter

      [Install]
      WantedBy=multi-user.target
      ```
      {: codeblock}

     2. Register the node exporter service:
      ```
      sudo useradd --system --shell /bin/false node_exporter
      sudo systemctl daemon-reload
      sudo systemctl start node_exporter
      sudo systemctl enable node_exporter
      ```
      {: codeblock}

### Windows
{: #step5-configure-monitoring-virtual-server-instance-windows}

1. To monitor Windows host metrics, you can install the [Prometheus exporter for Windows machines](https://github.com/prometheus-community/windows_exporter){: external}.

### Custom endpoint for exposing-metrics
{: #step5-configure-monitoring-virtual-server-instance-custom-endpoint}

Your application can also expose an endpoint that you can use to provide metrics to Prometheus.

1. Allow TCP inbound traffic to port 9100 and any other ports that your application uses to expose metrics within the VPC [access control list](/docs/vpc?topic=vpc-using-acls) and [security group](/docs/vpc?topic=vpc-using-security-groups) that your virtual server instance is in.
2. Within the {{site.data.keyword.openshiftshort}} cluster, create a project called *monitoring-vpc-vsi*.
   ```
   oc new-project monitoring-vpc-vsi
   ```
3. For each of your virtual server instances apply the following configuration into the *monitoring-vpc-vsi* project.  Make the appropriate changes where noted with `<variables>`. The following content is a sample and assumes that it is scraping your virtual server instance by using the HTTP protocol on port *9100* with the path of `/metrics`:
   1. Apply the *Service* resource:
      ```
      kind: Service
      apiVersion: v1
      metadata:
        name: vsi-<Name of VSI you used to provision>
        namespace: monitoring-vpc-vsi
        labels:
          k8s-app: vsi-<Name of VSI you used to provision>
      spec:
        externalName: <IP Address of your VSI>
        type: ExternalName
        ports:
        - name: http
          port: 9100
          protocol: TCP
          targetPort: 9100
      ```
      {: codeblock}

   1. Apply the *Endpoint* resource:
      ```
      apiVersion: v1
      kind: Endpoints
      metadata:
        name: vsi-<Name of VSI you used to provision>
        namespace: monitoring-vpc-vsi
        labels:
          k8s-app: vsi-<Name of VSI you used to provision>
      subsets:
      - addresses:
        - ip: <IP Address of your VSI>
        ports:
        - name: http
          port: 9100
          protocol: TCP
      ```
      {: codeblock}

   1. Apply the *ServiceMonitor* resource:
      ```
      apiVersion: monitoring.coreos.com/v1
      kind: ServiceMonitor
      metadata:
        name: vsi-<Name of VSI you used to provision>
        labels:
          k8s-app: vsi-<Name of VSI you used to provision>
        namespace: monitoring-vpc-vsi
      spec:
        endpoints:
        - interval: 15s
          port: http
          path: /metrics
          honorLabels: true
        jobLabel: k8s-app
        namespaceSelector:
          matchNames:
          - monitoring-vpc-vsi
        selector:
          matchLabels:
            k8s-app: vsi-<Name of VSI you used to provision>
      ```
      {: codeblock}

## Set up a custom Grafana dashboard
{: #step6-set-up-a-custom-grafana-dashboard}
{: step}

{{site.data.keyword.openshiftshort}} comes with a static Grafana dashboard by default.  The static dashboard cannot be easily extended to add new dashboards, alerts, or other features.  The following steps describe how to provision a Grafana instance by using the Grafana Operator and creating your own dashboards. For more information and for available installation options, see [Grafana Operator](https://operatorhub.io/operator/grafana-operator){: external}.

1. Install the Grafana Operator:
   1. Create a namespace to which you install the operator.
       ```
       oc new-project grafana
      ```
   1. From the web interface of your {{site.data.keyword.openshiftshort}} cluster switch to the *grafana* project.
   1. From the web interface, select **Operators -> OperatorHub** and find **Grafana Operator (community)**.
   1. Click **Continue** to accept the disclaimer and then click **Install**.
   1. Keep the configuration as is and make sure that the Installed Namespace is *grafana*.
1. Apply the following Grafana resource.  The settings are configurable.

   ```
   apiVersion: integreatly.org/v1alpha1
   kind: Grafana
   metadata:
     name: grafana
     namespace: grafana
   spec:
     dataStorage:
       accessModes:
         - ReadWriteOnce
       class: ibmc-vpc-block-metro-10iops-tier
       size: 10Gi
     config:
       auth:
         disable_signout_menu: false
       auth.anonymous:
         enabled: false
       log:
         level: warn
         mode: console
       security:
         admin_password: secret
         admin_user: root
     tolerations:
       - key: "logging-monitoring"
         operator: "Equal"
         value: "node"
         effect: "NoExecute"
     ingress:
       enabled: true
     dashboardLabelSelector:
       - matchExpressions:
           - key: app
             operator: In
             values:
               - grafana
    ```
    {: codeblock}

1. Wait for all pods in the Grafana namespace to be in *Running* state.
1. Log in to the Grafana instance. The default username and password is *root/secret*. After you are logged in, change the password. You can locate the route by using the following command:
   ```
   oc get route
   ```
1. [Add Grafana users](https://grafana.com/tutorials/create-users-and-teams/){: external}.
1. Connect Prometheus to Grafana.
   1. Grant the *cluster-monitoring-view* cluster role to the *grafana-serviceaccount* service account.
      ```
      oc adm policy add-cluster-role-to-user cluster-monitoring-view -z grafana-serviceaccount
      ```
   1. Create a bearer token for the *grafana-serviceaccount* service account.
      ```
      oc serviceaccounts get-token grafana-serviceaccount -n grafana
      ```
   1. Create a [GrafanaDataSource resource](https://github.com/integr8ly/grafana-operator/blob/v3.10.1/documentation/datasources.md){: external}. In the following YAML example, substitute <BEARER_TOKEN> with the output of the previous command.

      ```
      apiVersion: integreatly.org/v1alpha1
      kind: GrafanaDataSource
      metadata:
        name: prometheus-datasource
        namespace: grafana
      spec:
        datasources:
          - access: proxy
            editable: true
            isDefault: true
            jsonData:
              httpHeaderName1: 'Authorization'
              timeInterval: 5s
              tlsSkipVerify: true
            name: Prometheus
            secureJsonData:
              httpHeaderValue1: 'Bearer <BEARER_TOKEN>'
            type: prometheus
            url: 'https://thanos-querier.openshift-monitoring.svc.cluster.local:9091'
        name: prometheus-datasource.yaml
        ```
        {: codeblock}

        You can add another Prometheus for your {{site.data.keyword.openshiftshort}} cluster by including it in the data sources list. You must create a *grafana-serviceaccount* service account within the {{site.data.keyword.openshiftshort}} cluster that you want to connect to and grant the cluster role privileges. Then, you can generate a bearer token.
        {: note}

   2. Create [Grafana Dashboard resources](https://github.com/integr8ly/grafana-operator/blob/v3.10.1/documentation/dashboards.md){: external}. You can also import existing dashboards by using the JSON.

## Related controls in {{site.data.keyword.framework-fs_notm}}
{: #related-controls}

See the [related controls for operational monitoring](/docs/framework-financial-services?topic=framework-financial-services-shared-monitoring-operational).
