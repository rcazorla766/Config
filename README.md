# MLOps GitOps Configuration Repository

Repositorio de configuración declarativa para el despliegue del TFM:

**"Uso de GitOps para la Gobernanza de Modelos de IA en Producción (MLOps + DevOps)"**

---

## Estructura (Sprint 2)

```
Config/
├── base/
│   ├── namespace.yaml
│   ├── deployment.yaml
│   ├── service.yaml
│   ├── configmap.yaml
│   ├── secret.example.yaml
│   └── kustomization.yaml
└── overlays/
    ├── dev/
    └── prod/
```

---

## Despliegue rápido

### Base (2 réplicas)

```bash
# Construir imagen local
cd ../App
docker build -t ai-house-predictor:1.0.0 .

# Desplegar manifiestos
kubectl apply -k ../Config/base

# Verificar
kubectl -n mlops-house-predictor get pods,deploy,svc
kubectl -n mlops-house-predictor rollout status deployment/ai-house-predictor
```

### Entorno dev (1 réplica)

```bash
kubectl apply -k overlays/dev
```

### Entorno prod (2 réplicas, más recursos)

```bash
kubectl apply -k overlays/prod
```

---

## Secret de ejemplo

El archivo `base/secret.example.yaml` es una **plantilla**. No se aplica en el despliegue base.

```bash
cp base/secret.example.yaml base/secret.yaml
# Editar valores y aplicar manualmente en fase GitOps con Sealed Secrets
```

---

## Próxima fase (Sprint 3)

- Argo CD Application apuntando a este repositorio
- Sincronización automática GitOps
- Gestión de secretos con Sealed Secrets / External Secrets

---

## Documentación

- Informe Sprint 2: [`SPRINT2_REPORT.md`](SPRINT2_REPORT.md)
