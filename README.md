# EKS GitOps Platform

A hands-on DevOps portfolio project focused on building and operating a cloud-native platform on AWS EKS.

## Goals

- Provision AWS infrastructure with Terraform
- Run Kubernetes workloads on Amazon EKS
- Manage application delivery with Argo CD and GitOps
- Package workloads with Helm
- Expose services through an AWS-native ingress/load-balancing path
- Add persistent storage with the EBS CSI Driver
- Build observability with Prometheus, Grafana, Loki, and OpenTelemetry
- Add practical DevSecOps controls such as RBAC, NetworkPolicies, and image/IaC scanning

## Planned Architecture

```text
GitHub
  |
  +--> Terraform --> AWS VPC --> EKS --> Managed Node Group
  |
  +--> Argo CD -------------------------------------------+
                                                          |
                                                    Kubernetes
                                                          |
                  +----------------+----------------------+----------------+
                  |                |                      |                |
               Jenkins         PostgreSQL           Observability      Security
                                                    Prometheus          RBAC
                                                    Grafana             NetworkPolicy
                                                    Loki                Scanning
                                                    OpenTelemetry
```

## Repository Layout

```text
infra/
  terraform/
    eks/
platform/
  argocd/
  ingress/
  storage/
apps/
  jenkins/
  postgres/
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

## Project Status

Current phase: **Phase 1 - AWS / EKS foundation**.

The project is intentionally being rebuilt from a previous local Kubernetes assignment. Useful Helm/application components will be migrated selectively, while Minikube-specific configuration will be replaced with AWS/EKS-native infrastructure and GitOps patterns.

## Roadmap

1. Terraform foundation: VPC, EKS, Managed Node Group
2. EKS add-ons and EBS CSI Driver
3. Argo CD bootstrap
4. Helm-based application migration
5. Ingress / AWS load balancing
6. Prometheus + Grafana
7. Loki + OpenTelemetry
8. Security controls and CI scanning

