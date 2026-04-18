# Project-on-prim---K8s

Kubernetes deployment (Helm + Argo CD manifests) and microservices source with Jenkins pipelines.

- **K8s-Files** — Helm chart `boutique`, raw `kubernetes-files`, Argo CD applications
- **Src** — application code and `jenkinsfiles` for CI/CD

Docker Hub registry: `01061875164/<service>:<tag>`.

**Argo CD** uses repo [Project-on-prim---K8s](https://github.com/Ahmedhessn/Project-on-prim---K8s), chart path `K8s-Files/helm/boutique`, branches: `dev`, `test` (staging), `main` (prod).
