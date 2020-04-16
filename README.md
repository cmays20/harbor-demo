# Run Harbor on Kubernetes with Self Signed Certificate
The goal of this demo is to install harbor into a kubernetes cluster and have it work correctly with Self Signed Certificates.

## Prerequisites
1. A Kubernetes cluster with kubectl connection
2. Able to install using Helm

## Create and Trust a Self Signed Certificate
### 1. Generate your own certificate
```bash
mkdir -p certs

openssl req \
-newkey rsa:4096 -nodes -sha256 -keyout certs/domain.key \
-x509 -days 365 -out certs/domain.crt
```
<b>NOTE:</b> Be sure to use the name `harbordomain.com` as a CN

### 2. Create the harbor-system namespace
```bash
kubectl create ns harbor-system
```

### 3. Create secret in Kubernetes
```bash
kubectl -n harbor-system create secret tls tls-harbor-ingress \
  --cert=certs/domain.crt \
  --key=certs/domain.key
```

### 4. Install Harbor using helm
```bash
helm install -f harbor-config.yaml --name harbor harbor/harbor
```
