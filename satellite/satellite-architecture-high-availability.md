---

copyright:
  years: 2020, 2024
lastupdated: "2024-11-19"

keywords: 

subcollection: framework-financial-services

---

{{site.data.keyword.attribute-definition-list}}

# High availability for {{site.data.keyword.satelliteshort}} reference architecture
{: #satellite-architecture-high-availability}

As described in [High availability (HA) overview](/docs/framework-financial-services?topic=framework-financial-services-shared-high-availability), HA is important for all cloud-based applications. Ready to learn more specifics about HA in the {{site.data.keyword.satelliteshort}} reference architecture?

## High availability in your {{site.data.keyword.satelliteshort}} location
{: #your-workloads}

For [HA in {{site.data.keyword.satellitelong_notm}}](/docs/satellite?topic=satellite-ha#satellite-ha-setup), you need to consider HA in three main areas within your {{site.data.keyword.satelliteshort}} location. For more information, see the following topics:

1. [{{site.data.keyword.satelliteshort}} control plane master](/docs/satellite?topic=satellite-ha#ha-control-plane-master)
2. [{{site.data.keyword.satelliteshort}} control plane worker nodes](/docs/satellite?topic=satellite-ha#ha-control-plane-worker)
3. [{{site.data.keyword.cloud_notm}} services that run in your {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-ha#ha-cloud-services) such as {{site.data.keyword.openshiftshort}}

In addition, you own all of the underlying infrastructure that the {{site.data.keyword.satelliteshort}} location components run on. Consider the following options to increase resiliency of that infrastructure:

* Add more compute hosts
* Add redundant power, network, and storage
* Isolate machines within one data center
* Spread hosts across physical locations

### Your workloads in {{site.data.keyword.openshiftshort}}
{: #your-workloads-multizone-scaling}

After you have your underlying infrastructure in place, begin deploying your workloads in your on-premises {{site.data.keyword.openshiftshort}} workload clusters. You can use the `cluster-autoscaler` add-on to scale the worker pools in your cluster automatically to increase or decrease the number of worker nodes in the worker pool based on the sizing needs of your scheduled workloads. The `cluster-autoscaler` add-on is based on the [Kubernetes Cluster-Autoscaler project](https://github.com/kubernetes/autoscaler/tree/master/cluster-autoscaler). For more information, see [Autoscaling clusters](/docs/openshift?topic=openshift-cluster-scaling-classic-vpc&interface=ui).

In addition, it is possible to autoscale your pods. For more information, see [Scaling apps](/docs/openshift?topic=openshift-update_app#app_scaling).

## HA for {{site.data.keyword.cloud_notm}} services
{: #ibm-cloud-services}

Your high availability strategy needs to consider all of the {{site.data.keyword.cloud_notm}} services in your deployment, including the services that run in {{site.data.keyword.cloud_notm}} outside of your {{site.data.keyword.satelliteshort}} location. For more information, see [High availability for {{site.data.keyword.cloud_notm}} services](/docs/framework-financial-services?topic=framework-financial-services-shared-high-availability#ibm-cloud-services).

## Related controls in {{site.data.keyword.framework-fs_notm}}
{: #related-controls}

For more information, see [related controls for high availability](/docs/framework-financial-services?topic=framework-financial-services-shared-high-availability#related-controls).

## Next steps
{: #next-steps}

- [Development processes and software integrity](/docs/framework-financial-services?topic=framework-financial-services-shared-development-processes)
