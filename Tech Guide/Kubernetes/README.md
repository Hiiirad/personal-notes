# Kubernetes | K8s

- [Kubernetes | K8s](#kubernetes--k8s)
  - [Session 00 (Basic Info)](#session-00-basic-info)
    - [Introduction](#introduction)
  - [Session 01 (K8S Concepts and Keywords)](#session-01-k8s-concepts-and-keywords)
  - [Session 02 (Installation)](#session-02-installation)
  - [Session 03 (YAML Intro + Pod)](#session-03-yaml-intro--pod)
    - [YAML Introduction](#yaml-introduction)
    - [POD](#pod)
  - [Session 04 (Extra Concepts for POD)](#session-04-extra-concepts-for-pod)
  - [Session 05 (POD + Services Intro)](#session-05-pod--services-intro)
    - [Services](#services)
      - [ClusterIP](#clusterip)
      - [Node Port](#node-port)
      - [Load Balancer](#load-balancer)
  - [Session 06 (Services)](#session-06-services)
      - [Service without Selector](#service-without-selector)
      - [ExternalName](#externalname)
  - [Session 07 (StaticPodPath + Context)](#session-07-staticpodpath--context)
    - [Static Pod Path](#static-pod-path)
    - [Context](#context)
  - [Session 08 (ReplicaSet + Advance Labeling + Replication Controller)](#session-08-replicaset--advance-labeling--replication-controller)
    - [ReplicaSet](#replicaset)
    - [Advanced Scheduling \& Session/Node Affinity](#advanced-scheduling--sessionnode-affinity)

## Session 00 (Basic Info)

Exams (66 is Pass):
- CKAD (Certified Kubernetes Application Developer)
- CKA (Certified Kubernetes Administrator)
- CKS (Certified Kubernetes Security Specialist)

Helm is not coming in exam.

Zero time upgrade!

Config and support of microservices is hard and frustrating.

Expanding The Infrastructure Resources:
- Scale Up: Increase Resources -> Vertical
- Scale Out: Increase Number of Instance or Servers -> Horizontal

Distributed Tracing System:
  - Jaeger
  - OpenTracing
  - Zipkin

Container Runtime:
- Namespace:
  - PID
  - UTC
  - User
- CGroup
  - Limitation
  - Memory
  - CPU
- LXC -> Manage CGroup and Namespace

Kubernetes used for stateless containers/softwares

Moby Project (Before Docker) -> Depends on LXC -> In the end changed to containerd

Everything changed to container orchestration -> The most popular: Kubernetes

- Cluster: 1 master + N worker node
- HA on cluster needs more masters

Kubernetes: Data Center OS

Kubernetes starts on 2007 from Google -> 2014 Public Publish

Borg (Proprietary) + Omega (Proprietary) -> Kubernetes (Open-Source)

Semantic Versioning for Kubernetes: X.Y.Z

### Introduction

[What is Kubernetes and What is not](https://kubernetes.io/docs/concepts/overview/what-is-kubernetes/)

[Kubernetes Components](https://kubernetes.io/docs/concepts/overview/components/)

- Master Node
  - API Server (Brian of k8s) -> The only way to connect and control k8s
  - Scheduler -> Control what container/pod goes to which node based on its algorithm to choose the best node
  - etcd (Heart of k8s) -> Distributed Database -> Every change will log in etcd -> need HA on etcd or get backup from it
  - Controller Manager (Binary file) -> Checks every changes or current state (Desired state / Current state) of everything on cluster level, report it to API server (Reconciliation Loop)
  - Cloud Controller Manager -> only on cloud solution for better work
  - CoreDNS ->  Uses for service discovery
- Worker Node
  - Container Runtime Interface (CRI) (only talks to kubelet) -> docker / containerd / CRI-O / coreos on rocket
  - Kubelet -> Agent of kubernetes on worker nodes to talk to master node
  - kube-proxy -> Update IPtables of the system. Ports and everything

All connections between different part of k8s are based on MTLS (Mutual-TLS).

Split brain on cluster concept -> Must be an odd number

No one install the hardway (from source)

Some Useful Ports:
- API Server: 6443
- etcd: 2379

Config file of kubectl: `~/.kube/config`

---

## Session 01 (K8S Concepts and Keywords)

Atomic Units of Scheduling:
- VM
- Container
- Pod

Every pod can contain more than 1 containers.

Every pod contains 1 container, unless you need a **supporting container** in one pod. If you need more than one instance of a container, you can put each of them in a different pod.

Multi-Container Design Patterns of pods (multi container pod):
- Sidecar -> It helps the main container (Helper: An extra container in your pod to enhance or extend the functionality of the main container.)
- Adapter -> Good for logging and monitoring systems -> A container that transform output of the main container.
- Ambassador -> A container that proxy the network connection to the main container.
- [Supporting Link 1 of Patterns for Composite Containers](https://kubernetes.io/blog/2015/06/the-distributed-system-toolkit-patterns/)
- [Supporting Link 2 of Designing Distributed Systems](https://www.vittorionardone.it/en/2021/02/16/pattern-sidecars-ambassadors-and-adapters-containers/)
- Pod doesn't service/work until all the containers in the pod are up and running.

Every container MUST do a single task.

[12 Factors of Application Architecture](https://12factor.net/)

[DDD or Domain Driven Design](https://khalilstemmler.com/articles/)

Every pod has only a single IP even if it's a multi container pod.

Pets vs Cattle:
- Pets: Old structure of VM (Every pet has their names) (If one of their servers went down, the whole system went down)
- Cattle: (Microservice Orchestration/Architecture) (If one of the containers went down, the whole system still works)

Deployment (Scaling, updates without downtime, and rollbacks) -> Pod -> Container (Application Code)
- Deployment is a very important component because of the self-healing
- Pod wrapped inside a deployment
- Deployment can have more than one pod depends of the architecture and needs

Service: The IP of pods doesn't matter. The label of them is more important than IP or Port. Even if the pod destroyed, the labels will still be the same for the service. The service exposes the pod to the internet.

Pause Container:
- The service that holds container's IP while new container recreates.
- When a pod created, a pause container automatically creates and the pause container is very important. It holds namespaces of a container inside a pod. If it destroys the container will destroy.
- Pause container is like the parent of containers.

CNI (Container Network Interface): Instead of Routing Table of the OS. It helps inter node communication. Cluster Networking have some of network plugins to handle networking. (Weave-net, Calico and Flannel are more famous than the others)

---

## Session 02 (Installation)

Don't use docker export -> It removes entrypoint and compress all layers to one single layer.

- `docker image save CONTAINER:TAG > NAME.tar`
- `docker image load -i NAME.tar` -> all of them save in `/var/lib/docker`

Tools to Install K8s:
- Kubeadm -> Install K8s Cluster (Recommended by Kubernetes)
- kOps / Kubernetes Operations -> use for clouds (AWS and ...). It doesn't use for on-premise
- Kubespray -> Install K8s with Ansible on GCE, Azure, OpenStack, AWS, vSphere, Packet (bare metal), Oracle Cloud Infrastructure (Experimental) or Baremetal.

Kubernetes Cluster Federation (IMPORTANT for Experts):
- Project [Kubefed](https://github.com/kubernetes-sigs/kubefed)
- [Kuberentes Federation Evolution](https://kubernetes.io/blog/2018/12/12/kubernetes-federation-evolution/)

Check out Netflix migrate to Microservice [YouTube](https://www.youtube.com/watch?v=CZ3wIuvmHeM)

Installing K8s using [Kubeadm](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/):

1. Update the OS and all the related packages.
2. Make sure all the hostname, MAC addresses, and product_uuid on every node are unique. [Link](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/#verify-mac-address)
3. Check your firewall and make sure some ports related to K8s are open for all nodes. [Link](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/#check-required-ports)
4. Swap on all nodes must be disabled on all nodes. (The idea of kubernetes is to tightly pack instances to as close to 100% utilized as possible. All deployments should be pinned with CPU/memory limits. So if the scheduler sends a pod to a machine it should never use swap at all. You don't want to swap since it'll slow things down.)
   ```bash
   # First you need to disable swap
   swapoff -a
   # Then disable swap on startup in /etc/fstab
   sed -i '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab
   ```
5. Configure iptables to see bridged traffic on all nodes. This must be configured for the CNI. [Link](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/#letting-iptables-see-bridged-traffic)
6. Install CRI (Container Runtime Interface) on all nodes -> The Container Runtime is the software that is responsible for running containers.
7. If you already installed docker on the OS, `containerd` installed with it. If you don't specify a runtime, `kubeadm` automatically tries to detect an installed container runtime by scanning through a list of well known Unix domain sockets. If both Docker and `containerd` are detected, Docker takes precedence. This is needed because Docker 18.09 ships with `containerd` and both are detectable even if you only installed Docker. If any other two or more runtimes are detected, `kubeadm` exits with an error.
8. Install `kubeadm`, `kubelet`, and `kubectl` on all nodes. [Link](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/#installing-kubeadm-kubelet-and-kubectl)
9. Configuring a cgroup driver is important because both CRI and containerd have a property called "cgroup driver". Configure your "cgroup driver" based on your CRI. [Link](https://kubernetes.io/docs/setup/production-environment/container-runtimes/)
10. You can restart all the servers if you want :)
11. Let's create cluster with kubeadm. [Kubeadm Commands](https://kubernetes.io/docs/reference/setup-tools/kubeadm/)
12. It is recommended to run `kubeadm config images pull` prior to `kubeadm init` to verify connectivity to the [Google Container Registry](https://gcr.io) container image registry. You can also get the list of the packages with `kubeadm config images list` command.
13. Now we need to initialize the control-plane node (MASTER NODE). [Link](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/#initializing-your-control-plane-node). Based on what you want to do, you need to fill the arguments of the command below with care:
    ```bash
    kubeadm init <args>
    # Set your k8s network IPs based on IP
    # that they're not in any range in your network.
    # K8s default IP range: 192.168.0.0/16
    kubeadm init --pod-network-cidr=M.N.O.P/Q --kubernetes-version=X.Y.Z --v=NUM
    kubeadm init --pod-network-cidr=10.20.0.0/12 --kubernetes-version=1.19.3
    kubeadm init --kubernetes-version=1.19.3
    # Bigger IP range is better for production
    # --v means verbose and it starts from 5
    # bigger number is more verbosity and 8 is more efficient than others
    ```
14. After a few minutes, the output of `kubeadm init <args>` command shows you the rest of the commands to complete the process:
    ```bash
    mkdir -p $HOME/.kube
    sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
    sudo chown $(id -u):$(id -g) $HOME/.kube/config
    # If you logged in as root user already, you can run the command below
    export KUBECONFIG=/etc/kubernetes/admin.conf
    ```
15. This is the important for for installing the CNI (Container Network Interface) and you have to choose between these CNIs: [List of CNIs](https://kubernetes.io/docs/concepts/cluster-administration/networking/)
16. I chose the [Calico](https://docs.projectcalico.org/about/about-calico) for my cluster.
17. [Read the documents](https://docs.projectcalico.org/getting-started/kubernetes/self-managed-onprem/onpremises) and chose the configuration fit for your environment. REMEMBER, if you change the default IP range of the K8s, you have to change the IP pool of the CNI in the configuration file too. Finally, you run the command below:
    ```bash
    kubectl apply -f calico.yaml
    ```
18. At this stage, the installation of your control-plane node finished. You can check the status of your cluster with the command below:
    ```bash
    kubectl get nodes
    # or
    kubectl get nodes -o wide
    ```
19. After CNI's installation finished, you need to join other nodes to your cluster. There are many ways to finish this step:
    1.  Show the valid tokens using `kubeadm token list` command, and choose between the token.
    2.  Remove the token generated when you executed `kubeadm init <args>` with the `kubeadm token delete TOKEN` command and generate a new one using `kubeadm token create --print-join-command --ttl=15m` command. (If you use a token, it removes from the list, but you can access it from the etcd)
20. Tokens are valid for 24h, but 15 min is for best practice.
21. After you get the command to join other nodes to your cluster, using `kubeadm join IP:6443 --token=TOKEN --discovery-token-ca-cert-hash sha256:HASH` command, you call it a day :)

There are some important points I should add:
- If the config file from `~/.kube` deleted, you can assign a different config file using `kubectl --kubeconfig=/ADDRESS/OF/FILE` command.
- The file `/etc/kubernetes/pki/ca.crt` is a CA Authority from Master Node and the ca validates every connections/requests.
- How to reset everything (remove cluster) from a specific node:
  - `kubeadm reset`
  - The reset process does not clean CNI configuration. So, you must remove `/etc/cni/net.d/*`, reset iptables manually using `iptables -F` command, remove `$HOME/.kube/config/*`, remove `/etc/kubernetes/kubelet.conf`, and remove `/etc/kubernetes/pki/*`
- For better managing your cluster, It's recommended to install auto completion for kubectl:
  - Install bash-completion for the OS.
  - Install Kubectl autocomplete. [Link](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)
- If you label nodes, you can query on those objects easily. [Understanding Kubernetes Objects](https://kubernetes.io/docs/concepts/overview/working-with-objects/kubernetes-objects/)
  - Roles on `kubectl get nodes` are just labels and you can check them out using `kubectl get nodes --show-labels` command, or you can query on them with `kubectl get nodes -l KEY=VALUE` command.
  - You can also assign labels using `kubectl label nodes NAME KEY=VALUE` command. For instance, `kubectl label nodes NAME node-role.kubernetes.io/worker=` or `kubectl label nodes NAME node-role.kubernetes.io/worker=worker`
  - You can also remove a label from a node using `kubectl label nodes NODE KEY-` command.
  - Replace/Overwrite a label that already exists using `kubectl label nodes NODE KEY=VALUE --overwrite` command.

Different API on API Server:
1. Storage
2. Network
3. Pod-Namespace
4. Deployment
5. ...

- API-Resources: `kubectl api-resources | less` -> empty APIGROUP considered as default group named `v1` -> tells you what you can create
- API-Version: `kubectl api-versions | less`
- Create a manifest file (declarative mode) (Better for technical in production) (Imperative mode is for the exam or get a YAML file):
  - Requirements
  - Rules
  - Base configuration
  - (WHAT WE NEED/WANT)

---

## Session 03 (YAML Intro + Pod)

Useful Tools:
1. [Kaniko](https://github.com/GoogleContainerTools/kaniko) is a tool to build container images from a Dockerfile, inside a container or Kubernetes cluster. Kaniko doesn't depend on a Docker daemon and executes each command within a Dockerfile completely in userspace. This enables building container images in environments that can't easily or securely run a Docker daemon, such as a standard Kubernetes cluster.
2. [Podman](https://github.com/containers/podman) is a tool for managing OCI containers and pods. It's a binary file that doesn't need linux services to run. The Podman approach is simply to directly interact with the image registry, with the container and image storage, and with the Linux kernel through the runC container runtime process (not a daemon). It's a wrapper on lxc.
3. [Buildah](https://github.com/containers/buildah) is a tool that facilitates building, pushing, and signing OCI images.
4. [Skopeo](https://github.com/containers/skopeo) works with remote images registries, retrieving information, images, signing content copying, inspecting, and etc. It does not require the user to be running as root to do most of its operations. It does not require a daemon to be running to perform its operations.
5. If your company still using K8s version older than v1.20, you need to check Dockershim and this [link](https://kubernetes.io/docs/tasks/administer-cluster/migrating-from-dockershim/check-if-dockershim-deprecation-affects-you/) to check whether the deprecation of Dockershim will affect you or not.
6. It is really important to know about Container Runtime if you want to know how they work in detail. I recommend you to read about different runtimes and understand the difference between high level runtime (Containerd) and low level runtime (runC). Check these [Link 1](https://www.ianlewis.org/en/container-runtimes-part-1-introduction-container-r) & [Link 2](https://www.ianlewis.org/en/container-runtimes-part-2-anatomy-low-level-contai) & [Link 3](https://www.ianlewis.org/en/container-runtimes-part-3-high-level-runtimes).

### YAML Introduction

YAML:
- List
- Dictionary
- Key:Value

List:
```yaml
Fruits:
  - apple
  - banana
```

Dictionary:
```yaml
Person:
  name: hirad
  age: 27
  height: 180
```

Key:Value
```yaml
- hirad:
  name: Hirad Rasoolinejad
  job: DevOps
  skills:
    - linux
    - shell
- ali:
  name: Ali Fazeli
  job: Data Scientist
  skills:
    - python
    - big data
    - AI
```

Multi line stream:
```yaml
include_newlines: |
          hi how are you?
          these lines appear in 
          three lines
fold_newlines: >
          hello
          you will see these lines
          in only one line
```

### POD

Manifest files of K8s can be `.yml` or `.yaml` or `.json`.

What do we need in creating a pod:
1. Name
2. Image

Dockerfile Important Keywords and their differences:
- CMD: ping -> pass a command to CMD -> 4.2.2.4 -> FAILED! It replaced on whole CMD functions.
- Entrypoint: ping -> pass a command to entrypoint -> 4.2.2.4 -> pinging 4.2.2.4 because it will be an argument to entrypoint.

Apply a [manifest](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.19/#podspec-v1-core): `kubectl apply -f first-pod.yml`

```yml
apiVersion: v1
kind: Pod
metadata:
  name: pod-example
spec:
  containers:
  - name: nginx-test
    image: nginx
```
- Pod names can only be in lowercase alphanumeric.

Simple commands for pod:
- Delete a pod: `kubectl delete -f first-pod.yml`
- Debug: `kubectl describe pod NAME`
- Logs:
  - `kubectl logs POD_NAME -c CONTAINER_NAME`
  - `kubectl logs -f POD_NAME -c CONTAINER_NAME` -> Follow New Logs
- Exec Single Command:
  - `kubectl exec POD_NAME -c CONTAINER_NAME -- COMMAND`
    - `kubectl exec -it POD_NAME -c CONTAINER_NAME -- bash`
  - `--` means the next argument is not an argument from the kubectl command.
  - Example from the GREP command:
    - `grep - FILE` -> OK!
    - `grep -- FILE` -> ERROR!
    - `grep -- -- FILE` -> OK!
- Watch: `kubectl get pods --watch`

Pause Container:
- Pause containers are a pod implementation detail where one pause container is used for each pod and shouldn't be shown when listing containers that are members of pods. Pause containers hold the cgroups, reservations, and namespaces of a pod before its individual containers are created.
- Pause containers contain network namespace. After a container exited/killed/failed, the IP of that container remains the same until the pause container destroyed. We have pause container for every container! And it comes up before each container.

Every pod has a lifecycle:
1. `Manifest -> Pending ------OK------> Running -> Succeeded`
2. `Manifest -> Pending ----Not OK----> Failed`
3. `Manifest -> Pending ----Running---> Not OK  -> Failed`
- Pending: Scheduler couldn't find a proper/specified node to deploy pod(s) on it.

Namespaces:
- Namespaces are Kubernetes objects which partition a single Kubernetes cluster into multiple virtual clusters. Each Kubernetes namespace provides the scope for Kubernetes Names it contains; which means that using the combination of an object name and a Namespace, each object gets an unique identity across the cluster.
- List of Namespaces:
  - `default`: The default namespace for objects with no other namespace
  - `kube-system`: The namespace for objects created by the Kubernetes system
  - `kube-public`: This namespace is created automatically and is readable by all users (including those not authenticated). This namespace is mostly reserved for cluster usage, in case that some resources should be visible and readable publicly throughout the whole cluster. The public aspect of this namespace is only a convention, not a requirement.
  - `kube-node-lease`: This namespace for the lease objects associated with each node which improves the performance of the node heartbeats as the cluster scales.
- List of namespaces:
  - `kubectl get pods --all-namespaces`
  - `kubectl get namespace`
  - `kubectl get ns`
- Not All Objects are in a Namespace:
  - Most Kubernetes resources (e.g. pods, services, replication controllers, and others) are in some namespaces. However namespace resources are not themselves in a namespace. And low-level resources, such as nodes and persistentVolumes, are not in any namespace.
  - To see which Kubernetes resources are and aren't in a namespace:
    ```bash
    # In a namespace
    kubectl api-resources --namespaced=true
    # Not in a namespace
    kubectl api-resources --namespaced=false
    ```
- Command to delete and inspect pods with namespaces:
  - Delete all pods: `kubectl delete pod --all`
  - Delete all pods with their namespaces: `kubectl delete pod --a --all-namespaces`
  - Delete namespace (if the namespace is in-use, the whole network with that namespace will be down!): `kubectl delete ns NAMESPACE_NAME`
  - Inspect the pod: `kubectl get pods POD_NAME -o yaml > test.yml`
  - Inspect the pod in different namespace: `kubectl get pods -n NAMESPACE POD_NAME -o yaml > test.yml`
- Sample 1:
  ```yaml
  apiVersion: v1
  kind: Namespace
  metadata:
    name: test
  spec:
  ```
- Sample 2:
  ```yaml
  apiVersion: v1
  kind: Pod
  metadata:
    name: pod-example
    namespace: test
  spec:
    containers:
    - name: nginx-test
      image: nginx
    - image: redis-alpine
      name: redis
  ```

Same Pods with different namespaces:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod-example1
spec:
  containers:
  - name: nginx-test1
    image: nginx
---
apiVersion: v1
kind: Pod
metadata:
  name: pod-example2
spec:
  containers:
  - name: nginx-test2
    image: nginx
```

Important: If you apply/create a yaml file without specific namespace, it will create pod(s) in the current active namespace.

Imperative Mode to run a Pod:
- `kubectl run --image=IMAGE_NAME POD_NAME`
- `kubectl run --image=IMAGE_NAME POD_NAME -o yaml --dry-run=client`

Important: Cross Namespace Communication will be discussed in the DNS section.

Argument `--dry-run` modes:
1. None: This was the default option, and it deprecated.
2. Client: You can use the `--dry-run=client` flag to preview the object that would be sent to your cluster, without really submitting it.
3. Server: You can use the `--dry-run=server` flag to submit server-side request without persisting the resource.

---

## Session 04 (Extra Concepts for POD)

Controller assign IPs to Pause containers from the range `--pod-network-cidr=20.16.0.0/12` and assign a subnet to each node when they join the cluster. We don't have a DHCP in K8s network.

CNI tasks:
- Internode Communication Connection
- It makes sure each node gets an unique IP and they don't conflict with other nodes
- For instance, Weave has better monitoring, Flannel doesn't have security, Calico has the best performance and flexibility, etc.
- IP Range of each node: `kubectl get nodes -o json | grep podCIDR`

Flow of API calls in K8s:
- Kubectl --with manifest(s)--> API Server -> Scheduler -> Kubelet -> CRI -> CNI (have access to API Server) -> gives a range IP to your node -> CRI -> OCI (runC)

CGroup is a resource limitation of containers + Namespace is for container isolation => makes a container

Address of CNI configs `/etc/cni/net.d/`:
- `10-calico.conflist`
- `calico-kubeconfig`

List of IP addresses of Nodes `/var/lib/cni/`:
- `cache`
- `networks`

List of commands that calico uses for network management: `/opt/cni/bin`

Labels are important for query nodes: `get pods --show-labels`
- You can label every object!
- Assign a label: `kubectl label pod POD_NAME KEY=VALUE`
- Assign labels: `kubectl label pod POD_NAME KEY1=VALUE1 KEY2=VALUE2 KEY3=VALUE3`
- Delete a label: `kubectl label pod POD_NAME KEY-`
- Query: `kubectl get pods -l KEY=VALUE`

Apply vs Create:
- `kubectl create -f manifest.yml`
- `kubectl apply -f manifest.yml`
- If an object created with create command, you can't edit or change it!
- We don't use _create_ command usually.
- We edit objects like this:
  - change manifest and apply (again).
  - `kubectl edit pod POD_NAME`
  - The name of a pod is not editable, but if you change it, a new manifest created in `/tmp` and you can apply it from that location.
  - Imperative way to create a pod with configuration like restart which is not a metadata. It is in spec section: `kubectl run --image=IMAGE:TAG restart --restart POLICY`

Container Restart Policy for Kubernetes:
- Always (Default)
- OnFailure
- Never (Used for Jobs)

Environment:
- Linux:
  - set: Local Environment
  - env: Global Environment
- Play with ENV variables:
  - manifest:
    ```yaml
    apiVersion: v1
    kind: Pod
    metadata:
      name: NAME
      labels: {
        "key1":"value1",
        "key2":"value2"
      }
    specs:
      containers:
        - image: busybox:1.28
          name: test-env
          command:
            - sleep
            - "3600"
          env:
            - name: db_address
              value: "10.10.1.1"
            - name: db_port
              value: "3306"
    ```
  - `kubectl exec NAME -- printenv`
  - Imperative: `kubectl run --image=IMAGE:TAG NAME --command COMMAND --env=KEY=VALUE --env=KEY1=VALUE1 -o yaml --dry-run=client`

Explain in manual/documentation: `kubectl explain OBJECT`

Image Pull Policy (subset of spec.container in manifest):
- IfNotPresent (Default)
- Always

Resource Limitation:
- Every single pod must have Request or Limit!
- Namespaces can have resource limitation too.
- CGroup (Memory, CPU)
  - RAM: Mi, Gi, ...
  - CPU: Millicore (4 Cores = 4000 Millicore = 4000m = 4)
- Request (Minimum Requirement)
- Limit (Maximum Requirement)
- In the future DISK IO will be added for limitation.

QOS states:
- BestEffort (It will be the first pod that will evict on that node, if another pod needed to deploy on the node)
- Burstable (no limits)
  - `kubectl run --image=IMAGE:TAG POD_NAME resource --requests=cpu=20m,memory=10Mi -o yaml --dry-run=client`
- Guaranteed
  - `kubectl run --image=IMAGE:TAG POD_NAME resource --requests=cpu=20m,memory=10Mi --limits=cpu=40m,memory=20Mi -o yaml --dry-run=client`
<details>
  <summary>Avoiding CPU Throttling in a Containerized Environment</summary>
  <p>
  In order to use cpusets, containers must be bound to cores. Allocating cores correctly requires a bit of background on how modern CPU architectures work since the wrong allocation can cause significant performance degradations.

  A CPU is typically structured around:
  - A physical machine can have multiple CPU sockets
  - Each socket has an independent L3 cache
  - Each CPU has multiple cores
  - Each core has independent L2/L1 caches
  - Each core can have hyperthreads
  - Hyperthreads are often regarded as cores, but allocating 2 hyperthreads instead of 1 might only increase performance by 1.3x
  </p>
  <p>
    <a href="https://eng.uber.com/avoiding-cpu-throttling-in-a-containerized-environment/">
      Source
    </a>
</details>

Port Forward:
- `kubectl port-forward --help`
- `kubectl port-forward POD_NAME CLUSTER_PORT:APPLICATION_PORT`

Proxy (It's just for a test and connect to that cluster):
- `kubectl proxy --help`
- `kubectl proxy PORT`

initContainers in multi-container Pods:
- Containers in single pod come up in parallel simultaneously.
- Init-Container: It/They will be up until it finishes the job and then goes down and then the rest of the containers come up.
  - In the manifest file, the initContainers must be written in order. If one of them fails, the rest of them won't run/execute, so the Pod will fail.
  - All of containers must completely run successfully until the pod runs.
  - When a pod stop working, the initContainer in the new pod will run.
  - Examples:
    - We want to connect a pod to a database and we want to make sure that this container connects successfully to the database; otherwise the pod will fail.
    - We can create a initContainer with a single command such as `git pull` to get the latest version of the source code. (If we don't have any CI/CD or container registry)
    - We can create a initContainer with sets of command to check network connectivity of the main container, like `nslookup`, `ping`, or `telnet`. If these commands executed successfully, then then the main container comes up.
- Sample:
  ```yaml
  apiVersion: v1
  kind: Pod
  metadata:
    name: NAME
    labels: {
      "key1":"value1",
      "key2":"value2"
    }
  specs:
    containers:
      - image: nginx:latest
        name: webserver
    initContainers:
      - image: busybox:1.28
        name: test
        command: ["/bin/sh"]
        args: ["-c", "for i in $(seq 1 10); do echo hello; done"]
  ```

Check this link out if you are interested in the last topic: [Inject Data into Application](https://kubernetes.io/docs/tasks/inject-data-application/define-command-argument-container)

---

## Session 05 (POD + Services Intro)

Technically, we run the commands in an imperative way to find the declarative way:
```bash
kubectl run --image=busybox:1.28 BUSY --command sleep 3600 -o yaml --dry-run=client > busy.yaml
kubectl apply busy.yaml
kubectl exec -it BUSY -- bash
date -s "2020-11-24"
# We are root, but we cannot change the date because of security policies!
```

Read these Security Context:
- [Search Pod Security Context](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/)
- [Linux Capabilities and Security Best Practice](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/)
- [Pod Security Policy](https://kubernetes.io/docs/concepts/policy/pod-security-policy)
- [Linux Capabilities](https://book.hacktricks.xyz/linux-unix/privilege-escalation/linux-capabilities)
- Check the manual page of this linux command: `man 7 capabilities`

Capabilities Levels Sample:
1. Pod
    ```yaml
    apiVersion: v1
    kind: Pod
    metadata:
      name: security-context-demo
    spec:
      securityContext:
        runAsUser: 1000
        runAsGroup: 3000
        fsGroup: 2000
      volumes:
      - name: sec-ctx-vol
        emptyDir: {}
      containers:
      - name: sec-ctx-demo
        image: busybox
        command: [ "sh", "-c", "sleep 1h" ]
        volumeMounts:
        - name: sec-ctx-vol
          mountPath: /data/demo
        securityContext:
          allowPrivilegeEscalation: false
    ```
2. Container
    ```yaml
    apiVersion: v1
    kind: Pod
    metadata:
      name: security-context-demo-4
    spec:
      containers:
      - name: sec-ctx-4
        image: gcr.io/google-samples/node-hello:1.0
        securityContext:
          capabilities:
            add: ["SYS_TIME"]
    ```

**PODS MUST NOT RUN BY ROOT USER.**

- Run a container as a non-root user:
  ```yaml
  apiVersion: v1
  kind: Pod
  metadata:
    labels:
      run: nginx
    name: nginx
  spec:
    containers:
      - image: nginx
        name: nginx
        imagePullPolicy: IfNotPresent
        securityContext:
          runAsNonRoot: true
  ```
- You can't deploy the last manifest in your K8s cluster obviously. The pod won't create because the container wants to run as root, and the pod wants to run as non root. So this conflict won't let the pod to run.

Kubectl's plugins:
- `kubectl get pods BUSY -o yaml`
- [Krew](https://krew.sigs.k8s.io/) -> This program must not install on the production! It's only for developers.
- Check the installation: `kubectl krew`
- Install Plugin: `kubectl krew install PLUGIN`
- Sample Plugin usage: `kubectl get pods BUSY -o yaml | kubectl neat`

### Services
- Pods get IPs from CNI (calico, ...)
- We want to expose some pods using a Load Balancer -> we create a new resource as a SERVICE
  - IP will change if a pod kills/destroys
  - Name of PODs can change (Depends on how you create/run a pod)
  - Labels won't change under any circumstances
- Service Types:
  - ClusterIP: Only for internal communication (Default service type) (Example: Connect your backend to frontend)
  - Node Port: Opens a random port between 30000 and 32767 on your node
  - Load Balancer
  - ExternalName
  - Service without Selector
  - Headless
    - When there is no need of load balancing or single-service IP addresses. We create a headless service which is used for creating a service grouping. That does not allocate an IP address or forward traffic. So you can do this by explicitly setting ClusterIP to "None" in the manifest file, which means no cluster IP is allocated.
    - Kubernetes allows clients to discover pod IPs through DNS lookups. Usually, when you perform a DNS lookup for a service, the DNS server returns a single IP which is the service’s cluster IP. But if you don’t need the cluster IP for your service, you can set ClusterIP to "None", then the DNS server will return the individual pod IPs instead of the service IP. Then client can connect to any of them.
- Different Types of port:
  - Port: Input port of a service
  - Target port: Output port of service connects to different pods
  - Node port: Port of a node that connects to input port of service
- IP of service can change.
- ClusterIP can change to NodePort, and NodePort can change to LoadBalancer

#### ClusterIP
```yaml
kind: Service
apiVersion: v1
metadata:
  # Unique key of the Service instance
  name: service-example
spec:
  ports:
    # Accept traffic sent to port 80
    - name: http
      port: 80
      targetPort: 80
  selector:
    # Loadbalance traffic across Pods matching
    # this label selector
    app: nginx
  # Create an HA proxy in the cloud provider
  # with an External IP address - *Only supported
  # by some cloud providers*
  type: ClusterIP
```

Execute these commands for better understanding:
- Run pods, then run service
  ```bash
  kubectl get service      # kubectl get svc
  kubectl get endpoints    # kubectl get ep
  kubectl get pods -o wide
  kubectl get svc -o wide
  ```
- Go to busybox of the cluster and `nslookup SERVICE-NAME`
- `kubectl exec -it BUSY -- nslookup SVC-NAME`
- CoreDNS: `kubectl get svc -n kube-system` and `kubectl get ep -n kube-system`
- `kubectl get pods -n kube-system -o wide`
- Logs of components are VERY IMPORTANT!

#### Node Port
```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    app: nginx
  ports:
      # By default and for convenience, the `targetPort` is set to the same value as the `port` field.
    - port: 80 # mandatory
      targetPort: 80
      # Optional field
      # By default and for convenience, the Kubernetes control plane will allocate a port from a range (default: 30000-32767)
      nodePort: 32100
  type: NodePort
```

Commands:
- `kubectl get svc`
- `kubectl get ep`
- `kubectl describe svc kube-dns`
- kube proxy nat your real IP and masquerade it. It creates a hub inside your network and more latency on the network. kube proxy has a feature called `externalTrafficPolicy=Cluster` and based on the health of pods, does its job as load balancing. It has `externalTrafficPolicy=Local` which makes the traffic imbalance traffic spreading. Local mode works based on the health of node. Cluster mode works based on the health of pod.
- Maintenance cost is high in nodeport.
- externalTrafficPolicy can only be assigned on NodePort and LoadBalancer

#### Load Balancer
- Cloud providers give us the loadbalancer.
- Kubernetes by default doesn't have a network loadbalancer, so we can use programs like [MetalLB](metallb.universe.tf)
  - Go to installation
  - Go to configuration
  - Select Layer 2 for test usage
- `kubectl get svc --all-namespaces`

```yaml
apiVersion: v1
kind: Service
metadata:
  name: service-lb
spec:
  selector:
    app: nginx
  ports:
    - port: 80
  type: LoadBalancer
```

---

## Session 06 (Services)

Services:
1. ClusterIP (Internal)
2. NodePort (External on Node Level)
3. LoadBalancer (Metallb, ...)
4. Service Without Selectors
5. ExternalName
6. Headless

- kubectl expose ?
  - Deployment
  - Pod
  - ReplicaSet
  - ReplicationController
  - Service

Kubernetes Dashboards/Visualizers:
- [Kubernetes Visualizer](https://schoolofdevops.github.io/ultimate-kubernetes-bootcamp/kube_visualizer/)
- [Kubernetes Dashboard](https://github.com/kubernetes/dashboard)
- [Kubernetes Lens](https://k8slens.dev/)
- [OpenShift](https://www.redhat.com/en/technologies/cloud-computing/openshift)
- [Rancher](https://rancher.com/)

#### Service without Selector
- Accessing a Service without a selector works the same as if it had a selector. For instance, traffic is routed to the single endpoint defined in the YAML file.
- We create a service without selector that connects to modified endpoint which let k8s to redirect service to that external server, db, etc.
- Endpoints cannot be localhost or link-locals.
- Endpoints cannot be a part of your cluster or system.

#### ExternalName
- An ExternalName Service is a special case of Service that does not have selectors and uses DNS names instead.

Imperative way of a service:
- `kubectl create service TYPE --help`

- ExternalName service doesn't have an Endpoint or selector. You give the service a name so that a long CNAME can be mapped to a service name. 
- Example: `kubectl create service externalname --external-name=www.google.com test -o yaml --dry-run=client`
- Activate IP Forwarding:
  - Temporary: `echo 1 > /proc/sys/net/ipv4/ip_forward`
  - Permanently:
    - `sed "s/#net.ipv4.ip_forward=1/net.ipv4.ip_forward=1/" /etc/sysctl.conf && sysctl -p`
    - `echo "net.ipv4.ip_forward=1" >> /etc/sysctl.conf && sysctl -p`
- Example:
  ```yaml
  apiVersion: v1
  kind: Service
  metadata:
    name: test
    namespace: prod
  specs:
    externalName: www.google.com
    type: ExternalName
  ```

The Keepalived service makes a lot of ARP, we can use a pacemaker instead.

- ExternalIPs (one of the object that YOU have to manage instead of Kubernetes)
  - You have to assign your Floating IP to externalIP.
  - It can be used along side all services!
  - It connects you to your nodes and pods.
  - Example:
    - Expose a simple pod: `kubectl expose pod nginx --port 80 --name NAME -o yaml --dry-run=client`
    - Expose a pod with ExternalIP: `kubectl expose pod nginx --port 80 --name NAME --external-ip IP -o yaml --dry-run=client`

Session Affinity:
- `kubectl describe svc SERVICE_NAME`
- `kubectl explain svc.spec`
- Persistent Session for a pod is mandatory in production environment.
- Default: None
- Default Timeout: 10800s (3 Hours)
- Example 1:
  ```yaml
  apiVersion: v1
  kind: Service
  metadata:
    name: my-service
  spec:
    selector:
      app: my-app
    ports:
    - name: http
      protocol: TCP
      port: 80
      targetPort: 80
    sessionAffinity: ClientIP
  ```
- Example 2:
  ```yaml
  apiVersion: v1
  kind: Service
  metadata:
    name: my-service
  spec:
    selector:
      app: my-app
    ports:
    - name: http
      protocol: TCP
      port: 80
      targetPort: 80
    sessionAffinity:
      clientIP:
        timeoutSeconds: 300
  ```

---

## Session 07 (StaticPodPath + Context)

### Static Pod Path

StaticPodPath:
- Location: `/etc/kubernetes/manifests`
- If you want to create a pod that always be up on specific nodes, you have add their manifests to the directory of kubelet, and that/those pod(s) will always be up and running.
- Manifests are standard Pod definitions in JSON or YAML format in a specific directory. Use the *staticPodPath: /PATH/OF/CONFIGURATIONS/DIRECTORY* field in the kubelet configuration file, which periodically scans the directory and creates/deletes static Pods as YAML/JSON files appear/disappear there.
- Note that the kubelet will ignore files starting with dots when scanning the specified directory.

### Context
- Kubectl configs: `kubectl config view` or `kubectl config view --raw`
- Certificates: `/etc/kubernetes/pki`
- Every CA encodes with Base64
- `curl https://MASTERIP:6443/ --cacert ./K8S-CA.crt --cert ./USER-CRT --key ./USER-KEY`
- Create a new cluster with system's CA: `kubectl config --kubeconfig=PATH_OF_NEW_CONFIG set-cluster CLUSTER_NAME --certificate-authority=PATH_OF_CA --server https://IP:6443 --embed-certs`
- Add user to config file: `kubectl config --kubeconfig=PATH_OF_THIS_CONFIG set-credentials NAME_OF_CRED --client-key=PATH_OF_KEY --client-certificate=PATH_OF_CERT --user USERNAME --embed-certs`
- Add Context: `kubectl config --kubeconfig=PATH_OF_CONFIG set-context CONTEXT_NAME --server https://IP:6443 --user USERNAME`
- Add Current Context: `kubectl config --kubeconfig=PATH_OF_CONFIG use-context CONTEXT_NAME --cluster CLUSTER_NAME`
- User this kubeconfig: `kubectl --kubeconfig=CONFIG_FILE get nodes`
- Add config permanently:
  - `cp CONFIG_FILE /root/.kube/`
  - `echo "export KUBECONFIG=/root/.kube/config:/root/.kube/CONFIG_FILE" >> ~/.bashrc`
  - `kubectl config get-contexts`
  - `kubectl config use-context CONTEXT_NAME`

---

## Session 08 (ReplicaSet + Advance Labeling + Replication Controller)

- Extra Topics/Tools:
  - CD (Continuous Deployment) => Solutions: ArgoCD / Flux => [Gitops Website](https://gitops.tech)
  - Load-Balancer (Better than others, but it has a bad documentation) => [Porter](https://github.com/kubesphere/porter)
  - Sample Server Load Balancer named [PureLB](https://gitlab.com/purelb/purelb)
  - CoreDNS IP: `kubectl get svc -n kube-system`

### ReplicaSet
- Another object in kubernetes.
- It makes sure that the number of pods are correct. (current_state == desired_state)
- Reconciliation Loop | Self-Healing
- `kubectl api-resources`
- `kubectl explain rs`
- Example:
  ```yaml
  apiVersion: apps/v1
  kind: ReplicaSet
  metadata:
    name: testrs
  spec:
    replicas: 2
    selector:
      matchLables:
        app: v1
    template:
      # metadata of POD
      metadata:
        name: pod1
        labels:
          app: v1
      spec:
        containers:
          - name: test
            image: nginx
  ```
- `kubectl apply -f rs.yaml`
- `kubectl get rs`
- IMPORTANT:
  - We don't do this in production: `kubectl scale replicaset --replicas=4 REPLICASET_NAME`
  - We don't create a ReplicaSet in imperative way. (Best Practice)
- When you check the output of the `kubectl api-resources` command, you can see that there's a column name **NAMESPACED** which describes if an object's namespace is **True**, it can change action in different namespaces, and it won't be global. But if it is **False**, it means that the resource isn't limited by any namespaces and is globally used in the cluster.
- *Kubeproxy* setting allows you to change IPtables for assigning virtual IPs. IPVS is more flexible than IPtables. But it is more complex and sometimes makes more latency, and maybe you lose some features. (Check service in Kubernetes) -> we want to access all the backend and services (not randomly), so we change the backend networking to IPVS to access pods/services with different mechanisms/algorithms.
- Example:
  - `kubectl run --image=IMAGE:TAG --labels x=y,ver=v5 POD_NAME -o yaml --dry-run=client`
  - ImagePullPolicy: never
    ```yaml
    apiVersion: apps/v1
    kind: ReplicaSet
    metadata:
      name: rs-vote
    spec:
      replicas: 3
      selector:
        matchLables:
          app: vote
          ver: v3
      template:
        # metadata and spec of POD
    ```
  - `kubectl apply -f rs-sample.yaml`
  - `kubectl expose replicaset rs-sample --port=80 --name=rs-sample --external-ip=WORKER_IP -o yaml --dry-run=client > svc-sample.yaml`
  - You can't edit fundamental configurations of a replicaset after creation. Replicaset only handles replication, not editing. -> Check it for yourself `kubectl edit pod POD_OF_REPLICASET`
  - When we decrease the number of replicas, it removes pod(s) RANDOMLY.

### Advanced Scheduling & Session/Node Affinity
