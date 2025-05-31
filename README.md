# Vault External Secrets Tutorial

This repo contains all the code needed to follow along with our **[YouTube Tutorial](https://youtu.be/CF6ARIXdA4A)** or **[Written Article](https://kubernetestraining.io/blog/hashicorp-vault-kubernetes-create-external-secrets)**.

## Prerequisites

To follow along with this tutorial, you'll need:

- kubectl installed and configured ([https://youtu.be/IBkU4dghY0Y](https://youtu.be/IBkU4dghY0Y))
- Helm installed: ([https://kubernetestraining.io/blog/installing-helm-on-mac-and-windows](https://kubernetestraining.io/blog/installing-helm-on-mac-and-windows))

**Become a Cloud and DevOps Engineer:** https://rayanslim.com

## Install ESO

```bash
# Add the External Secrets Helm repository
helm repo add external-secrets https://charts.external-secrets.io

# Update Helm repositories
helm repo update

# Install External Secrets Operator
helm install external-secrets external-secrets/external-secrets \
  --namespace external-secrets \
  --create-namespace \
  --version 0.15.0
```

## Creating Authentication for Vault

```bash
# Create a namespace for our demo
kubectl create namespace demo

# Create a Kubernetes secret with the Vault token
kubectl create secret generic vault-token \
  --namespace demo \
  --from-literal=token=root
  ```
## Test using pod

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: api-consumer
  namespace: demo
spec:
  containers:
  - name: api-consumer
    image: busybox
    command: ['sh', '-c', 'echo "Using API Key: $API_KEY" && sleep 3600']
    env:
    - name: API_KEY
      valueFrom:
        secretKeyRef:
          name: dev-api-credentials
          key: api-key
```

## Become a Cloud and DevOps Engineer

Learn every tool that matters: https://rayanslim.com

## If you will run this in a Kubernetes cluster, then please run the  vault deployment and the vault service in the demo namespace 
**
kubectl apply -n demo -f vault-deployment.yaml
kubectl apply -n demo -f vault-service.yaml
