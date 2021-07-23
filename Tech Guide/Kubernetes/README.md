# Kubernetes | K8s

- [Kubernetes | K8s](#kubernetes--k8s)
  - [Session 00 (Basic Info)](#session-00-basic-info)
    - [Introduction](#introduction)
  - [Session 01 (K8S Concepts and Keywords)](#session-01-k8s-concepts-and-keywords)
  - [Session 02 (Installation)](#session-02-installation)
  - [Session 03 (YAML Intro + Pod)](#session-03-yaml-intro--pod)
    - [YAML Introduction](#yaml-introduction)
    - [POD](#pod)
  - [Session 04](#session-04)

## Session 00 (Basic Info)

Kubernetes used for stateless containers/softwares -> rook

Helm is not coming in exam.

Exams (66 is Pass):
- CKAD (Certified Kubernetes Application Developer)
- CKA (Certified Kubernetes Administrator)
- CKS (Certified Kubernetes Security Specialist)

Solutions:
- Persistent storage (NFS)
- ...

Zero time upgrade

Config and support of microservices is hard and frustrating.

- Scale Up: Increase Resources -> Vertical
- Scale Out: Increase Number of Instance or Servers -> Horizontal

- Distributed Tracing System:
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

All connections between different part of k8s are based on MTLS (Multi Layer TLS or Mutual TLS).

Split brain on cluster concept -> Must be an odd number

No one install the hardway (from source)

Some Useful Ports:
- API Server: 6443
- etcd: 2379

Config file of kubectl: `~/.config/.kube/config`

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

**Kubernetes Cluster Federation** (Read it)

**Check out Netflix migrate to Microservice Roadmap YouTube**

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
12. It is recommended to run `kubeadm config images pull` prior to `kubeadm init` to verify connectivity to the [Google Container Registry](gcr.io) container image registry. You can also get the list of the packages with `kubeadm config images list` command.
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

Imperative Mode to run a Pod:
- `kubectl run --image=IMAGE_NAME POD_NAME`
- `kubectl run --image=IMAGE_NAME POD_NAME -o yaml --dry-run=client`

Argument `--dry-run` modes:
1. None: This was the default option, and it deprecated.
2. Client: You can use the `--dry-run=client` flag to preview the object that would be sent to your cluster, without really submitting it.
3. Server: You can use the `--dry-run=server` flag to submit server-side request without persisting the resource.

---

## Session 04