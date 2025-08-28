# Uptime Kuma Helm Chart

[![Helm Release](https://github.com/SKILLAB-DevOps/helm-uptime-kuma/actions/workflows/helm-release.yml/badge.svg)](https://github.com/SKILLAB-DevOps/helm-uptime-kuma/actions/workflows/helm-release.yml)

A production-ready Helm chart for deploying [Uptime Kuma](https://github.com/louislam/uptime-kuma), a self-hosted monitoring tool, on Kubernetes.

## Features

- Easy deployment with sensible defaults
- Security-hardened configuration
- Production-ready resource limits and health checks
- Persistent storage with configurable options
- Ingress support with TLS
- Automated CI/CD with GitHub Actions
- Horizontal Pod Autoscaler support
- Built-in testing

## Quick Start

### Prerequisites

- Kubernetes 1.19+
- Helm 3.2.0+
- Storage provisioner (for persistent volumes)

### Installation

```bash
# Add the repository (replace with your actual OCI registry)
helm registry login ghcr.io

# Install the chart
helm install uptime-kuma oci://ghcr.io/SKILLAB-DevOps/charts/uptime-kuma

# Or install with custom values
helm install uptime-kuma oci://ghcr.io/SKILLAB-DevOps/charts/uptime-kuma -f my-values.yaml
```

### Access the Application

```bash
# Port forward to access locally
kubectl port-forward deployment/uptime-kuma 3001:3001

# Access at http://localhost:3001
```

## Chart Development

### Local Development

```bash
# Clone the repository
git clone https://github.com/SKILLAB-DevOps/helm-uptime-kuma.git
cd helm-uptime-kuma

# Lint the chart
helm lint uptime-kuma/

# Test template rendering
helm template test-release uptime-kuma/ --debug

# Package the chart
helm package uptime-kuma/

# Install locally
helm install my-uptime-kuma ./uptime-kuma/
```

### Publishing

The repository includes automated CI/CD that lints and tests the chart on every PR and packages and publishes to GHCR on releases.

Manual push command:
```bash
# Package the chart
helm package uptime-kuma/

# Push to OCI registry
helm push uptime-kuma-1.0.0.tgz oci://ghcr.io/SKILLAB-DevOps/charts/uptime-kuma
```

## Configuration

See [uptime-kuma/README.md](uptime-kuma/README.md) for detailed configuration options.

### Common Configurations

#### Enable Ingress

```yaml
ingress:
  enabled: true
  className: "nginx"
  annotations:
    cert-manager.io/cluster-issuer: "letsencrypt-prod"
  hosts:
    - host: uptime.yourdomain.com
      paths:
        - path: /
          pathType: Prefix
  tls:
    - secretName: uptime-tls
      hosts:
        - uptime.yourdomain.com
```

#### Increase Resources

```yaml
resources:
  limits:
    cpu: 1000m
    memory: 1Gi
  requests:
    cpu: 200m
    memory: 256Mi

persistence:
  size: 10Gi
  storageClassName: "fast-ssd"
```

#### Use Existing PVC

```yaml
persistence:
  enabled: true
  existingClaim: "my-existing-pvc"
```

## Troubleshooting

### Common Issues

1. **Pod stuck in pending**: Check PVC creation and storage class
2. **Readiness probe failures**: Increase `initialDelaySeconds` for slow clusters
3. **Permission denied**: Verify security context settings

### Debugging Commands

```bash
# Check pod status
kubectl get pods -l app.kubernetes.io/name=uptime-kuma

# View logs
kubectl logs deployment/uptime-kuma

# Describe pod for events
kubectl describe pod -l app.kubernetes.io/name=uptime-kuma

# Check PVC
kubectl get pvc
kubectl describe pvc uptime-kuma
```

## Security

This chart implements security best practices including non-root user execution, read-only root filesystem where possible, dropped ALL capabilities, and security context constraints.

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test the chart locally
5. Submit a pull request

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
