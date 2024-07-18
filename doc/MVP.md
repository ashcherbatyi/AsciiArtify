## Introduction

This document provides a demonstration of the application deployment using ArgoCD. The goal is to show how ArgoCD automatically tracks and synchronizes changes from the Git repository and deploys them to the Kubernetes cluster.

## Prerequisites

- Kubernetes cluster with kind
- ArgoCD installed and configured
- Git repository for the application: [go-demo-app](https://github.com/den-vasyliev/go-demo-app)

## Steps

### 1. Configure ArgoCD to Track the Git Repository

Create an ArgoCD application that tracks the Git repository:

```bash
cat <<EOF >> go-demo-app.yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: go-demo-app
  namespace: argocd
spec:
  project: default
  source:
    repoURL: 'https://github.com/den-vasyliev/go-demo-app'
    path: helm
    targetRevision: HEAD
  destination:
    server: 'https://kubernetes.default.svc'
    namespace: demo
  syncPolicy:
    automated: {}
    syncOptions:
      - CreateNamespace=true
EOF
```

Apply the ArgoCD application configuration:

```bash
kubectl apply -f go-demo-app.yaml
```

### 2. Verify the Deployment

Check the status of the pods in the `demo` namespace:

```bash
kubectl get pods -n demo
```

All pods should be in the `Running` state.

### 3. Access the Application

Set up port-forwarding to access the application:

```bash
kubectl port-forward -n demo svc/ambassador 8088:80
```

### 4. Test the Application

After port-forwarding is set up, you can send an image to the service and display it in the console:

```bash
curl -F 'image=@/tmp/g.png' localhost:8088/img/
```

Replace `/tmp/g.png` with the path to your image file.

## Conclusion

You have successfully set up an ArgoCD application to track and deploy changes from the Git repository. You can now observe how ArgoCD automatically synchronizes changes and deploys them to the Kubernetes cluster.

![Image](/data/demo1.gif)