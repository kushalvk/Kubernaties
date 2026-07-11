# First-app-MERN Kubernetes Deployment

This folder contains Kubernetes manifests for running a simple MongoDB-backed web app in Minikube.

## Files

- `secret.yaml` creates a Secret with the MongoDB username and password.
- `mongo-config.yaml` creates a ConfigMap with the MongoDB service name.
- `mongo-app.yaml` creates the MongoDB Deployment and ClusterIP Service.
- `web-app.yaml` creates the web app Deployment and a NodePort Service.

## Command Breakdown

### 1. Create the Secret

```powershell
kubectl apply -f .\secret.yaml
```

This creates `mongo-secret`, which stores the MongoDB credentials used by both the MongoDB pod and the web app.

### 2. Create the ConfigMap

```powershell
kubectl apply -f .\mongo-config.yaml
```

This creates `mongo-config`, which provides the MongoDB service name (`mongo-service`) to the web app.

### 3. Deploy MongoDB

```powershell
kubectl apply -f .\mongo-app.yaml
```

This creates:

- `mongo-deployment` running the `mongo:6.0` container
- `mongo-service`, a ClusterIP Service that exposes MongoDB inside the cluster on port `27017`

The MongoDB container uses the credentials from `mongo-secret` through environment variables.

### 4. Deploy the Web App

```powershell
kubectl apply -f .\web-app.yaml
```

This creates:

- `webapp-deployment` running `mongo-express`
- `webapp`, a NodePort Service exposed on port `30200`

The web app reads:

- MongoDB username and password from `mongo-secret`
- MongoDB host from `mongo-config`

### 5. Get the Minikube IP

```powershell
minikube ip
```

This shows the Minikube node IP. In your output, it was `192.168.58.2`.

### 6. Check Pods

```powershell
kubectl get pod
```

This confirms that both pods are running:

- `mongo-deployment-...`
- `webapp-deployment-...`

### 7. Check Services

```powershell
kubectl get svc
```

This shows:

- `mongo-service` as a ClusterIP service on port `27017`
- `webapp` as a NodePort service on port `8081` mapped to `30200`

### 8. Open the Web App

```powershell
minikube service webapp
```

This opens the web app in the browser through a Minikube tunnel.

If the tunnel closes on Windows with the Docker driver, keep the terminal open while the service is running.

## Result

After applying all manifests, the app should be available through the `webapp` service, and MongoDB should be reachable internally through `mongo-service`.
