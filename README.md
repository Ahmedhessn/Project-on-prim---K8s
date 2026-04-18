# Project-on-prim---K8s

Monorepo for **Kubernetes (GitOps)** and **microservices CI/CD**: Helm chart, Argo CD manifests, application source, and GitHub Actions.

- **K8s-Files** — Helm chart `boutique`, raw `kubernetes-files/`, Argo CD applications under `argocd/applications/`
- **Src** — service code under `Src/src/`, legacy Jenkinsfiles under `Src/jenkinsfiles/` (optional; primary CI is GitHub Actions)

**Docker Hub:** `01061875164/<service>:<tag>`  
**Helm chart path (Argo):** `K8s-Files/helm/boutique`  
**GitHub repo:** [Ahmedhessn/Project-on-prim---K8s](https://github.com/Ahmedhessn/Project-on-prim---K8s)

---

## Branch strategy

| Branch | Purpose | Kubernetes target (Argo CD) |
|--------|---------|-------------------------------|
| **dev** | Active development; integration of features | `dev` namespace — Application tracks branch `dev` |
| **test** | Staging / QA; promoted code from `dev` | `staging` namespace — Application tracks branch `test` |
| **main** | Production-stable; promoted after QA sign-off | `prod` namespace — Application tracks branch `main` |

**Deployment flow (Git + GitOps):**

```text
feature/* → dev → test → main
```

1. Developers work on **feature branches** and open PRs into **`dev`** (or push to `dev` per team policy).  
2. After validation on the **development** cluster, merge **`dev` → `test`** for staging/QA.  
3. After approval, merge **`test` → `main`** for production.

Argo CD watches the matching branch and namespace; image tags in `K8s-Files/helm/boutique/values.yaml` are bumped by CI on the **same branch** that triggered the build.

---

## GitHub Actions (CI)

Workflow: **`.github/workflows/microservices-ci.yml`**

| Trigger | Intended environment |
|---------|----------------------|
| Push to **`dev`** (paths: `Src/src/**` or workflow file) | Development — build/push images; bump Helm tags on **`dev`** |
| Push to **`test`** | Staging/QA — same pipeline; bump on **`test`** for Argo staging |
| Push to **`main`** | Production — same pipeline; bump on **`main`** for Argo prod |
| **workflow_dispatch** | Manual build for one selected service on the branch you choose in the UI |

**Required secrets (repository → Settings → Secrets and variables → Actions):** `DOCKERHUB_USERNAME`, `DOCKERHUB_TOKEN` (do not commit real tokens).

---

## Argo CD (quick reference)

Register app source:

- **Repository:** `https://github.com/Ahmedhessn/Project-on-prim---K8s.git`  
- **Path:** `K8s-Files/helm/boutique`  
- **Revision:** `dev` | `test` | `main` per environment  

See `K8s-Files/argocd/applications/` for example `Application` manifests.
