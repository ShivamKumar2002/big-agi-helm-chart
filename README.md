# Big-AGI Helm Chart (Unofficial)

Welcome to the Big-AGI Helm chart (Unofficial) repository! This helm chart is designed to deploy the [Big-AGI](https://github.com/enricoros/big-AGI) generative AI suite on a Kubernetes cluster.


## Overview

[Big-AGI](https://github.com/enricoros/big-AGI) is AI suite for professionals that need function, form, simplicity, and speed. Powered by the latest models from 12 vendors and open-source servers, big-AGI offers best-in-class Chats, Beams, and Calls with AI personas, visualizations, coding, drawing, side-by-side chatting, and more -- all wrapped in a polished UX. It features AI personas, AGI functions, multi-model chats, text-to-image, voice, response streaming, code highlighting and execution, PDF import, presets for developers, much more.

This Helm chart simplifies the deployment of Big-AGI on Kubernetes, allowing you to easily configure and manage your Big-AGI instances at scale. Whether you're running a small cluster or managing a large-scale deployment, this chart provides the flexibility and control you need.

This chart includes inbuilt support for [Browserless](https://github.com/browserless/browserless) and PostgreSQL with [CloudNativePG Operator](https://cloudnative-pg.io/).

 ## Features

- **Easy Deployment**: Quickly deploy Big-AGI on your Kubernetes cluster with just a few commands.
- **Customizable Configuration**: Tailor Big-AGI settings and APIs to your needs with a wide range of configurable options.
- **Inbuilt Browserless Support**: Support for seamless deployment of Browserless using my unofficial [Browserless Helm Chart](https://github.com/ShivamKumar2002/browserless-helm-chart).
- **PostgreSQL Integration**: Support for seamless deployment of PostgreSQL using the [CloudNativePG Operator](https://cloudnative-pg.io/), ensuring high availability and scalability.
- **Scalability**: Easily scale Big-AGI instances horizontally to handle increased workloads.
- **Health Checks**: Integrated health checks ensure that your Big-AGI instances are running smoothly.
- **Security**: Customizable security contexts and service accounts to align with your cluster's security policies.
- **Resource Management**: Define CPU and memory requests and limits to optimize resource utilization.


## Prerequisites

- A Kubernetes cluster (version 1.16 or higher)
- Helm 3.x installed on your local machine or CI/CD environment


## Quick Start

To get started with the Big-AGI Helm chart, follow these steps:

1. Add the my personal Helm repository:

```bash
helm repo add shivam-charts https://shivamkumar2002.github.io/helm-charts
helm repo update
```

2. Install the `big-agi` chart:

```bash
helm install big-agi shivam-charts/big-agi --namespace big-agi --create-namespace --values my-values.yaml
```

Replace `my-values.yaml` with the path to your custom values file if you have one.

3. Access Big-AGI through your Kubernetes service or Ingress, depending on your configuration.


## Configuration

The `values.yaml` file in the chart's directory contains the default configuration values. You can override these values by providing a custom `values.yaml` file or by setting individual parameters during the Helm installation or upgrade command.

For a detailed breakdown of the available configuration options, please refer to the [Values](#values) section of this README.

For detailed information on how to configure each aspect of the Big-AGI Helm chart, including service accounts, resource limits, ingress rules, and Big-AGI-specific settings, please see the [Values](#values) section of this README.

## Upgrading

To upgrade the Big-AGI deployment with new values or a new version of the chart, use the following Helm command:

```bash
helm upgrade big-agi shivam-charts/big-agi --namespace big-agi --values my-new-values.yaml
```

Ensure that `my-new-values.yaml` contains your updated configuration settings.


## Uninstallation

To remove the Big-AGI deployment from your Kubernetes cluster, execute the following command:

```bash
helm uninstall big-agi --namespace big-agi
```

This will delete all the resources associated with the big-agi Helm release.


## Values

The `values.yaml` file contains the default configuration values for the Big-AGI Helm chart. You can override these values by providing a custom `values.yaml` file or by setting individual parameters during the Helm installation or upgrade command.


### Basic Configuration

- `nameOverride`: Override the name of the chart.
- `fullnameOverride`: Override the full name of the chart.
- `clusterDomain`: The domain of the Kubernetes cluster (default: `cluster.local`).
- `replicaCount`: The number of Big-AGI pod replicas (default: `1`).
- `imagePullSecrets`: A list of secrets for authenticating with a Docker registry.

### Image Configuration

- `image.repository`: The Docker image repository for Big-AGI (default: `ghcr.io/enricoros/big-agi`).
- `image.tag`: The Docker image tag for Big-AGI (default: appVersion in chart).
- `image.pullPolicy`: The image pull policy (default: `Always`).

### Service Account

- `serviceAccount.create`: Specifies whether a Kubernetes service account should be created for Big-AGI (default: `true`).
- `serviceAccount.automount`: Automatically mount the service account's API credentials (default: `true`).
- `serviceAccount.annotations`: Annotations to add to the service account.
- `serviceAccount.name`: The name of the service account to use.

### Service and Exposure

- `service.type`: The type of Kubernetes service to create (default: `ClusterIP`).
- `service.port`: The port on which the Big-AGI service will be exposed (default: `3000`).
- `ingress.enabled`: Enable or disable the creation of an Ingress resource (default: `false`).
- `ingress.className`: The Ingress class name to use.
- `ingress.annotations`: Annotations to add to the Ingress resource.
- `ingress.hosts`: A list of hosts to configure for the Ingress resource.
- `ingress.tls`: TLS configuration for the Ingress resource.


### PostgreSQL

This chart allows you to deploy integrated PostgreSQL database using CloudNativePG Operator. See [Official Documentation](https://cloudnative-pg.io/documentation/current/) for more details.

CloudNative-PG Operator must be installed in cluster. Give following commands to install:

```bash
helm repo add cnpg https://cloudnative-pg.github.io/charts
helm upgrade --install cnpg \
  --namespace cnpg-system \
  --create-namespace \
  cnpg/cloudnative-pg
```

**NOTE:** This chart has its own minimal template for postgres cluster. In future we will switch to official chart (once managed roles support is released in chart). Till then, configuring everything is not possible.

Currently available configuration:

`postgres.enabled`: Enable or disable the deployment of a PostgreSQL instance alongside your Big-AGI application. When set to true, a PostgreSQL database will be provisioned.

`postgres.mode`: Choose the operational mode of PostgreSQL, which can be either standalone for a single-node deployment or cluster for a multi-node, highly available setup.

`postgres.cluster.instances`: Define the number of PostgreSQL instances to deploy when operating in cluster mode.

`postgres.cluster.imagePullPolicy`: Set the image pull policy for the PostgreSQL container images.

`postgres.cluster.enableSuperuserAccess`: Determine whether superuser access should be enabled for the database.

`postgres.cluster.roles`: Specify the roles and their associated permissions within the PostgreSQL cluster. Each role can be configured with login rights, superuser privileges, and membership in predefined roles.

`postgres.cluster.storage.size`: Allocate persistent storage size for the PostgreSQL database, ensuring enough space for your data needs.


### Browserless Configuration

This chart allows you to deploy integrated Browserless service using unofficial [Browserless Helm Chart](https://github.com/ShivamKumar2002/browserless-helm-chart) made by me. See readme at given chart repo for details and configuration.

`browserless.enabled`: Activate or deactivate the Browserless service within your deployment. Setting this to true will include Browserless in your Helm release.

For more configurable options, see the chart repo [Here](https://github.com/ShivamKumar2002/browserless-helm-chart). You can add values in `browserless` section and they will be applied.


### Resources and Scaling

- `resources`: Resource requests and limits for the Big-AGI pods.
- `autoscaling`: Horizontal pod autoscaling configuration.


### Autoscaling (HPA)

The Helm chart provides configuration options for Horizontal Pod Autoscaling (HPA), which can be used to automatically scale the number of pods based on the observed CPU or memory utilization. Below are the details of the autoscaling configuration settings:

| Configuration Setting                   | Default Value | Description                                                                                                                                                                                                  |
|------------------------------------------|---------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `autoscaling.enabled`                   | `false`      | Enables or disables the Horizontal Pod Autoscaler for the deployment. When `true`, the autoscaler manages the scaling of your pods.                                                                                   |
| `autoscaling.minReplicas`               | `1`          | The minimum number of replicas that the autoscaler will maintain. This is the lower limit for scaling operations.                                                                                                      |
| `autoscaling.maxReplicas`               | `5`          | The maximum number of replicas to which the autoscaler can scale up. This is the upper limit for the number of pods that can be created.                                                                                     |
| `autoscaling.targetCPUUtilizationPercentage` | `80`         | The target CPU utilization percentage that the autoscaler aims to maintain. The autoscaler scales up/down based on whether the CPU usage exceeds or falls below this value.                                                      |
| `autoscaling.targetMemoryUtilizationPercentage` | Not set | The target memory utilization percentage for the autoscaler to consider. If set, the autoscaler will scale based on memory usage in addition to CPU usage. This setting is optional and will not take effect unless specified by the user. |


### Additional Configurations

- `additionalEnvVars`: A map of additional environment variables to set for the Big-AGI pods.
- `podAnnotations`: Annotations to add to the Big-AGI pods.
- `podLabels`: Labels to add to the Big-AGI pods.
- `podSecurityContext`: The security context for the pod.
- `securityContext`: The security context for the container running Big-AGI.


### Advanced Configuration

- `volumes`: Additional volumes to attach to the Big-AGI pods.
- `volumeMounts`: Additional volume mounts for the Big-AGI container.
- `nodeSelector`: Node labels for pod placement.
- `tolerations`: Tolerations for pod scheduling on tainted nodes.
- `affinity`: Affinity and anti-affinity rules for pod placement.


## Contributing

We welcome contributions to this chart! If you have a feature request, bug report, or improvement, please open an issue or submit a pull request.


## Support

If you encounter any issues or have questions about this, please open an issue in this repository, and I will be happy to assist you.


## Notes

**NOTE:** This chart is not affiliated with official Big-AGI repo in any way.


## Credits

- [Big-AGI](https://github.com/enricoros/big-AGI)
- [Browserless](https://github.com/browserless/browserless)
- [CloudNativePG](https://cloudnative-pg.io)
- [Helm](https://helm.sh)
