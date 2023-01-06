- [Preparation for Kubernetes Exams](#preparation-for-kubernetes-exams)
  - [CKAD Introduction](#ckad-introduction)
  - [CKA Introduction](#cka-introduction)
  - [CKS Introduction](#cks-introduction)
- [CKAD](#ckad)
- [CKA](#cka)
- [CKS](#cks)

> This Document Updated at Dec-14-2022

# Preparation for Kubernetes Exams

## CKAD Introduction
The Certified Kubernetes Application Developer (CKAD) exam certifies that candidates can design, build and deploy cloud-native applications for Kubernetes. This exam is an online, proctored, performance-based test that consists of a set of performance-based tasks (problems) to be solved in a command line. Candidates have 2 hours to complete the tasks.

The exam is based on Kubernetes v1.25

It is also a performance-based hands-on exam where you need to solve Kubernetes tasks related to cloud-native application deployment on Kubernetes. CKAD covers the following Domains & Competencies:
- Application Design and Build
- Application Deployment
- Application Observability and Maintenance
- Application Environment, Configuration, and Security
- Services and Networking

## CKA Introduction
The Certified Kubernetes Administrator (CKA) program provides assurance that CKAs have the skills, knowledge, and competency to perform the responsibilities of Kubernetes administrators. This exam is an online, proctored, performance-based test that requires solving multiple tasks from a command line running Kubernetes. Candidates have 2 hours to complete the tasks.

Candidates who register for the Certified Kubernetes Administrator (CKA), exam will have 2 attempts (per exam registration) to an exam simulator, provided by [Killer.sh](https://killer.sh).

The exam is based on Kubernetes v1.25

The CKA exam focuses on the following domain and competencies:
- Storage
- Troubleshooting
- Workloads & Scheduling
- Cluster Architecture, Installation & Configuration
- Services & Networking

## CKS Introduction
The Certified Kubernetes Security Specialist (CKS) program provides assurance that a CKS has the skills, knowledge, and competence on a broad range of best practices for securing container-based applications and Kubernetes platforms during build, deployment and runtime. Certified Kubernetes Security Specialist (CKS) candidates must have taken and passed the Certified Kubernetes Administrator (CKA) exam prior to attempting the CKS exam.

CKS may be purchased but not scheduled until CKA certification has been achieved. CKA Certification must be active (non-expired) on the date the CKS exam (including Retakes) is scheduled.

This exam is an online, proctored, performance-based test that requires solving multiple tasks from a command line running Kubernetes. Candidates have 2 hours to complete the tasks.

The exam is based on Kubernetes v1.25

To appear for the CKS exam, you need to pass the CKA certification first. CKS exam covers the following domains and competencies:
- Cluster Setup
- Cluster Hardening
- System Hardening
- Minimize Microservice Vulnerabilities
- Supply Chain Security
- Monitoring, Logging, and Runtime Security

# CKAD
- Exam Domains & Competencies:
  - Application Design and Build (20%)
    - Define, build and modify container images
    - Understand Jobs and CronJobs
    - Understand multi-container Pod design patterns (e.g. sidecar, init and others)
    - Utilize persistent and ephemeral volumes
  - Application Deployment (20%)
    - Use Kubernetes primitives to implement common deployment strategies (e.g. Blue/Green or Canary)
    - Understand Deployments and how to perform rolling updates
    - Use the Helm package manager to deploy existing packages
  - Application Observability and Maintenance (15%)
    - Understand API deprecations
    - Implement probes and health checks
    - Use provided tools to monitor Kubernetes applications
    - Utilize container logs
    - Debugging in Kubernetes
  - Application Environment, Configuration and Security (25%)
    - Discover and use resources that extend Kubernetes (CRD)
    - Understand authentication, authorization and admission control
    - Understanding and defining resource requirements, limits and quotas
    - Understand ConfigMaps
    - Create & consume Secrets
    - Understand ServiceAccounts
    - Understand SecurityContexts
  - Services & Networking (20%)
    - Demonstrate basic understanding of NetworkPolicies
    - Provide and troubleshoot access to applications via services
    - Use Ingress rules to expose applications

[Source of Details](https://devopscube.com/ckad-exam-study-guide/)

# CKA
- Exam Domains & Competencies:
  - Storage (10%)
    - Understand storage classes, persistent volumes
    - Understand volume mode, access modes and reclaim policies for volumes
    - Understand persistent volume claims primitive
    - Know how to configure applications with persistent storage
  - Troubleshooting (30%)
    - Evaluate cluster and node logging
    - Understand how to monitor applications
    - Manage container stdout & stderr logs
    - Troubleshoot application failure
    - Troubleshoot cluster component failure
    - Troubleshoot networking
  - Workloads & Scheduling (15%)
    - Understand deployments and how to perform rolling update and rollbacks
    - Use ConfigMaps and Secrets to configure applications
    - Know how to scale applications
    - Understand the primitives used to create robust, self-healing, application deployments
    - Understand how resource limits can affect Pod scheduling
    - Awareness of manifest management and common templating tools
  - Cluster Architecture, Installation & Configuration (25%)
    - Manage role based access control (RBAC)
    - Use Kubeadm to install a basic cluster
    - Manage a highly-available Kubernetes cluster
    - Provision underlying infrastructure to deploy a Kubernetes cluster
    - Perform a version upgrade on a Kubernetes cluster using Kubeadm
    - Implement etcd backup and restore
  - Services & Networking (20%)
    - Understand host networking configuration on the cluster nodes
    - Understand connectivity between Pods
    - Understand ClusterIP, NodePort, LoadBalancer service types and endpoints
    - Know how to use Ingress controllers and Ingress resources
    - Know how to configure and use CoreDNS
    - Choose an appropriate container network interface plugin

[Source of Details](https://devopscube.com/cka-exam-study-guide/)

# CKS
- Exam Domains & Competencies:
  - Cluster Setup (10%)
    - Use Network security policies to restrict cluster level access
    - Use CIS benchmark to review the security configuration of Kubernetes components (etcd, kubelet, kubedns, kubeapi)
    - Properly set up Ingress objects with security control
    - Protect node metadata and endpoints
    - Minimize use of, and access to, GUI elements
    - Verify platform binaries before deploying
  - Cluster Hardening (15%)
      - Restrict access to Kubernetes API
      - Use Role Based Access Controls to minimize exposure
      - Exercise caution in using service accounts (e.g. disable defaults, minimize permissions on newly created ones)
      - Update Kubernetes frequently
  - System Hardening (15%)
      - Minimize host OS footprint (reduce attack surface)
      - Minimize IAM roles
      - Minimize external access to the network
      - Appropriately use kernel hardening tools such as AppArmor, seccomp
  - Minimize Microservice Vulnerabilities (20%)
      - Setup appropriate OS level security domains (e.g. using PSP, OPA, security contexts)
      - Manage Kubernetes secrets
      - Use container runtime sandboxes in multi-tenant environments (e.g. gvisor, kata containers)
      - Implement pod to pod encryption by use of mTLS
  - Supply Chain Security (20%)
      - Minimize base image footprint
      - Secure your supply chain: whitelist allowed registries, sign and validate images
      - Use static analysis of user workloads (e.g. Kubernetes resources, Dockerfiles)
      - Scan images for known vulnerabilities
  - Monitoring, Logging and Runtime Security (20%)
    - Perform behavioral analytics of syscall process and file activities at the host and container level to detect malicious activities
    - Detect threats within physical infrastructure, apps, networks, data, users and workloads
    - Detect all phases of attack regardless where it occurs and how it spreads
    - Perform deep analytical investigation and identification of bad actors within environment
    - Ensure immutability of containers at runtime
    - Use Audit Logs to monitor access

[Source of Details](https://devopscube.com/cks-exam-guide-tips/)
