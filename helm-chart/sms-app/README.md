# SMS Spam Detection Application - Helm Chart

This Helm chart deploys the SMS spam detection application (app + model service) to a Kubernetes cluster.

## Prerequisites

- Kubernetes cluster (v1.20+)
- Helm 3.x installed
- Ingress Controller (e.g., Nginx) installed and configured
- (Optional) Istio service mesh installed for advanced traffic management

## Installation

### Basic Installation

Install the chart with default values:

```bash
helm install sms-app ./helm-chart/sms-app
```

### Custom Installation

Install with custom hostname:

```bash
helm install sms-app ./helm-chart/sms-app \
  --set ingress.hosts[0].host=my-custom-domain.com
```

Install in a specific namespace:

```bash
helm install sms-app ./helm-chart/sms-app \
  --namespace my-namespace \
  --create-namespace \
  --set namespace=my-namespace
```

### Using Custom Values File

Create a custom `my-values.yaml`:

```yaml
app:
  replicaCount: 3
  image:
    tag: v1.2.3

ingress:
  hosts:
    - host: sms.example.com
      paths:
        - path: /
          pathType: Prefix
          serviceName: sms-app
          servicePort: 8080
```

Install with custom values:

```bash
helm install sms-app ./helm-chart/sms-app -f my-values.yaml
```

## Configuration

The following table lists the configurable parameters:

| Parameter | Description | Default |
|-----------|-------------|---------|
| `namespace` | Kubernetes namespace | `default` |
| `app.replicaCount` | Number of app replicas | `1` |
| `app.image.repository` | App image repository | `ghcr.io/doda25-team16/app` |
| `app.image.tag` | App image tag | `latest` |
| `model.replicaCount` | Number of model replicas | `1` |
| `model.image.repository` | Model image repository | `ghcr.io/doda25-team16/model-service` |
| `model.image.tag` | Model image tag | `latest` |
| `ingress.enabled` | Enable ingress | `true` |
| `ingress.className` | Ingress class name | `nginx` |
| `ingress.hosts[0].host` | Hostname for the application | `team16-sms.local` |

## Accessing the Application

After installation, add the hostname to your `/etc/hosts` file (for local testing):

```bash
# Get the ingress IP
kubectl get ingress -n <namespace>

# Add to /etc/hosts
<INGRESS_IP> team16-sms.local
```

Then access the application at: `http://team16-sms.local`

## Upgrading

```bash
helm upgrade sms-app ./helm-chart/sms-app
```

## Uninstalling

```bash
helm uninstall sms-app
```

## Troubleshooting

### Pods not starting

Check pod logs:
```bash
kubectl logs -l app=sms-app
kubectl logs -l app=sms-model
```

### Ingress not working

Verify ingress controller is running:
```bash
kubectl get pods -n ingress-nginx
```

Check ingress resource:
```bash
kubectl describe ingress sms-app-ingress
```

### Image pull errors

Ensure you have access to the GitHub Container Registry images. You may need to create an image pull secret if the images are private.

## Support

For issues or questions, please contact the team or create an issue in the repository.
