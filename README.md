# Hyperglass Helm Charts

## Installation
Have a `devices.yaml` ready.

Create a namespace
```bash
kubectl create namespace hyperglass
```
Create the `devices.yaml` configmap:
```bash
kubectl create -n hyperglass configmap devices-config --from-file=./devices.yaml
```
## Install hyperglass into the namespace
```
helm repo add mowoe-hyperglass https://mowoe.github.io/hyperglass-helm
helm install -n mowoe-hyperglass/hyperglass
```

## Installation with argocd