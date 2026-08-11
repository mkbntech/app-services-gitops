# Trailhead Supply Co. — GitOps Repository (`app-services-gitops`)

Declarative Kubernetes and Knative Serverless manifests for Trailhead Supply Co. microservices on **OpenShift 4.22.1**, supporting **Single Node OpenShift (SNO Staging)** and **HA Multi-Node Production**.

---

## 📁 Repository Structure

```text
app-services-gitops/
├── README.md
├── argocd-apps/
│   ├── trailhead-sno-app.yaml     # ArgoCD Application targeting SNO Staging
│   └── trailhead-prod-app.yaml    # ArgoCD Application targeting Production
└── manifests/
    ├── base/                      # Base Kubernetes & Knative manifests
    │   ├── kustomization.yaml
    │   ├── reviews-db.yaml
    │   ├── ui-service-ksvc.yaml
    │   ├── catalogue-service-ksvc.yaml
    │   ├── recommendation-service-ksvc.yaml
    │   └── review-service-ksvc.yaml
    └── overlays/
        ├── sno/                   # Single Node OpenShift (SNO) Staging Overlay
        │   └── kustomization.yaml
        └── production/            # HA Multi-Node Enterprise Production Overlay
            ├── kustomization.yaml
            ├── database-ha-postgres.yaml
            └── security-networkpolicies.yaml
```

---

## 🚀 Environment Overview

* **Staging Target:** Single Node OpenShift (SNO) `v4.22.8` (`trailhead-staging` namespace)
  * Knative Services scale to **0 pods** after 90 seconds of inactivity.
  * Single-replica PostgreSQL database with tuned resource limits.
  * Automated sync and self-healing managed by ArgoCD (`trailhead-sno-app`).

* **Production Target:** Multi-Node OpenShift Container Platform `v4.22.8` (`trailhead-production` namespace)
  * Knative Services configured with `min-scale: 1` to prevent cold starts.
  * High-Availability 3-node PostgreSQL cluster managed by CloudNativePG.
  * Zero Trust NetworkPolicies for database isolation and route protection.
  * Controlled manual sync gates in ArgoCD (`trailhead-prod-app`).
