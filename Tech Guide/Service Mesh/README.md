# Service Mesh

> This basic document is just a concept of the service mesh.

## Introduction

A service mesh is a way to control how different parts of an application share data with one another. Unlike other systems for managing this communication, a service mesh is a dedicated infrastructure layer built right into an app. This visible infrastructure layer can document how well (or not) different parts of an app interact, so it becomes easier to optimize communication and avoid downtime as an app grows.

Service Mesh is basically a proxy network with lots of features.

Key Components of a Service Mesh:
- Service Discovery
  - Address
  - Traffic Split
  - Deployment Strategies/Patterns
- Traffic Management
  - Load-balancing
  - Retries
  - Fault Injection (Chaos Engineering)
  - Mutual TLS (mTLS)
  - Cert Management
  - Authentication & Authorization
- Observability
  - Traffic Visualization (Kiali or ...)
  - Distributed Tracing (Jaeger)
  - Metrics (Prometheus & Grafana) & Logging

Service Mesh:
- Proxy
  - Sidecar (EnvoyProxy)
  - Network Proxy
  - Rules & Extensions
- Control-Plane
  - Sidecar
  - Network Proxy
  - Automatic Injection

Service Mesh Cons:
- Complex Configuration
- Sidecar Setup Added
- Envoy Configuration Added

Main Service Meshes:
- Linkerd (Graduated CNCF Project)
  - [Linkerd Official Website](https://linkerd.io/)
  - [Deploying and Upgrading Linkerd using ArgoCD for GitOps](https://linkerd.io/2/tasks/gitops/)
- Istio
- Open Service Mesh
- Service Mesh Interface (SMI)

Main Parts of Service Meshes on K8s:
- CLI / Operator -> To Deploy Control-Plane + Inject Proxy + ...
- CRDs (Custom Resource Definition)
- Mutating Webhooks