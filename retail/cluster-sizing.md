---
layout: default
title: Cluster Sizing
grand_parent: Patterns
parent: Retail
nav_order: 5
---
# OpenShift Cluster Sizing for the Retail Pattern

## Table of contents

{: .no_toc .text-delta }

1. TOC
{:toc}

## Tested Platforms

The **retail** pattern has been tested in the following Certified Cloud Providers.

{% comment %}supportmatrix-qe-start{% endcomment %}
| **Certified Cloud Providers** | 4.10 |
| :---- | :---- |
| Amazon Web Services|   :heavy_check_mark: |
| Microsoft Azure| |
| Google Cloud Platform| |
{% comment %}supportmatrix-qe-end{% endcomment %}

## General OpenShift Minimum Requirements

OpenShift 4 has the following minimum requirements for sizing of nodes:

* **Minimum 4 vCPU** (additional are strongly recommended).
* **Minimum 16 GB RAM** (additional memory is strongly recommended, especially if etcd is colocated on masters).
* **Minimum 40 GB** hard disk space for the file system containing /var/.
* **Minimum 1 GB** hard disk space for the file system containing /usr/local/bin/.

There are several applications that comprise the **retail** pattern.  In addition, the **retail** pattern also includes a number of supporting operators that are installed by **OpenShift GitOps** using ArgoCD.

### Retail Pattern OpenShift Datacenter HUB Cluster Size

The retail pattern has been tested with a defined set of specifically tested configurations that represent the most common combinations that Red Hat OpenShift Container Platform (OCP) customers are using or deploying for the x86_64 architecture.

The Datacenter HUB OpenShift Cluster is made up of the the following on the AWS deployment tested:

| Node Type | Number of nodes | Cloud Provider | Instance Type
| :---- | :----: | :---- | :----
| Master | 3 | Amazon Web Services | m5.xlarge
| Worker | 3 | Amazon Web Services | m5.xlarge

The Datacenter HUB OpenShift cluster needs to be a bit bigger than the Factory/Edge clusters because this is where the developers will be running pipelines to build and deploy the **Industrial Edge** pattern on the cluster.  The above cluster sizing is close to a **minimum** size for a Datacenter HUB cluster.  In the next few sections we take some snapshots of the cluster utilization while the **Industrial Edge** pattern is running.  Keep in mind that resources will have to be added as more developers are working building their applications.

### Retail Pattern OpenShift Store Edge Cluster Size

The OpenShift cluster is made of 3 Nodes combining Master/Workers for the Edge/Factory cluster.

| Node Type | Number of nodes | Cloud Provider | Instance Type
| :----: | :----: | :----: | :----:
| Master/Worker | 3 | Google Cloud | n1-standard-8
| Master/Worker | 3 | Amazon Cloud Services | m5.2xlarge
| Master/Worker | 3 | Microsoft Azure | Standard_D8s_v3

### AWS Instance Types

The **retail** pattern was tested with the highlighted AWS instances in **bold**.   The OpenShift installer will let you know if the instance type meets the minimum requirements for a cluster.

The message that the openshift installer will give you will be similar to this message

```text
INFO Credentials loaded from default AWS environment variables
FATAL failed to fetch Metadata: failed to load asset "Install Config": [controlPlane.platform.aws.type: Invalid value: "m4.large": instance type does not meet minimum resource requirements of 4 vCPUs, controlPlane.platform.aws.type: Invalid value: "m4.large": instance type does not meet minimum resource requirements of 16384 MiB Memory]
```

Below you can find a list of the AWS instance types that can be used to deploy the **retail** pattern.

| Instance type | Default vCPUs | Memory (GiB) | Datacenter | Factory/Edge
| :------: | :-----: | :-----: | :----: | :----:
| | | | 3x3 OCP Cluster | 3 Node OCP Cluster
| m4.xlarge   | 4  | 16 | N | N
| m4.2xlarge  | 8  | 32 | Y | Y
| m4.4xlarge  | 16 | 64 | Y | Y
| m4.10xlarge | 40 | 160 | Y | Y
| m4.16xlarge | 64 | 256 | Y | Y
| **m5.xlarge**   | 4  | 16 | Y | N
| m5.2xlarge  | 8  | 32 | Y | Y
| m5.4xlarge  | 16 | 64 | Y | Y
| m5.8xlarge  | 32 | 128 | Y | Y
| m5.12xlarge | 48 | 192 | Y | Y
| m5.16xlarge | 64 | 256 | Y | Y
| m5.24xlarge | 96 | 384 | Y | Y

The OpenShift cluster is made of 3 Masters and 3 Workers for the Datacenter and the Edge/Factory cluster are made of 3 Master/Worker nodes.  For the node sizes we used the **m5.xlarge** on AWS and this instance type met the minimum requirements to deploy the **retail** pattern successfully on the Datacenter hub.  On the Factory/Edge cluster we used the **m5.2xlarge** since the minimum cluster was comprised of 3 nodes.  .

To understand better what types of nodes you can use on other Cloud Providers we provide some of the details below.

### Azure Instance Types

The **retail** pattern was also deployed on Azure using the **Standard_D8s_v3** VM size.  Below is a table of different VM sizes available for Azure.  Keep in mind that due to limited access to Azure we only used the **Standard_D8s_v3** VM size.

The OpenShift cluster is made of 3 Master and 3 Workers for the Datacenter cluster.

The OpenShift cluster is made of 3 Nodes combining Master/Workers for the Edge/Factory cluster.

| Type |  Sizes | Description
| :---- | :---- | :----
| [General purpose](https://docs.microsoft.com/en-us/azure/virtual-machines/sizes-general) |B, Dsv3, Dv3, Dasv4, Dav4, DSv2, Dv2, Av2, DC, DCv2, Dv4, Dsv4, Ddv4, Ddsv4 | Balanced CPU-to-memory ratio. Ideal for testing and development, small to medium databases, and low to medium traffic web servers.
| [Compute optimized](https://docs.microsoft.com/en-us/azure/virtual-machines/sizes-compute) | F, Fs, Fsv2, FX | High CPU-to-memory ratio. Good for medium traffic web servers, network appliances, batch processes, and application servers.
| [Memory optimized](https://docs.microsoft.com/en-us/azure/virtual-machines/sizes-memory) | Esv3, Ev3, Easv4, Eav4, Ev4, Esv4, Edv4, Edsv4, Mv2, M, DSv2, Dv2 | High memory-to-CPU ratio. Great for relational database servers, medium to large caches, and in-memory analytics.
| [Storage optimized](https://docs.microsoft.com/en-us/azure/virtual-machines/sizes-storage) | Lsv2 | High disk throughput and IO ideal for Big Data, SQL, NoSQL databases, data warehousing and large transactional databases.
| [GPU](https://docs.microsoft.com/en-us/azure/virtual-machines/sizes-gpu) | NC, NCv2, NCv3, NCasT4_v3, ND, NDv2, NV, NVv3, NVv4 | Specialized virtual machines targeted for heavy graphic rendering and video editing, as well as model training and inferencing (ND) with deep learning. Available with single or multiple GPUs.
| [High performance compute](https://docs.microsoft.com/en-us/azure/virtual-machines/sizes-hpc) | HB, HBv2, HBv3, HC, H | Our fastest and most powerful CPU virtual machines with optional high-throughput network interfaces (RDMA).

For more information please refer to the [Azure VM Size Page](https://docs.microsoft.com/en-us/azure/virtual-machines/sizes).

### Google Cloud (GCP) Instance Types

The **retail** pattern was also deployed on GCP using the **n1-standard-8** VM size.  Below is a table of different VM sizes available for GCP.  Keep in mind that due to limited access to GCP we only used the **n1-standard-8** VM size.

The OpenShift cluster is made of 3 Master and 3 Workers for the Datacenter cluster.

The OpenShift cluster is made of 3 Nodes combining Master/Workers for the Edge/Factory cluster.

The following table provides VM recommendations for different workloads.

| **General purpose** | **Workload optimized**
| Cost-optimized | Balanced | Scale-out optimized |  Memory-optimized  |Compute-optimized |  Accelerator-optimized
| :---- | :---- | :---- |  :---- | :---- | :----
| E2 | N2, N2D, N1 |  T2D |  M2, M1 | C2 |  A2
Day-to-day computing at a lower cost | Balanced price/performance across a wide range of VM shapes | Best performance/cost for scale-out workloads | Ultra high-memory workloads | Ultra high performance for compute-intensive workloads | Optimized for high performance computing workloads

For more information please refer to the [GCP VM Size Page](https://cloud.google.com/compute/docs/machine-types).
