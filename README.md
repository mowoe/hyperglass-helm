# Hyperglass Helm Charts
[![Latest stable release of the Helm chart](https://img.shields.io/badge/dynamic/yaml.svg?label=stable&url=https://mowoe.github.io/hyperglass-helm/index.yaml&query=$.entries.hyperglass[0].version&color=1b53c2&logo=helm)](https://mowoe.github.io/hyperglass-helm)

## Overview

This repository contains helmcharts for a [hyperglass](https://hyperglass.dev/) deployment. The deployment includes a redis caching instance as well as a configurable [`hyperglass.yaml`](https://hyperglass.dev/docs/parameters) configuration file which is stored as a kubernetes ConfigMap and is later mounted as a file into the containers.
## Installation
Consult the [official documentation]() on how to setup hyperglass, especially in regards to creating a `devices.yaml` config. Once you have created this config, you are ready to deploy hyperglass using this helmchart.

### Deployment via `helm` and `kubectl`
First, create a namespace in which hyperglass will be deployed:
```bash
kubectl create namespace hyperglass
```
Then, we can inject the `devices.yaml` as a configmap into the namespace. Please note the naming, as the deployment will look for a configmap called `devices-config` in the namespace.
```bash
kubectl create -n hyperglass configmap devices-config --from-file=./devices.yaml
```
Then we can install hyperglass in the namespace:
```bash
helm repo add mowoe-hyperglass https://mowoe.github.io/hyperglass-helm
helm install --set primary_asn=123456 --set site_title="My cool Lookingglass" -n hyperglass mylookingglass mowoe-hyperglass/hyperglass
```
#### Additional configuration
Take a look at the [`values.yaml`](/charts/hyperglass/values.yaml) for additional parameters like changing the appearance and different configuration parameters. You can supply these option when deploying the helmchart (see [the helm documentation](https://helm.sh/docs/helm/helm_install/) on how to use the `--set` parameter for `helm install`).

### Network configuration
Note that the default deployment will only use a `NodePort`-Service because the intended way to deploy this, is to have a seperate loadbalancer like traefik in place. If you want to directly assign a loadbalancer to the service you can change this to `LoadBalancer`.

#### Additional Information
Due to some unfourtunate design choices in hyperglass itself, the pods take quite some time (~1 min) to be ready, as they will always build the ui again upon start. After the first deployment, this shouldnt be an issue though, as the service will only hand off traffic to new replica sets as soon as the pods are fully ready (like you would expect).