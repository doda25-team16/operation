# SMS Checker Application Cluster Operation

This repository contains configuration files and documentation for deploying the SMS Checker application. This `README.md` focusses mainly on concrete deployment steps, while there the `extension.md` and `deployment.md` provide further documentation on the architecture and future extensions. The application consists of two services:

- **`sms-app`** (Java/Spring Boot) - Main application serving the web UI and API
- **`sms-model`** (Python/Flask) - Machine learning model service for SMS classification

## Deployment options
There are 2 ways to deploy the application:
1. Run the apps locally with `docker compose`
2. Run the apps on a k8s cluster manually with `kubectl` or with `helm` (recommended)

---

### Option 1: Docker Compose

#### Requirements
1. Docker & Docker Compose: Must be installed and running on the host machine
2. Built Images: The following application images must be built:
    - `sms-app` (from the `app` repository)
    - `sms-model` (from the `model-service` repository)

    To build these images, navigate to their respective source directories and run:
    ```
    docker build -t sms-app .
    docker build -t sms-model .
    ```

#### Starting the application
Run the application with default settings:
```
docker compose up -d
```
This exposes the app frontend on port `8080` and the model on port `8081`. Once both services are started up healthy, the web UI can be accessed at http://localhost:8080/sms

#### Custom configuration
The services are configured to be flexible using environment variables. You can override the default ports by setting them in your terminal session or by setting them in a .env file before running docker compose.

| Variable  | Service  | Default  | Description  |
|---|---|---|---|
| `SERVER_PORT`  | `sms-app`  | `8080`  | Port the app service will listen on for the user browser.  |
| `MODEL_PORT`  | `sms-model`  | `8081`  | Port the model service will listen on for the frontend requests.  |

**Example: Custom ports via terminal**
```bash
SERVER_PORT=9000 MODEL_PORT=8090 docker compose up -d
```
Access at: http://localhost:9000/sms

**Example: Custom ports via `.env` file**
```env
SERVER_PORT=9000
MODEL_PORT=8090
```
Then simply use the default command to start the application up:
```bash
docker compose up -d
```

The file will automatically grab the necessary environment variables to run the program. If no `.env` is found it will use the default variables.

After running the example, again access at: http://localhost:9000/sms, depending on which port you assigned to the server in the `.env`.

#### Stopping the application
To stop and remove the running containers:
```
docker compose down
```

---

### Option 2: Kubernetes cluster deployment

#### Provisioning the cluster
This step uses Vagrant and Ansible to provision a production-like cluster. Note this does not deploy the applications, but simply makes the cluster ready for deployment.

```bash
cd operation
vagrant up
```

This runs the `general.yaml`, `ctrl.yaml` and `node.yaml` playbooks.

**Finalization step**
After `vagrant up` completes, run the `finalization.yaml` playbook manually:
```bash
# from the operation root folder
ansible-playbook -i ansible/inventory.cfg ansible/finalization.yaml
```

At this point the following is provisioned, and the cluster is ready for deployment:
- 1 control plane node (`ctrl`) at `192.168.56.100`
- worker nodes, by default 2: `node-1` & `node-2` at `192.168.56.101-102`
- MetalLB load balancer
- Nginx Ingress Controller
- Istio service mesh
- Prometheus & Grafana for monitoring & alerting
- Kubernetes Dashboard

#### Application deployment

**Method A: Helm Chart (recommended)**
Use Helm to manage the deployment:
```bash
# SSH into the ctrl node and run it from there
vagrant ssh ctrl
helm install sms-app vagrant/helm-chart/sms-app
```
further commands if needed:
```bash
# or with kubectl configured locally (note that now you dont need the vagrant dir)
helm install sms-app ./helm-chart/sms-app

# Update configuration
helm upgrade sms-app ./helm-chart/sms-app --set app.replicas=5

# Rollback if needed
helm rollback sms-app 1

# Uninstall
helm uninstall sms-app
```
Then add our url to the hosts file:
```bash
echo "192.168.56.90 team16-sms.local" | sudo tee -a /etc/hosts
```

Check [helm-chart/sms-app/README.md](helm-chart/sms-app/README.md) for detailed information about the helm chart config.

**Method B: kubectl manifests (manual, not recommended)**
Deploy all k8s manifests manually using `kubectl`:
```bash
kubectl apply -f k8s/configmap.yml
kubectl apply -f k8s/secret.yml
kubectl apply -f k8s/sms-app-deployment.yml
kubectl apply -f k8s/sms-app-service.yml
kubectl apply -f k8s/sms-model-deployment.yml
kubectl apply -f k8s/sms-model-service.yml
kubectl apply -f k8s/ingress.yml
kubectl apply -f k8s/istio.yml
kubectl apply -f k8s/monitoring.yml
kubectl apply -f k8s/prometheus-rules.yml
kubectl apply -f k8s/alertmanager.yml
```

Then add our url to the known hosts file:
```bash
echo "192.168.56.90 team16-sms.local" | sudo tee -a /etc/hosts
```

*Important: if you are switching between Helm deployments and manual manifest deployments, make sure to clean up the whole application. `helm uninstall sms-app` should do the trick, otherwise manually apply `kubectl delete` all resources.*

#### Accessing the application
Access the app:
- when added to hosts file http://team16-sms.local/sms/
- or simply http://192.168.56.91/sms/

#### Debugging and verifying
**Nginx Ingress**
http://192.168.56.90
  
**Istio Gateway**:
http://192.168.56.91

**Direct Service Access (for debugging):**
```bash
# Port-forward to services from ctrl node
kubectl port-forward svc/sms-app 8080:8080
kubectl port-forward svc/sms-model 8081:8081
```

**Verify Deployment:**
```bash
# Check pod status
kubectl get pods

# Check services and their IPs
kubectl get svc

# Check ingress status
kubectl get ingress

# View application logs
kubectl logs -l app=sms-app
kubectl logs -l app=sms-model
```

---

## Documentation
Further documentation can be found here:
- [deployment.md](deployment.md) - Detailed architecture and networking
- [extension.md](extension.md) - Advanced features to be implemented
- [helm-chart/sms-app/README.md](helm-chart/sms-app/README.md) - Helm chart specific usage info

---

## Attribution

This documentation was created with the assistance of generative AI. We provided all ideas, structural sections, and specific technical requirements, while some AI was used for refining the language and clean markdown syntax.
