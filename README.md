# helm-emissary

Crossplane configuration that wraps both [Emissary CRDs](https://github.com/emissary-ingress/emissary) and [Emissary-ingress](https://www.getambassador.io/products/api-gateway) Helm charts, providing a unified interface for deploying the complete Emissary API Gateway on Kubernetes.

## Usage

### Minimal

```yaml
apiVersion: helm.hops.ops.com.ai/v1alpha1
kind: Emissary
metadata:
  name: emissary
  namespace: my-env
spec:
  clusterName: my-cluster
```

### With Custom Configuration

```yaml
apiVersion: helm.hops.ops.com.ai/v1alpha1
kind: Emissary
metadata:
  name: emissary
  namespace: my-env
spec:
  clusterName: my-cluster

  labels:
    team: platform

  crds:
    enabled: true
    namespace: emissary

  ingress:
    enabled: true
    namespace: emissary
    values:
      replicaCount: 2
      service:
        type: LoadBalancer
      resources:
        requests:
          cpu: 100m
          memory: 256Mi
```

### CRDs Only (for shared clusters)

```yaml
apiVersion: helm.hops.ops.com.ai/v1alpha1
kind: Emissary
metadata:
  name: emissary-crds
  namespace: my-env
spec:
  clusterName: my-cluster
  crds:
    enabled: true
  ingress:
    enabled: false
```

## Configuration

| Field | Description | Default |
|-------|-------------|---------|
| `spec.clusterName` | Target cluster name (used as default providerConfigRef) | Required |
| `spec.labels` | Labels applied to all resources | `{}` |
| `spec.providerConfigRef.name` | Helm ProviderConfig name | `<clusterName>` |
| `spec.providerConfigRef.kind` | Helm ProviderConfig kind | `ProviderConfig` |
| `spec.crds.enabled` | Install Emissary CRDs | `true` |
| `spec.crds.namespace` | Namespace for CRDs release | `emissary` |
| `spec.crds.name` | Helm release name for CRDs | `emissary-crds` |
| `spec.crds.values` | Helm values for CRDs (merged) | `{}` |
| `spec.ingress.enabled` | Install Emissary-ingress | `true` |
| `spec.ingress.namespace` | Namespace for ingress release | `emissary` |
| `spec.ingress.name` | Helm release name for ingress | `emissary` |
| `spec.ingress.values` | Helm values for ingress (merged) | `{}` |

## Chart Info

- **CRDs Chart**: `emissary-crds-chart` from `oci://ghcr.io/emissary-ingress`
- **Ingress Chart**: `emissary-ingress` from `oci://ghcr.io/emissary-ingress`
- **Version**: 3.10.0
