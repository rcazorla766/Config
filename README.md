# MLOps GitOps Configuration Repository

Repositorio de configuración declarativa para el despliegue del TFM:

**"Uso de GitOps para la Gobernanza de Modelos de IA en Producción (MLOps + DevOps)"**

---

## Estructura

```
Config/
├── base/                  # Manifiestos Kustomize base
├── overlays/
│   ├── dev/
│   └── prod/              # Entorno productivo (gobernado por Argo CD)
├── argocd/
│   ├── application.yaml   # Application de Argo CD
│   └── project.yaml       # AppProject (opcional)
├── SPRINT2_REPORT.md
└── SPRINT3_REPORT.md
```

---

## GitOps con Argo CD (Sprint 3)

El despliegue productivo está **gobernado por Git**. No se usa `kubectl apply -k` para la aplicación.

### Application

| Campo | Valor |
|---|---|
| Nombre | `ai-house-predictor-prod` |
| Repositorio | `https://github.com/rcazorla766/Config.git` |
| Path | `overlays/prod` |
| Branch | `main` |
| Namespace destino | `mlops-house-predictor-prod` |
| Auto-sync | `prune: true`, `selfHeal: true` |

### Acceso a Argo CD UI

```bash
kubectl -n argocd get secret argocd-initial-admin-secret \
  -o jsonpath="{.data.password}" | base64 -d
# Usuario: admin
```

### Bootstrap (solo una vez)

```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
kubectl apply -f argocd/application.yaml
```

---

## Flujo GitOps

1. Desarrollador modifica manifiestos en `overlays/prod/`.
2. `git commit` + `git push` a `main`.
3. Argo CD detecta el cambio en Git.
4. Sincroniza automáticamente el clúster.
5. Kubernetes aplica RollingUpdate.

---

## CI/CD → GitOps

El pipeline de **GitHub Actions** en el repo `App` actualiza automáticamente la imagen en `overlays/prod`:

```
App (push main) → pytest → GHCR → kustomize edit set image → commit Config → Argo CD sync
```

| Campo prod overlay | Valor |
|---|---|
| Registry | `ghcr.io/rcazorla766/ai-house-predictor` |
| Tag | Short SHA del commit de App (actualizado por CI) |
| `imagePullPolicy` | `Always` |

**Secreto requerido en repo App:** `CONFIG_REPO_TOKEN`

---

## Documentación

- Sprint 2 (Kubernetes): [`SPRINT2_REPORT.md`](SPRINT2_REPORT.md)
- Sprint 3 (GitOps): [`SPRINT3_REPORT.md`](SPRINT3_REPORT.md)
- Sprint 4 (CI/CD): [`../App/SPRINT4_REPORT.md`](../App/SPRINT4_REPORT.md)
