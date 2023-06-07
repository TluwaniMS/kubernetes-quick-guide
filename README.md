# kubernetes-quick-guide

### What is Kubernetes?

Kubernetes is a container orchestration engine that is open source, designed to automate the deployment, scaling, and management of applications running in containers.

![Kubernetes Components](https://github.com/TluwaniMS/kubernetes-quick-guide/blob/master/supporting-images/components-of-kubernetes.svg)

# Kubernetes Components:

* ### Cluster:

A collection of worker machines referred to as nodes, which execute applications in containerized form.

* ### Control Plane (Master Node):

The control plane serves as the container orchestration layer responsible for providing the API and interfaces necessary for defining, deploying, and managing the lifecycle of containers.

###### Control Plane Components:

#### i. kube-apiserver:

The Kubernetes control plane includes the API server, which serves as a component responsible for exposing the Kubernetes API. Serving as the front end for the Kubernetes control plane, the API server plays a crucial role.

#### ii. etcd:

The etcd serves as a reliable and resilient key-value store, providing consistency and high availability. It functions as the underlying storage system for all cluster data in Kubernetes.

#### iiii. kube-scheduler:

The Kube-scheduler actively monitors for recently created Pods that haven't been assigned a node yet, and then proceeds to choose a suitable node where these Pods can be executed.

#### iv. Controllers:

Controllers are control loops that monitor the condition of your cluster and subsequently initiate or request modifications as necessary. Each controller endeavors to bring the current state of the cluster closer to the desired state.

#### v. kube-controller-manager:

The kube-controller-manager operates the controller processes.

#### vi. cloud-controller-manager:

The cloud-controller-manager in Kubernetes integrates cloud-specific control logic. By utilizing the cloud controller manager, you can establish a connection between your cluster and your cloud provider's API. This approach separates the components that interact with the cloud platform from the components that solely interact with your cluster.

* ### Node (Worker Node):

In Kubernetes, a node refers to a machine that performs work tasks.

######	Node Components:

#### i. Kubelet:

A node-level agent deployed within the cluster, responsible for ensuring the proper execution of containers within a Pod.

#### ii. kube-proxy:

Each node in the cluster hosts kube-proxy, which serves as a network proxy.

#### iiii. Container-runtime:

The container runtime is the software tasked with executing containers.


# Basic Kubernetes Objects:

### What is a Kubernetes object?

In the Kubernetes system, a Kubernetes object refers to an entity used by the Kubernetes API to depict the state of your cluster. Typically, a Kubernetes object serves as a "record of intent," allowing you to convey your desired configuration for a specific aspect of your cluster's workload. By creating an object, you are essentially informing the Kubernetes system about your cluster's desired state for that particular workload component.

* ### Deployment:

A Kubernetes deployment serves as an API object responsible for overseeing a replicated application, usually achieved by running stateless Pods. Each replica is represented by a Pod, and these Pods are distributed across the cluster's nodes.

* ### Service:

Kubernetes services are employed to expose a network application running within your cluster as one or multiple Pods. A Service typically relies on a selector to determine the specific set of Pods it targets. As Pods are added or removed, the set of Pods matching the selector will dynamically adjust. The purpose of the Service is to ensure that network traffic can be efficiently directed to the current set of Pods handling the workload.

* ### ConfigMap:

A ConfigMap serves as an API entity utilized for storing non-sensitive data in the form of key-value pairs. ConfigMaps can be utilized by pods in various ways, including as environment variables, command-line arguments, or configuration files within a volume.

* ### Kubernetes Secrets:

Secrets serve a similar purpose to ConfigMaps, but their main focus is to securely store sensitive information.

* ### Namespaces:

In Kubernetes, namespaces serve as a means to segregate sets of resources within a cluster, facilitating the sharing of a Kubernetes cluster among various projects, teams, or customers. Resource names must be unique within a namespace but not across different namespaces.
