# EKS GitOps Platform

A hands-on DevOps portfolio project focused on building a cloud-native Kubernetes platform locally first, then designing its AWS EKS production equivalent.

The project intentionally avoids creating AWS resources during the main implementation phase. EKS, VPC, IAM, storage, and ingress architecture will be designed and validated with Terraform without requiring a live AWS deployment.

## Goals

- Run a reproducible multi-node Kubernetes lab with kind
- Manage application delivery with Argo CD and GitOps
- Package workloads with Helm
- Build CI workflows with GitHub Actions and/or Jenkins
- Run stateful workloads such as PostgreSQL
- Add asynchronous messaging with RabbitMQ
- Build observability with Prometheus, Grafana, Loki, and OpenTelemetry
- Add practical DevSecOps controls such as RBAC, NetworkPolicies, image scanning, and IaC scanning
- Design the AWS EKS production equivalent with Terraform without applying cloud resources

## Architecture

### Local implementation

```text
Developer
   |
   v
GitHub Repository
   |
   +--> CI: GitHub Actions / Jenkins
   |
   v
Argo CD
   |
   v
kind Kubernetes Cluster
   |
   +--> Applications
   |      +--> Producer
   |      +--> Consumer
   |
   +--> RabbitMQ
   |
   +--> PostgreSQL
   |
   +--> Observability
   |      +--> Prometheus
   |      +--> Grafana
   |      +--> Loki
   |      +--> OpenTelemetry
   |
   +--> Platform / Security
          +--> Ingress
          +--> RBAC
          +--> NetworkPolicies
          +--> Scanning
```

### Messaging flow

```text
Producer
   |
   v
RabbitMQ
   |
   +--> Queue
          |
          v
       Consumer
          |
          v
      PostgreSQL
```

The messaging phase will include acknowledgements, retries, Dead Letter Queue (DLQ), idempotency concepts, and failure testing.

### Future EKS design

```text
GitHub
   |
Terraform design
   |
   +--> VPC / Subnets
   +--> EKS Managed Control Plane
   +--> Managed Node Groups
   +--> IAM / Pod Identity
   +--> EBS CSI Driver
   +--> AWS Load Balancer Controller
```

This phase is architecture and Terraform validation only. No AWS `terraform apply` is required for the project.

## Repository Layout

```text
infra/
  kind/
  terraform/
    eks/
platform/
  argocd/
  namespaces/
  ingress/
  storage/
apps/
  producer/
  consumer/
  jenkins/
  postgres/
messaging/
  rabbitmq/
observability/
  prometheus/
  grafana/
  loki/
  opentelemetry/
security/
  rbac/
  network-policies/
  scanning/
scripts/
```

## Current Status

### Day 1 - Local Kubernetes + GitOps foundation ✅

Completed:

- Created a multi-node kind cluster
  - 1 control-plane node
  - 2 worker nodes
- Installed Argo CD
- Connected Argo CD to this GitHub repository
- Declared platform namespaces in Git
- Enabled automated sync, pruning, and self-healing
- Tested drift by manually deleting a namespace
- Verified that Argo CD reconciled the cluster back to the desired state
- Verified the Application is `Synced` and `Healthy`

## 7-Day Learning & Build Plan

### Day 1 - Kubernetes + Argo CD foundation ✅

- kind cluster
- kubeconfig and contexts
- Argo CD bootstrap
- Git as the source of truth
- automated sync
- `selfHeal`
- `prune`
- drift and reconciliation testing

### Day 2 - Helm + PostgreSQL

- Migrate useful PostgreSQL configuration from the previous local assignment
- Package/configure the workload with Helm
- Deploy PostgreSQL through Argo CD
- Understand StatefulSet, Service, PVC, storage, and Helm values

### Day 3 - GitOps application structure

- Move application/platform deployments fully under Argo CD
- Improve repository structure
- Work with multiple Argo CD Applications or an App-of-Apps pattern
- Practice sync, rollback, drift, and configuration changes through Git

### Day 4 - CI/CD

- Add GitHub Actions and/or Jenkins
- Run linting and validation
- Validate Helm manifests
- Build/test an application container
- Understand the separation between CI and GitOps-based CD

### Day 5 - Observability

- Deploy Prometheus and Grafana
- Add useful dashboards and alerts
- Add Loki for logs
- Introduce OpenTelemetry concepts and collection
- Perform troubleshooting using metrics and logs

### Day 6 - RabbitMQ + asynchronous processing

- Deploy RabbitMQ through Helm and Argo CD
- Build a simple Producer -> Queue -> Consumer flow
- Persist processed data in PostgreSQL
- Practice acknowledgements and retries
- Configure a Dead Letter Queue (DLQ)
- Discuss idempotency and backpressure
- Simulate a Consumer failure and observe queue behavior

### Day 7 - DevSecOps + EKS design

- Add RBAC
- Add NetworkPolicies
- Add container image scanning with Trivy
- Add IaC scanning with tools such as Checkov
- Design the EKS production equivalent
- Write/validate Terraform for VPC, EKS, node groups, IAM, EBS CSI, and ingress components
- Do not deploy AWS resources

## Interview Topics Covered by the Project

By the end of the project, the implementation should support hands-on discussion of:

- Kubernetes architecture and troubleshooting
- Helm
- Argo CD and GitOps
- Desired state, drift, reconciliation, `selfHeal`, and `prune`
- CI vs CD
- Stateful workloads and persistent storage
- Prometheus, Grafana, logs, and observability
- RabbitMQ messaging
- Queues, acknowledgements, retries, DLQ, idempotency, and backpressure
- RBAC and NetworkPolicies
- Container and IaC scanning
- EKS architecture, IAM, networking, storage, and ingress concepts

## Project Principle

The goal is not to collect technologies. Every component added to the project must be understood well enough to explain its purpose, architecture, failure modes, and trade-offs in a DevOps interview.
