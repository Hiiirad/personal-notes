# Service Mesh

> This basic document is just a concept of the service mesh.

## Introduction

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
- Istio
- Open Service Mesh
- Service Mesh Interface (SMI)

Main Parts of Service Meshes on K8s:
- CLI / Operator -> To Deploy Control-Plane + Inject Proxy + ...
- CRDs (Custom Resource Definition)
- Mutating Webhooks