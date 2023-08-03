# k0smotron - An open source control plane manager for unified cluster management

k0smotron allows you to unify your Kubernetes cluster management for an efficient use of resources. It’s designed for k0s.

## Features

### Control Plane-as-a-Service

k0smotron streamlines k0s control plane creation and management within your management cluster, reducing traditional operational overhead. k0smotron encapsulates the control plane service as a pods (and other Kubernetes constructs) and provides an intuitive approach to cluster lifecycle management through ClusterAPI integration.

### Enhanced High Availability

k0smotron allows you to leverage the power of SQL databases as a data store for your control planes through Kine. This flexibility means you can choose from a variety of data storage operators like Postgres, MySQL, or your cloud provider's managed databases. This offers a robust, high-availability solution, eliminating the dependency on Etcd. 

### Bring Your Own Worker Nodes

k0smotron prioritizes flexibility in the integration of worker nodes and allows easy connection or creation of nodes. This ensures node isolation and flexible scaling, minimizing interference with the control plane. k0smotron operates with or without the ClusterAPI, offering you the freedom to select your preferred operational mode.

## How does it work

The k0smotron controller manager is a service that will be installed into an existing Kubernetes cluster. k0smotron will create and manage k0s control planes in the management cluster like workloads. It leverages the natural pattern of working with custom resources to manage the lifecycle of the k0s control planes. k0smotron will automatically create all the needed Kubernetes lower level resources, such as pods, configmaps, etc.

k0smotron is a service designed to manage the lifecycle of k0s control planes in a Kubernetes (any distro) cluster. By running the control plane on a k8s cluster we can enjoy and leverage the high availability and auto-healing functionalities of the management cluster.

![k0smotron-diagram-v2](images/k0smotron-diagram-v2.png)

## Use cases

### Development and CI/CD

In the process of continuous integration and end-to-end testing, a temporary Kubernetes Cluster is needed. With k0smotron, these clusters can be created on demand in a declarative way and thus integrated into the existing CI process with ease. This avoids cluster sprawl and long-living snowflake clusters.

### Edge Container Management

Kubernetes at the edge typically comes with the requirement of a low resource footprint. As a result, clusters with distributed roles are more challenging and a lot of single-node clusters are created. Managing the large number of clusters confronts us with almost impossible tasks. 

Offloading the control plane means that the persistence layer of the cluster can run on dedicated hardware and workloads can be managed at the edge on devices dedicated to their purposes. With k0smotron worker nodes get ephemeral.

### Multi-cloud Cluster LCM

A multi-cloud strategy is essential these days. But managed Kubernetes offerings are often different in versioning or even the built-in tooling.

With k0smotron, you can run the control plane management cluster in a public or private cloud provider of your choice and the worker nodes in different clouds. With this you have an approach to unified cluster management, providing one flavor on all clouds and saving the costs for the highly available Control Plane in the cloud.

## Getting Started

Getting started with k0smotron is easy. Simply install the controller into an existing cluster:

``` bash
kubectl apply -f https://docs.k0smotron.io/stable/install.yaml
```

Then create a k0s control plane:
``` bash
kubectl apply -f - <<EOF 
apiVersion: k0smotron.io/v1beta1 
kind: Cluster 
metadata: 
name: my-k0smotron 
spec: {} 
EOF	
```

That's it. You now have a k0s control plane running in your cluster. For further details how to work with the new cluster control plane check out the documentation.

## FAQ

### What is the relationship between k0smotron and Cluster API?

k0smotron already provides integration into Cluster API, can create Control Planes and act as a CAPI Bootstrap Provider for Worker Nodes. We are looking to expand k0smotron as a full Cluster API provider.

### How is k0smotron different from multi-cluster management solutions such as Tanzu, Rancher, etc.?

Most of the existing multi-cluster management solutions provision infrastructure for the control planes, in most cases VMs. In all of the cases we've looked at the worker plane infrastructure is also provisioned in the same infrastructure with the control plane and thus not allowing you to fully utilize the capabilities of the management cluster.

### How is this different to managed Kubernetes providers?

Control and Flexibility: k0smotron gives you full control over your cluster configurations within your Management cluster, offering unparalleled flexibility.
Bring Your Own Workers: Unlike managed Kubernetes providers, k0smotron allows you to connect worker nodes from any infrastructure, providing greater freedom and compatibility.
Cost Efficiency: By leveraging your existing Kubernetes cluster, k0smotron helps reduce costs associated with managing separate clusters or paying for additional resources.
Homogeneous Setup: k0smotron ensures a consistent configuration across clouds, simplifying maintenance and management tasks.