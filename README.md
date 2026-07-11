# Kubernaties Workspace

This root folder contains Kubernetes deployment files for a simple MongoDB-backed web app.

## Project Structure

- `First-app-MERN/` contains the Kubernetes manifests and the app-specific README.
- The manifests in that folder define the Secret, ConfigMap, MongoDB deployment, and web app deployment.

## What This Deployment Does

The deployment creates:

- a Secret for MongoDB credentials
- a ConfigMap for the MongoDB service name
- a MongoDB Deployment and ClusterIP Service
- a web app Deployment using `mongo-express`
- a NodePort Service to access the web app from outside the cluster

## Typical Deployment Flow

Run the manifests from inside `First-app-MERN`:

```powershell
kubectl apply -f .\secret.yaml
kubectl apply -f .\mongo-config.yaml
kubectl apply -f .\mongo-app.yaml
kubectl apply -f .\web-app.yaml
```

Then check the cluster state:

```powershell
kubectl get pod
kubectl get svc
minikube ip
```

To open the web app in the browser:

```powershell
minikube service webapp
```

## Notes

- Keep the Minikube terminal open when using the service tunnel on Windows with the Docker driver.
- For command-by-command explanations, see [First-app-MERN/README.md](First-app-MERN/README.md).
