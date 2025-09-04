# ☸️ OpenShift on OpenShift Using Hosted Control Planes (HCP)

This document provides an overview of the solution pattern for deploying Red Hat OpenShift as a hosted cluster using Hosted Control Planes (HCP). This architecture decouples the control plane from the worker nodes, allowing a central management/hub cluster to host the control planes for multiple, separate managed clusters. This model increases the efficiency of OpenShift deployments, reduces physical resource costs.

---
## 🔑 Key Concepts

Hosted Control Planes (HyperShift) fundamentally change the OpenShift architecture:
* **Standalone OpenShift**: A traditional cluster requires a minimum of 3 dedicated control plane nodes and 2 worker nodes. The control plane and workers are tightly bundled.
* **Hosted Control Planes (HyperShift)**: A central **Management Cluster** hosts the control planes for multiple clusters as pods in a namespace. The managed clusters consist only of worker nodes, which can be located in different data centers or infrastructure environments.

---
## 🚀 Value Proposition

Deploying OpenShift with Hosted Control Planes offers several advantages:

* **Reduction of Physical Resources**: Eliminates the need for dedicated control plane hardware for each cluster.
* **Ease of Cluster Administration**: New clusters can be provisioned for end-users or developers in minutes and removed when no longer needed. Control planes can also be updated independently of their worker nodes.
* **Secure Multi-Tenancy**: Provides a truer form of multi-tenancy by dedicating entire managed clusters to specific projects or users, ensuring workloads are completely separated.
* **Secure Network Segregation**: The control planes can reside on the management cluster's network domain while worker nodes reside on a different physical network, ensuring management traffic is physically separated from the data plane.
* **Distributed Clusters for Disaster Recovery**: Worker node pools for a single cluster can be distributed across different data centers. If one location fails, application pods can be relaunched on worker nodes in a different location.

---
## 📖 Key Terminology

* **Management / Hosting Cluster**: An OpenShift cluster where the HyperShift Operator is deployed and where the control planes for hosted clusters run.
* **Hosted Control Plane**: An OpenShift control plane (etcd, Kubernetes API server, etc.) that runs as a workload on the management cluster.
* **Hosted Cluster**: The complete OpenShift cluster, which includes its control plane (running on the management cluster) and its corresponding data plane (worker nodes).
* **Node Pool**: A set of compute (worker) nodes associated with a hosted cluster that run applications and workloads.

---
## ✅ Deployment Prerequisites Checklist

Before deploying, ensure the following requirements are met:
- [ ] A highly available OpenShift 4.18 cluster to serve as the management/hub cluster. Single-node OpenShift is not supported for this role.
- [ ] The following operators are installed from the OpenShift OperatorHub on the management cluster:
    - [ ] Advanced Cluster Management for Kubernetes
    - [ ] Multicluster Engine for Kubernetes (installed as part of ACM)
    - [ ] Infrastructure Operator for Red Hat OpenShift (for bare metal workers)
    - [ ] OpenShift Virtualization (for virtual NodePool/workers)
- [ ] Access to a valid pull secret for deploying OpenShift.
- [ ] The Hosted Control Planes feature is enabled on the management cluster.

---
## ⚙️ Deployment Process Overview

### 1. 🖥️ Bare Metal Hosted Cluster

* Use the Infrastructure Operator to prepare resources for the agent-based installer.
* Create a namespace on the management cluster for the hosted cluster.
* Create a pull-secret and an infrastructure environment YAML, which includes an SSH key for worker node access.
* Use the generated discovery ISO to boot the bare metal worker nodes.
* Once the nodes appear, configure their hostname, install disk, and role.
* Set the `nodePool` count for the hosted cluster to the number of prepared worker nodes to begin the deployment.
* Monitor the control plane pods in the cluster's namespace on the management cluster. Once online, use the extracted `kubeconfig` to connect and check the status of the worker nodes.

### 2. 💻 Virtual Hosted Cluster

* The process for virtual clusters is less intensive.
* Ensure the OpenShift Virtualization operator and a default storage class are configured.
* Create the cluster using the Hypershift CLI, specifying `kubevirt` as the cluster type.


---
## 🏗️ Architectural Considerations

### 💾 Storage for `etcd`
The performance of `etcd` is critical. The following storage backends are options:
* **LVM**: **Recommended for production**. Offers near-native performance and is suitable for high-demand, latency-sensitive applications.


### 🌐 External Load Balancer
For integrating an external load balancer like F5 with HCP, the `NodePort` service type can be utilized. The hosted cluster's API endpoint, formatted as `api.${HOSTED_CLUSTER_NAME}.${BASEDOMAIN}`, should be configured to resolve to the load balancer's IP address. The load balancer, in turn, will distribute incoming traffic to the `NodePort` across the management cluster nodes.

### 🛡️ High Availability and Disaster Recovery (HA/DR)
The decoupled architecture provides resilience against various failures:
* **Loss of a management cluster worker**: The hosted control plane's API and data plane remain available. The impacted control plane member is rescheduled.
* **Loss of management cluster control plane**: The hosted control plane's API and data plane remain available. However, new hosted clusters cannot be created or destroyed until it is recovered.
* 🔥 **Loss of entire management cluster**: The hosted cluster's data plane remains available, but its API will be unavailable.


### 📊 Performance and Observability
* The HyperShift Operator creates monitoring dashboards and `ServiceMonitor` resources for each hosted cluster, allowing Prometheus to gather metrics.
* To manage the resource impact on the monitoring stack, you can configure one of three metric sets:
    * **Telemetry**: The default and smallest set of metrics.
    * **SRE**: Includes metrics needed for alerts and troubleshooting.
    * **All**: Includes all metrics produced by a standalone OpenShift control plane.

### 🔧 Day 2 Post Deployment
*  For Day 2 operations and post-deployment configurations, you can refer to the policies and examples in the [rhacm-policies repository](https://github.com/yantra-vinyasa/rhacm-policies).
