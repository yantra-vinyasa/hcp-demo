# ✅ Network and Storage Configuration Checklist

## 📋 Network Configuration Checklist

This checklist ensures your network is properly planned for performance, high availability, and critical features like live migration.

### Initial Setup & Host Networking

* `[ ]` **Determine Network Strategy**: Decide between using the default pod network (SDN) or secondary interfaces for connecting VMs to external networks (e.g., VLANs).
* `[ ]` **Use High Availability NICs**: Implement a **bonded pair** of physical NICs.
* `[ ]` **Select Bonding Mode**: Use a supported bonding mode like **active-backup (mode 1)** or **LACP (mode 4)**.
    * `[ ]` If using LACP, ensure it is configured on both the host and the upstream switches.
* `[ ]` **Use NMState Operator**: Configure all host networks (bridges, bonds) using the **NMState Operator**.
    * `[ ]` Consolidate the configuration into a single **NodeNetworkConfigurationPolicy (NNCP)** manifest to prevent race conditions.
* `[ ]` **Do Not Modify `br-ex`**: Avoid making changes to the default `br-ex` OVS bridge.

***

### VM Connectivity (Secondary Networks)

* `[ ]` **Choose Bridge Type**: Use **OVS bridges** over Linux bridges.
* `[ ]` **Use OVN-Kubernetes Localnet**: For connecting to external VLANs, use the **localnet topology** with an OVS bridge.
* `[ ]` **Create NetworkAttachmentDefinitions (NADs)**: Create a separate NAD for each VLAN.
    * `[ ]` Verify that the network name (`spec.config.name`) in the NAD exactly matches the localnet name in the `ovn.bridge-mappings`.
* `[ ]` **Configure Live Migration Network**: For heavy workloads, set up a **dedicated secondary network** for live migration.
    * `[ ]` Create a dedicated NAD for this purpose in the `openshift-cnv` namespace.

***

## 💽 Storage Configuration Checklist

This checklist covers the fundamental storage requirements for features like live migration, snapshots, and cloning.

### Core Requirements

* `[ ]` **Use a CSI Driver**: Always use a **Container Storage Interface (CSI) driver** from your storage vendor.
* `[ ]` **Verify RWX Support for Live Migration**: Confirm your CSI driver supports **ReadWriteMany (RWX)** access mode.
* `[ ]` **Configure a Default Storage Class**: Ensure a default storage class is configured for the cluster.
    * `[ ]` **(Recommended)** Annotate a storage class with `storageclass.kubevirt.io/is-default-virt-class: "true"` to make it the default for virtualization.
* `[ ]` **Customize Storage Profiles**: If needed, customize the **StorageProfile** to ensure correct settings for cloning and import operations.

***

### Data Protection & Advanced Features

* `[ ]` **Enable Snapshots & Clones**:
    * `[ ]` Ensure your storage provider supports the Kubernetes **Volume Snapshot API**.
    * `[ ]` Configure a `VolumeSnapshotClass`.

***

### Sizing & Performance

* `[ ]` **Plan Storage Sizing**: Calculate total storage needs for VMs and cluster services (logging, metrics).
* `[ ]` **Choose Storage for Performance**: For performance-sensitive workloads, use **block storage** rather than file-based storage.
* `[ ]` **Consult Tuning Guide**: Refer to the official **OpenShift Virtualization Tuning & Scaling guide**.