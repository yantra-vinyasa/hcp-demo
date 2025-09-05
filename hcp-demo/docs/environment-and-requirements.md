# 📝 Customer Environment and Requirements

This document outlines the customer's current infrastructure, environment, and the specific requirements for the Hosted Control Plane (HCP) deployment.

---

## 1. 🏢 Customer Profile

*   **Industry:** [e.g., Finance, Healthcare, Retail]
*   **Primary Business Goals:**
    *   [e.g., Modernize legacy applications]
    *   [e.g., Reduce infrastructure and operational costs]
    *   [e.g., Improve developer agility and self-service capabilities]

---

## 2. 👥 Stakeholders & Skillset

### Key Stakeholders
*   **Project Sponsor:** [e.g., Name, Title]
*   **Lead Architect:** [e.g., Name, Title]
*   **Platform Engineering Team:** [e.g., Team Name/Lead]
*   **Developer Community Lead:** [e.g., Name, Title]

### Team Skillset
*   **Kubernetes/OpenShift:** [e.g., Intermediate - Manages a few clusters]
*   **Automation (Ansible/Terraform):** [e.g., Advanced - Infrastructure as Code is standard practice]
*   **Networking:** [e.g., Beginner - Relies on a separate networking team]

---

## 3. 🖥️ Current Environment

### 🏗️ Infrastructure Overview
*   **Primary Infrastructure:** [e.g., On-premises data center, Public Cloud (AWS, Azure), Hybrid]
*   **Virtualization Platform:** [e.g., VMware vSphere, Red Hat Virtualization, None (Bare Metal)]
*   **Storage Solution:** [e.g., SAN (iSCSI/FC), NAS, Local Storage,Ceph]

### 🛠️ Hardware Details (for Bare Metal)
*   **Server Vendor/Model:** [e.g., Dell PowerEdge R740, HP ProLiant DL380]
*   **CPU/Memory/Storage Specs:** [e.g., 2x Intel Xeon Gold, 256GB RAM, 2x 1TB NVMe SSDs]
*   **Network Adapters:** [e.g., 2x 25GbE SFP28]

### 🌐 Networking
*   **Core Network Vendor:** [e.g., Cisco, Arista]
*   **Available Subnets/VLANs:** [Provide details on networking segments for management and workload traffic]
*   **External Load Balancer:** [e.g., F5 BIG-IP, NGINX Plus, None]
*   **Firewall and Proxy:** [Details on internet access, proxy servers, and firewall rules]

---

## 4. 🎯 Project Requirements for HCP

### 🏆 Primary Goals
*   **Key Driver for HCP:** [e.g., Reduce control plane overhead, fast cluster provisioning for dev teams]
*   **Number of Hosted Clusters:** [e.g., 10-15 clusters initially, scaling to 50+]
*   **Workload Types:** [e.g., CI/CD pipelines, stateless web applications, databases, VMs or AI Workloads]

### 🔧 Technical Requirements
*   **High Availability (HA):** [e.g., Must survive a single management cluster node failure]
*   **Disaster Recovery (DR):** [e.g., Strategy for management cluster backup and recovery]
*   **Security and Isolation Model:** [e.g., Shared Everything, Dedicated Request Serving]
*   **Authentication:** [e.g., Integration with Active Directory via LDAP/LDAPS]
*   **Monitoring and Logging:** [e.g., Export metrics to external Prometheus, forward logs to Splunk]

### 🔗 Integrations & Success Metrics

### Existing Toolchain & Integrations
*   **CI/CD:** [e.g., Jenkins, GitLab CI, GitHub Actions]
*   **GitOps Tool:** [e.g., Argo CD, Flux]
*   **Identity Provider:** [e.g., Active Directory, Okta]

### Success Metrics
*   **Goal:** [e.g., Reduce new cluster provisioning time from 2 weeks to under 1 hour.]
*   **Goal:** [e.g., Decrease infrastructure TCO by 30% by eliminating dedicated control planes.]
*   **Goal:** [e.g., Onboard 5 new development teams to the platform within the first quarter.]

---
