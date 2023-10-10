# Hyperglass Helm Charts

[![Latest stable release of the Helm chart](https://img.shields.io/badge/dynamic/yaml.svg?label=stable&url=https://mowoe.github.io/hyperglass-helm/index.yaml&query=$.entries.hyperglass[0].version&color=1b53c2&logo=helm)](https://mowoe.github.io/hyperglass-helm)

> [!WARNING]  
> Please note that hyperglass is currently not maintained and uses a lot of software that is EOL since a long time (e.g. see [this](https://github.com/thatmattlove/hyperglass/issues/226)). Hyperglass should not be considered a safe piece of software. This helmchart protects you to some degree, as the application cant escape out of the containers, but you should still think about this, especially since hyperglass may have ssh keys to your core infrastructure!

## Overview

This repository contains helmcharts for a [hyperglass](https://hyperglass.dev/) deployment. The deployment includes a redis caching instance as well as a configurable [`hyperglass.yaml`](https://hyperglass.dev/docs/parameters) configuration file which is stored as a kubernetes ConfigMap and is later mounted as a file into the containers.

## Prerequisites

Consult the [official documentation]() on how to setup hyperglass, especially in regards to creating a `devices.yaml` config and a `hyperglass.yaml`. Once you have created these configs, you are ready to deploy hyperglass using this helmchart.

> [!IMPORTANT]  
> Please note that it is absolutely mandatory that you have the following two lines in your `hyperglass.yaml`:

```yaml
listen_address: "0.0.0.0"
listen_port: 8080
cache:
  database: 0
  host: 127.0.0.1
  port: 6379
  show_text: true
  timeout: 120
```

## Deployment via `helm` and `kubectl`

Hyperglass is mainly configured through two files: `hyperglass.yaml` and `devices.yaml`.

As the `devices.yaml` and `hyperglass.yaml` file differs greatly from deployment to deployment, the user is expected to pass a valid yaml object in `values.devices_config` and `values.hyperglass_config`.

#### Installation

- Have your `devices.yaml` and `hyperglass.yaml` ready and in the current directory

Add the helm repo:

```bash
helm repo add mowoe-hyperglass https://mowoe.github.io/hyperglass-helm
```

And install the chart with the `devices.yaml`:

```bash
helm install \
    --set-file server_config=hyperglass.yaml \
    --set-file devices_config=devices.yaml \
    --create-namespace -n hyperglass \
    my-cool-lookingglass \
    mowoe-hyperglass/hyperglass
```

(Omit the `--create-namespace -n hyperglass` flags if youre on RedHat Openshift)

### Network configuration

Note that the default deployment will only use a `NodePort`-Service because the intended way to deploy this, is to have a seperate loadbalancer like traefik in place. If you want to directly assign a loadbalancer to the service you can change this to `LoadBalancer`.

#### Additional Information

Due to some unfourtunate design choices in hyperglass itself, the pods take quite some time (~1 min) to be ready, as they will always build the ui again upon start. After the first deployment, this shouldnt be an issue though, as the service will only hand off traffic to new replica sets as soon as the pods are fully ready (like you would expect).
