
## Introduction

This document provides step-by-step instructions for deploying a GitOps system using ArgoCD on a Kubernetes cluster with kind. The goal is to set up ArgoCD and provide team access to its graphical interface.

## Prerequisites

- Podman installed
- kind installed
- kubectl installed

## Steps

### 1. Create a Kubernetes Cluster with kind

Create a kind configuration file `kind-config.yaml`:

```bash
cat <<EOF >> kind-config.yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
EOF
```

Create the Kubernetes cluster:

```bash
kind create cluster --config kind-config.yaml --name argo
```

Verify the cluster information:

```bash
kubectl cluster-info
```

### 2. Install ArgoCD

Create a namespace for ArgoCD:

```bash
kubectl create namespace argocd
```

Apply the ArgoCD installation manifest:

```bash
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

Check the status of the pods:

```bash
kubectl get pods -A
```

Wait until all the ArgoCD pods are in the Running state.

### 3. Access ArgoCD

#### Get the ArgoCD Admin Password

Retrieve the initial admin password:

```bash
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo
```

Copy the displayed password.

#### Port-forwarding to Access the ArgoCD UI

Set up port-forwarding to access the ArgoCD UI:

```bash
kubectl port-forward svc/argocd-server -n argocd 8080:443
```

Open your browser and navigate to [https://localhost:8080](https://localhost:8080).

Log in with the following credentials:
- **Username**: \`admin\`
- **Password**: the password you retrieved in the previous step.

## Conclusion

You have successfully set up a Kubernetes cluster with kind and deployed ArgoCD. You can now use the ArgoCD interface to manage your GitOps workflows.


