# Assignment 1: Deployment of a Cloud-Native K3s Cluster on AWS

**Submitted by:** Lindokuhle Lusiba  
**Student ID:** 221089926  
**Programme:** Advanced Diploma in IT (Communication Networks)  

---

## 1. Technical Overview

### 1.1 K3s in Modern Networking

K3s is a lightweight and fully certified Kubernetes distribution designed for environments with limited resources, such as edge computing in 5G and IoT systems. By removing unnecessary legacy features and optional cloud components, K3s maintains a very small footprint (under 100MB) while still complying with CNCF standards.

---

### 1.2 Architecture Design

The cluster was deployed using a **High Availability (HA) control plane** consisting of three master nodes. This ensures that the system continues running even if one node fails.

- **Control Plane**  
  The control plane manages the cluster by handling API requests, scheduling workloads, and maintaining the cluster state.

- **Embedded Database (etcd)**  
  Instead of using SQLite (default in basic K3s setups), this deployment uses an embedded etcd cluster to synchronize data across all master nodes.

- **Networking (Flannel CNI)**  
  Flannel provides a Layer 3 virtual network using VXLAN, allowing communication between pods across different EC2 instances.

- **Container Runtime (containerd)**  
  Containerd is used to run and manage containerized applications efficiently.

---

## 2. Infrastructure Specifications

The following AWS EC2 instances were provisioned to support the HA cluster:

| Component | Role           | Instance Type | Resources         | OS            |
|----------|----------------|--------------|-------------------|---------------|
| Master-1 | Initial Leader | t3.large     | 2 vCPU / 8GB RAM  | Ubuntu 22.04  |
| Master-2 | HA Peer        | t3.large     | 2 vCPU / 8GB RAM  | Ubuntu 22.04  |
| Master-3 | HA Peer        | t3.large     | 2 vCPU / 8GB RAM  | Ubuntu 22.04  |

---

## 3. Implementation Evidence

## Evidence of Deployment

### AWS EC2 Management Console
![AWS Console](img/aws_coonsole.png)<img width="1911" height="944" alt="aws_coonsole png" src="https://github.com/user-attachments/assets/5ae1a298-4e94-4628-93d0-02a11677f79d" />


### Kubernetes Nodes Status
![Nodes Status](img/nodes_status.png)<img width="950" height="948" alt="nodes_status png" src="https://github.com/user-attachments/assets/2de92309-d31b-4c1e-aa84-1f798d8e8577" />


### Kubernetes Pods Status
![Pods Status](img/pods_status.png)<img width="951" height="950" alt="pods_status png" src="https://github.com/user-attachments/assets/87d341e5-e360-4474-b739-8e5d0297c9f9" />

## 4. Reflection

### 4.1 Skills Gained

This project helped me apply theoretical knowledge in a practical environment. I gained a deeper understanding of cloud-native systems, especially the importance of networking in Kubernetes deployments, including VPC configuration, security groups, and node communication.

---

### 4.2 Challenges and Solutions

One issue I encountered was a **403 Forbidden error** when pushing to GitHub. This was caused by an incorrect remote repository configuration. Updating the Git remote to the correct repository resolved the problem.

Another issue occurred when nodes failed to join the cluster due to a timeout. After investigating, I found that **UDP port 8472** (used for VXLAN) was blocked in the AWS security group. Opening this port allowed the nodes to communicate, and their status changed to **Ready**.

---

### 4.3 K3s in 5G Environments

K3s is well-suited for 5G edge computing because it can run on low-resource hardware. Since 5G requires low latency, processing needs to happen closer to the user (at the edge). K3s allows deployment of containerized network functions without requiring large-scale infrastructure.


---

### 4.4 Scalability

Using AWS EC2 (virtualization) together with K3s (containerization) creates a scalable system. Virtual machines provide flexible resources, while containers allow fast deployment of applications. This makes it easy to scale the system horizontally by adding more nodes when demand increases.

---
