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

###### Responsibilities:

* Serves API
* Validation
* Authorization
* Authentication
* Admission Control
* Serialization
* Reads/Writes on `etcd`

#### ii. etcd:

The etcd serves as a reliable and resilient key-value store, providing consistency and high availability. It functions as the underlying storage system for all cluster data in Kubernetes.

`NB!` Kubernetes components exclusively interact with `etcd` via the kube-api-server.

#### iiii. kube-scheduler:

The Kube-scheduler actively monitors for recently created Pods that haven't been assigned a node yet, and then proceeds to choose a suitable node where these Pods can be executed.

`NB`

###### At any given time, only one scheduler can be active,the scheduler functions based on a leader election system.

`In the process of leader election, a group of candidates vies for the position of leader. Each candidate competes by declaring themselves as the potential leader. Eventually, one candidate emerges as the winner and assumes the role of the leader. After winning the election, the leader regularly sends "heartbeats" to reaffirm their status as the leader, while the other candidates periodically make fresh attempts to become the leader. This approach guarantees a swift identification of a new leader in case the current leader encounters any issues or fails.`

#### iv. Controllers:

Controllers are control loops that monitor the condition of your cluster and subsequently initiate or request modifications as necessary. Each controller endeavors to bring the current state of the cluster closer to the desired state.

`NB`

###### At any given time, only one controller can be active,the controller functions based on a leader election system.

`In the process of leader election, a group of candidates vies for the position of leader. Each candidate competes by declaring themselves as the potential leader. Eventually, one candidate emerges as the winner and assumes the role of the leader. After winning the election, the leader regularly sends "heartbeats" to reaffirm their status as the leader, while the other candidates periodically make fresh attempts to become the leader. This approach guarantees a swift identification of a new leader in case the current leader encounters any issues or fails.`

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

### The .yaml Kubernetes object file must include these mandatory fields.:

#### apiVersion:

The version of the Kubernetes API  you’re utilizing to generate the object.

#### Kind:

The type of object you wish to create.

#### metadata:

The provided information consists of data that serves the purpose of uniquely identifying an object. This includes a string of characters representing a name, a UID (unique identifier), and an optional namespace.

#### spec:

What state you desire for the object.

`NB!` Each Kubernetes object has a unique object spec format that varies and includes nested fields specific to that particular object.

* ### Deployment:

A Kubernetes deployment serves as an API object responsible for overseeing a replicated application, usually achieved by running stateless Pods. Each replica is represented by a Pod, and these Pods are distributed across the cluster's nodes.

###### example:

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80

```

* ### Service:

Kubernetes services are employed to expose a network application running within your cluster as one or multiple Pods. A Service typically relies on a selector to determine the specific set of Pods it targets. As Pods are added or removed, the set of Pods matching the selector will dynamically adjust. The purpose of the Service is to ensure that network traffic can be efficiently directed to the current set of Pods handling the workload.

###### example:

```
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    app.kubernetes.io/name: MyApp
  ports:
    - protocol: TCP
      port: 80
      targetPort: 9376
```

* ### ConfigMap:

A ConfigMap serves as an API entity utilized for storing non-sensitive data in the form of key-value pairs. ConfigMaps can be utilized by pods in various ways, including as environment variables, command-line arguments, or configuration files within a volume.

###### example:

```
apiVersion: v1
kind: ConfigMap
metadata:
  name: game-demo
data:
  player_initial_lives: "3"
  ui_properties_file_name: "user-interface.properties"
```

* ### Kubernetes Secrets:

Secrets serve a similar purpose to ConfigMaps, but their main focus is to securely store sensitive information.

###### example:

```
apiVersion: v1
kind: Secret
metadata:
  name: mysecret
type: Opaque
data:
  username: YWRtaW4=
  password: MWYyZDFlMmU2N2Rm
```

* ### Namespaces:

In Kubernetes, namespaces serve as a means to segregate sets of resources within a cluster, facilitating the sharing of a Kubernetes cluster among various projects, teams, or customers. Resource names must be unique within a namespace but not across different namespaces.

###### example:

```
apiVersion: v1
kind: Namespace
metadata:
  name: production
  labels:
    name: production
```


# Kubectl:

### What is Kubectl?

The Kubernetes command-line tool known as "kubectl" enables you to execute commands on Kubernetes clusters. With kubectl, you can deploy applications, examine and administer cluster resources, and access log information.

# Basic Kubectl commands:

* ### kubectl apply (-f FILENAME | -k DIRECTORY)

You can apply a configuration to a resource either by specifying a file name or providing input via stdin. If the resource doesn't already exist, it will be created during this process.

* ### kubectl get <resource> [Options]

Display one or many resources.
 
* ### kubectl describe (-f FILENAME | TYPE [NAME_PREFIX | -l label] | TYPE/NAME)

Show details of a specific resource or group of resources.

* ### kubectl logs [-f] [-p] (POD | TYPE/NAME) [-c CONTAINER]

If you wish to print the logs for a container within a pod or a specific resource, you have the option to include the container name if the pod contains multiple containers.

* ### kubectl exec (POD | TYPE/NAME) [-c CONTAINER] [flags] -- COMMAND [args...]

Execute a command in a container.

# Kubernetes Practice Project:

* [Basic Kubernetes Resource Config](https://github.com/TluwaniMS/kubernetes-objects-yaml-config-guide)
