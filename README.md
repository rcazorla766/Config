# MLOps GitOps Configuration Repository

This repository contains the declarative deployment configuration for the TFM project:

**"Uso de GitOps para la Gobernanza de Modelos de IA en Producción (MLOps + DevOps)"**

The repository is designed following GitOps principles, using Git as the single source of truth for system state management.

---

# Objectives

This repository is responsible for:

- Kubernetes manifests
- Deployment configuration
- Environment definitions
- Infrastructure declarative state
- GitOps synchronization

---

# Planned Technologies

The following technologies are planned to be used during development:

- Kubernetes
- ArgoCD
- YAML manifests
- GitHub
- Infrastructure as Code (IaC)

---

# Repository Structure

.
├── deployments/          # Kubernetes Deployments
├── services/             # Kubernetes Services
├── environments/         # Environment-specific configuration
├── argocd/               # ArgoCD application definitions
└── README.md

---

# GitOps Workflow

The GitOps workflow is based on the following principles:

·Git as the single source of truth
·Declarative infrastructure and deployment definitions
·Automated reconciliation between desired and actual state
·Version-controlled deployments
·Full traceability of configuration changes

Deployment synchronization will be managed through ArgoCD.

# Integration with Application Repository

This repository works together with the application repository:

mlops-gitops-app

The CI pipeline from the application repository will update deployment manifests stored in this repository, enabling automated deployments through GitOps workflows.

# Status

Project currently under development as part of the Master's Thesis (TFM).

# Authors

Master's Degree in Development and Operations (DevOps)
Universidad Internacional de La Rioja (UNIR)

Rubén Cazorla Rodríguez
Cristhian Alexander Cano Correa
