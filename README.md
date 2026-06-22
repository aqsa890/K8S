# K8S

K8S is a Kubernetes-focused repository that combines cluster resource manifests with an included copy of the Kubernetes Autoscaler codebase (`apache/autoscaler-master`).

## Prerequisites

- [Go](https://go.dev/) (for working with autoscaler source code)
- [Docker](https://www.docker.com/) (for containerized workflows)
- [kubectl](https://kubernetes.io/docs/tasks/tools/)
- A Kubernetes cluster (local options like [kind](https://kind.sigs.k8s.io/) are suitable)

## Local development / setup

1. Clone the repository:
   ```bash
   git clone https://github.com/aqsa890/K8S.git
   cd K8S
   ```
2. (Optional) Create a local kind cluster using the provided config:
   ```bash
   kind create cluster --config config.yml
   ```
3. If developing autoscaler components, work under:
   - `apache/autoscaler-master/cluster-autoscaler`
   - `apache/autoscaler-master/vertical-pod-autoscaler`
   - `apache/autoscaler-master/addon-resizer`

## Build and test

Examples validated from the repository:

```bash
# Build Cluster Autoscaler
make -C apache/autoscaler-master/cluster-autoscaler build

# Run a targeted Go test package
cd apache/autoscaler-master/cluster-autoscaler
go test ./metrics/...
```

Additional component-specific scripts and Make targets are available under `apache/autoscaler-master`.

## Run / deploy

This repository contains Kubernetes YAML manifests that can be applied to a cluster, for example:

```bash
kubectl apply -f nginx/
kubectl apply -f mysql/
kubectl apply -f crd/
kubectl apply -f dashboard/
```

For autoscaler deployment details, refer to component docs:
- `apache/autoscaler-master/cluster-autoscaler/README.md`
- `apache/autoscaler-master/vertical-pod-autoscaler/README.md`

## Repository structure

- `apache/` – Kubernetes Autoscaler source tree and related manifests
- `nginx/` – Kubernetes resources for nginx workloads (deployment/service/ingress/etc.)
- `mysql/` – Kubernetes resources for MySQL (namespace/config/secret/statefulset/service)
- `crd/` – Custom resource examples and definitions
- `dashboard/` – Kubernetes dashboard admin user manifest
- `config.yml` – kind cluster configuration

## Notes for contributors

- Keep manifest changes scoped and environment-aware (namespace, storage class, ingress assumptions).
- Prefer validating changes in a local cluster before submitting.
- For autoscaler code contributions, follow the upstream guidelines in:
  - `apache/autoscaler-master/CONTRIBUTING.md`
  - component README files inside `apache/autoscaler-master/`
