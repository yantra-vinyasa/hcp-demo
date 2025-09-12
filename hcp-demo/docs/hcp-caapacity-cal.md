# Hosted Control Plane (HCP) Capacity Calculation

## Overview

A Hub Cluster, when configured with sufficient compute, memory, and storage resources, can manage up to 250 Hosted Control Plane (HCP) clusters under standard operational conditions.

However, it's recommended to start with a more conservative number, typically under 100, and gradually increase it while continuously monitoring the hub's health and performance.

## Factors Influencing Capacity

The actual number of HCP clusters a Hub Cluster can manage is influenced by several factors, including:

*   **Hub Cluster Resources:** The resource capacity (CPU, memory, storage) of the hub cluster itself.
*   **Network and Storage Performance:** Network throughput and storage performance are critical.
*   **Managed Cluster Activity:** The nature and activity level of the managed clusters (e.g., lightweight vs. production-grade with high control plane activity).

## Real-World Examples

Here are some common architectural patterns:

*   **Centralized Model:** Using a single, dedicated management cluster to host control planes for a large number of clusters (e.g., 20-30).
*   **Distributed Model:** Deploying multiple hosting clusters to manage subsets of hosted clusters, often for geographic or organizational reasons.
*   **Ratio-Based Scaling:** Scaling infrastructure based on a fixed ratio of nodes to clusters
    *   3 dedicated infra nodes for less than 6 clusters.
    *   6 dedicated infra nodes for less than 12 clusters 

## Resource Requirements and Calculation

The following table provides a sample calculation for a 3-node Hub Cluster to illustrate potential bottlenecks. The actual numbers will vary based on your specific hardware and configuration.

| # | Resource | Requirement per Hosted Cluster | Raw Capacity | Total Usable Capacity (3 Nodes) | Max Number of Clusters | Bottleneck? |
|---|---|---|---|---|---|---|
| 1 | Pods 🌰 | 75-80 Pods | 1528 pods | 1,200 Pods (3x508-300) | 16 | Yes ✅ |
| 2 | CPU ⚙️ | 5 vCPUs | 384 vCPUs (3 nodes × 128 vCPUs) | ~307 vCPUs | ~61 | No ❌ |
| 3 | Memory 🧠 | 20 GB | 3,072 GB (3 nodes × 1024 GB) | ~2,976 GB | ~149 | No ❌ |
| 4 | Storage (etcd) 📦 | ~15 GB | 1,440 GB (3 nodes × 480 GB) | ~1,200 GB | ~80 | No ❌ |

## References

*   [Red Hat Documentation: Preparing to deploy Hosted Control Planes](https://docs.redhat.com/en/documentation/openshift_container_platform/4.18/html/hosted_control_planes/preparing-to-deploy-hosted-control-planes#hcp-sizing-calculation_hcp-sizing-guidance)
*   [Red Hat OpenShift Sizing Calculator](https://access.redhat.com/labs/ocpnc/)
