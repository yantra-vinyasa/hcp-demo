# Hosted Control Plane (HCP) Installation for `hcp-demo`

This directory contains the manifests required to deploy the `hcp-demo` OpenShift cluster using the Hosted Control Plane (HCP) agent-based installation method on bare-metal servers.

The installation is broken down into sequential steps, with each subdirectory representing a stage in the process.

## Deployment Steps

### 1. Prerequisite Resources

This step creates the essential resources needed to bootstrap the installation process. This includes the target namespace, secrets for pulling images and SSH access, a ConfigMap for the CA bundle, and the CAPI provider role.

- **Key Files:**
  - [`01-resources/ns.yaml`](./01-resources/ns.yaml): Creates the `hcp-demo` namespace.
  - [`01-resources/pull-secret.yaml`](./01-resources/pull-secret.yaml): Defines the pull secret. **Note:** This must be populated with a valid secret from Red Hat.
  - [`01-resources/sshkey.yaml`](./01-resources/sshkey.yaml): Contains the SSH public key for node access.
  - [`01-resources/cm-ca-bundle.yaml`](./01-resources/cm-ca-bundle.yaml): Provides a custom CA bundle for the cluster.
  - [`01-resources/capi-provider-role.yaml`](./01-resources/capi-provider-role.yaml): Defines a role for the CAPI provider.

### 2. Configure Infrastructure and Worker Node Environment

This stage defines the installation environment for the bare-metal nodes that will act as the infrastructure and worker nodes.

- **Key Files:**
  - [`02-infraenv-worker-iso/infraenv.yaml`](./02-infraenv-worker-iso/infraenv.yaml): The `InfraEnv` resource specifies the OS image, proxy, NTP, and networking details for the nodes.
  - [`02-infraenv-worker-iso/`](./02-infraenv-worker-iso/): Contains `NMStateConfig` manifests that define the static network configuration for each bare-metal worker node.

### 3. Define and Create the Hosted Cluster

This is the core step where the Hosted Control Plane cluster and its associated node pools are defined.

- **Key Files:**
  - [`03-hcp/hostedcluster.yaml`](./03-hcp/hostedcluster.yaml): The primary `HostedCluster` custom resource. It defines the cluster's network, domain, and release version.
  - [`03-hcp/nodepool-worker.yaml`](./03-hcp/nodepool-worker.yaml): Defines the `NodePool` for the three worker nodes.

### 4. Configure MetalLB for External Services

After the cluster is provisioned, MetalLB is installed and configured to provide LoadBalancer services, which is necessary for exposing services like the Ingress Controller on a bare-metal environment.

- **Key Files:**
  - [`04-hcp-metallb-ingress/sub.yaml`](./04-hcp-metallb-ingress/sub.yaml): Subscribes to and installs the MetalLB Operator.
  - [`04-hcp-metallb-ingress/metal-cr.yaml`](./04-hcp-metallb-ingress/metal-cr.yaml): Creates the MetalLB instance.
  - [`04-hcp-metallb-ingress/layer2.yaml`](./04-hcp-metallb-ingress/layer2.yaml): Configures the IP address pool for MetalLB.
  - [`04-hcp-metallb-ingress/ingress.yaml`](./04-hcp-metallb-ingress/ingress.yaml): Exposes the default Ingress Controller using a `LoadBalancer` service.

## External Documentation

For more detailed information on the concepts and resources used here, refer to the official OpenShift documentation:

- [Installing a Hosted Control Plane with agent-based installers](https://docs.redhat.com/en/documentation/openshift_container_platform/4.19/html-single/hosted_control_planes/index)
- [Helm Chart for Hypershift](https://github.com/loganmc10/hypershift-helm)
- [Hypershift Baremetal Lab](https://labs.sysdeseng.com/hypershift-baremetal-lab/4.18/introduction.html)
- [Firewall requirements for bare metal](https://docs.redhat.com/en/documentation/red_hat_advanced_cluster_management_for_kubernetes/2.11/html-single/clusters/index#firewall-port-reqs-bare-metal)
