# Uptime Kuma Helm Chart

A Helm chart for deploying Uptime Kuma, a self-hosted monitoring tool, on Kubernetes.

## Installation

```bash
helm install uptime-kuma oci://ghcr.io/skillab-devops/charts/uptime-kuma
```

## Configuration

| Parameter | Description | Default |
|-----------|-------------|---------|
| `replicaCount` | Number of replicas | `1` |
| `image.repository` | Image repository | `louislam/uptime-kuma` |
| `image.tag` | Image tag | `1.23.16-debian` |
| `service.type` | Service type | `ClusterIP` |
| `service.port` | Service port | `3001` |
| `ingress.enabled` | Enable ingress | `false` |
| `persistence.enabled` | Enable persistent storage | `true` |
| `persistence.size` | Storage size | `4Gi` |
| `resources.limits.cpu` | CPU limit | `500m` |
| `resources.limits.memory` | Memory limit | `512Mi` |

## Example Values

```yaml
# Enable ingress
ingress:
  enabled: true
  className: "nginx"
  hosts:
    - host: uptime.example.com
      paths:
        - path: /
          pathType: Prefix

# Configure storage
persistence:
  size: 10Gi
  storageClassName: "fast-ssd"

# Set resources
resources:
  limits:
    cpu: 1000m
    memory: 1Gi
```

## Access

```bash
kubectl port-forward deployment/uptime-kuma 3001:3001
```

Access at http://localhost:3001
