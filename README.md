# Astronomer's Helm Chart for Apache Airflow 

[Apache Airflow](https://airflow.apache.org/) is a platform to programmatically author, schedule and monitor workflows. [Astronomer](https://www.astronomer.io/) is a software company built around Airflow. We have extracted this Helm Chart from our platform Helm chart and made it accessible under Apache 2 license.

## TL;DR

```console
helm install .
```

## Introduction

This chart will bootstrap an [Airfow](https://github.com/astronomer/astronomer/tree/master/docker/airflow) deployment on a [Kubernetes](http://kubernetes.io) cluster using the [Helm](https://helm.sh) package manager.

## Prerequisites

- Kubernetes 1.12+
- Helm 2.11+ or Helm 3.0-beta3+
- PV provisioner support in the underlying infrastructure

## Installing the Chart
To install the chart with the release name `my-release`:

```console
helm install --name my-release .
```

The command deploys Airflow on the Kubernetes cluster in the default configuration. The [Parameters](#parameters) section lists the parameters that can be configured during installation.

> **Tip**: List all releases using `helm list`

## Upgrading the Chart
To upgrade the chart with the release name `my-release`:

```console
helm upgrade --name my-release .
```

## Uninstalling the Chart

To uninstall/delete the `my-release` deployment:

```console
helm delete my-release
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## Updating DAGs

The recommended way to update your DAGs with this chart is to build a new docker image with the latest code (`docker build -t my-company/airflow:8a0da78 .`), push it to an accessible registry (`docker push my-company/airflow:8a0da78`), then update the Airflow pods with that image:

```console
helm upgrade my-release . \
  --set images.airflow.repository=my-company/airflow \
  --set images.airflow.tag=8a0da78
```

## Parameters

The following tables lists the configurable parameters of the Airflow chart and their default values.

| Parameter                                             | Description                                                                                                  | Default                                           |
| ----------------------------------------------------- | ------------------------------------------------------------------------------------------------------------ | ------------------------------------------------- |
| `uid`                                                 | UID to run airflow pods under                                                                                | `nil`                                             |
| `gid`                                                 | GID to run airflow pods under                                                                                | `nil`                                             |
| `nodeSelector`                                        | Node labels for pod assignment                                                                               | `{}`                                              |
| `affinity`                                            | Affinity labels for pod assignment                                                                           | `{}`                                              |
| `tolerations`                                         | Toleration labels for pod assignment                                                                         | `[]`                                              |
| `labels`                                              | Common labels to add to all objects defined in this chart                                                    | `{}`                                              |
| `privateRegistry.enabled`                             | Enable usage of a private registry for Airflow base image                                                    | `false`                                           |
| `privateRegistry.repository`                          | Repository where base image lives (eg: quay.io)                                                              | `~`                                               |
| `ingress.enabled`                                     | Enable Kubernetes Ingress support                                                                            | `false`                                           |
| `ingress.acme`                                        | Add acme annotations to Ingress object                                                                       | `false`                                           |
| `ingress.tlsSecretName`                               | Name of secret that contains a TLS secret                                                                    | `~`                                               |
| `ingress.baseDomain`                                  | Base domain for VHOSTs                                                                                       | `~`                                               |
| `ingress.class`                                       | Ingress class to associate with                                                                              | `nginx`                                           |
| `ingress.auth.enabled`                                | Enable auth with Astronomer Platform                                                                         | `true`                                            |
| `networkPolicies.enabled`                             | Enable Network Policies to restrict traffic                                                                  | `true`                                            |
| `airflowHome`                                         | Location of airflow home directory                                                                           | `/usr/local/airflow`                              |
| `rbacEnabled`                                         | Deploy pods with Kubernets RBAC enabled                                                                      | `true`                                            |
| `airflowVersion`                                      | Default Airflow image version                                                                                | `1.10.5`                                          |
| `executor`                                            | Airflow executor (eg SequentialExecutor, LocalExecutor, CeleryExecutor, KubernetesExecutor)                  | `KubernetesExecutor`                              |
| `allowPodLaunching`                                   | Allow airflow pods to talk to Kubernetes API to launch more pods                                             | `true`                                            |
| `defaultAirflowRepository`                            | Fallback docker repository to pull airflow image from                                                        | `astronomerinc/ap-airflow`                        |
| `defaultAirflowTag`                                   | Fallback docker image tag to deploy                                                                          | `1.10.7-alpine3.10`                               |
| `images.airflow.repository`                           | Docker repository to pull image from. Update this to deploy a custom image                                   | `astronomerinc/ap-airflow`                        |
| `images.airflow.tag`                                  | Docker image tag to pull image from. Update this to deploy a new custom image tag                            | `~`                                               |
| `images.airflow.pullPolicy`                           | PullPolicy for airflow image                                                                                 | `IfNotPresent`                                    |
| `images.flower.repository`                            | Docker repository to pull image from. Update this to deploy a custom image                                   | `astronomerinc/ap-airflow`                        |
| `images.flower.tag`                                   | Docker image tag to pull image from. Update this to deploy a new custom image tag                            | `~`                                               |
| `images.flower.pullPolicy`                            | PullPolicy for flower image                                                                                  | `IfNotPresent`                                    |
| `images.statsd.repository`                            | Docker repository to pull image from. Update this to deploy a custom image                                   | `astronomerinc/ap-statsd-exporter`                |
| `images.statsd.tag`                                   | Docker image tag to pull image from. Update this to deploy a new custom image tag                            | `~`                                               |
| `images.statsd.pullPolicy`                            | PullPolicy for statsd-exporter image                                                                         | `IfNotPresent`                                    |
| `images.redis.repository`                             | Docker repository to pull image from. Update this to deploy a custom image                                   | `astronomerinc/ap-redis`                          |
| `images.redis.tag`                                    | Docker image tag to pull image from. Update this to deploy a new custom image tag                            | `~`                                               |
| `images.redis.pullPolicy`                             | PullPolicy for redis image                                                                                   | `IfNotPresent`                                    |
| `images.pgbouncer.repository`                         | Docker repository to pull image from. Update this to deploy a custom image                                   | `astronomerinc/ap-pgbouncer`                      |
| `images.pgbouncer.tag`                                | Docker image tag to pull image from. Update this to deploy a new custom image tag                            | `~`                                               |
| `images.pgbouncer.pullPolicy`                         | PullPolicy for pgbouncer image                                                                               | `IfNotPresent`                                    |
| `images.pgbouncerExporter.repository`                 | Docker repository to pull image from. Update this to deploy a custom image                                   | `astronomerinc/ap-pgbouncer-exporter`             |
| `images.pgbouncerExporter.tag`                        | Docker image tag to pull image from. Update this to deploy a new custom image tag                            | `~`                                               |
| `images.pgbouncerExporter.pullPolicy`                 | PullPolicy for pgbouncer-exporter image                                                                      | `IfNotPresent`                                    |
| `env`                                                 | Environment variables key/values to mount into Airflow pods                                                  | `[]`                                              |
| `secret`                                              | Secret name/key pairs to mount into Airflow pods                                                             | `[]`                                              |
| `data.metadataSecretName`                             | Secret name to mount Airflow connection string from                                                          | `~`                                               |
| `data.resultBackendSecretName`                        | Secret name to mount Celery result backend connection string from                                            | `~`                                               |
| `data.metadataConection`                              | Field separated connection data (alternative to secret name)                                                 | `{}`                                              |
| `data.resultBakcnedConnection`                        | Field separated connection data (alternative to secret name)                                                 | `{}`                                              |
| `fernetKey`                                           | String representing an Airflow fernet key                                                                    | `~`                                               |
| `fernetKeySecretName`                                 | Secret name for Airlow fernet key                                                                            | `~`                                               |
| `workers.replicas`                                    | Replica count for Celery workers (if applicable)                                                             | `1`                                               |
| `workers.keda.enabed`                                 | Enable KEDA autoscaling features                                                                             | `false`                                           |
| `workers.keda.pollingInverval`                        | How often KEDA should poll the backend database for metrics in seconds                                       | `5`                                               |
| `workers.keda.cooldownPeriod`                         | How often KEDA should wait before scaling down in seconds                                                    | `30`                                              |
| `workers.keda.maxReplicaCount`                        | Maximum number of Celery workers KEDA can scale to                                                           | `10`                                              |
| `workers.persistence.enabled`                         | Enable log persistence in workers via StatefulSet                                                            | `false`                                           |
| `workers.persistence.size`                            | Size of worker volumes if enabled                                                                            | `100Gi`                                           |
| `workers.persistence.storageClassName`                | StorageClass worker volumes should use if enabled                                                            | `default`                                         |
| `workers.resources.limits.cpu`                        | CPU Limit of workers                                                                                         | `~`                                               |
| `workers.resources.limits.memory`                     | Memory Limit of workers                                                                                      | `~`                                               |
| `workers.resources.requests.cpu`                      | CPU Request of workers                                                                                       | `~`                                               |
| `workers.resources.requests.memory`                   | Memory Request of workers                                                                                    | `~`                                               |
| `workers.terminationGracePeriodSeconds`               | How long Kubernetes should wait for Celery workers to gracefully drain before force killing                  | `600`                                             |
| `workers.autoscaling.enabled`                         | Traditional HorizontalPodAutoscaler                                                                          | `false`                                           |
| `workers.autoscaling.minReplicas`                     | Minimum amount of workers                                                                                    | `1`                                               |
| `workers.autoscaling.maxReplicas`                     | Maximum amount of workers                                                                                    | `10`                                              |
| `workers.targetCPUUtilization`                        | Target CPU Utilization of workers                                                                            | `80`                                              |
| `workers.targetMemoryUtilization`                     | Target Memory Utilization of workers                                                                         | `80`                                              |
| `workers.safeToEvict`                                 | Allow Kubernetes to evict worker pods if needed (node downscaling)                                           | `true`                                            |
| `scheduler.podDisruptionBudget.enabled`               | Enable PDB on Airflow scheduler                                                                              | `false`                                           |
| `scheduler.podDisruptionBudget.config.maxUnavailable` | MaxUnavailable pods for scheduler                                                                            | `1`                                               |
| `scheduler.resources.limits.cpu`                      | CPU Limit of scheduler                                                                                       | `~`                                               |
| `scheduler.resources.limits.memory`                   | Memory Limit of scheduler                                                                                    | `~`                                               |
| `scheduler.resources.requests.cpu`                    | CPU Request of scheduler                                                                                     | `~`                                               |
| `scheduler.resources.requests.memory`                 | Memory Request of scheduler                                                                                  | `~`                                               |
| `scheduler.airflowLocalSettings`                      | Custom Airflow local settings python file                                                                    | `~`                                               |
| `scheduler.safeToEvict`                               | Allow Kubernetes to evict scheduler pods if needed (node downscaling)                                        | `true`                                            |
| `webserver.livenessProbe.initialDelaySeconds`         | Webserver LivenessProbe initial delay                                                                        | `15`                                              |
| `webserver.livenessProbe.timeoutSeconds`              | Webserver LivenessProbe timeout seconds                                                                      | `30`                                              |
| `webserver.livenessProbe.failureThreshold`            | Webserver LivenessProbe failure threshold                                                                    | `20`                                              |
| `webserver.livenessProbe.periodSeconds`               | Webserver LivenessProbe period seconds                                                                       | `5`                                               |
| `webserver.readinessProbe.initialDelaySeconds`        | Webserver ReadinessProbe initial delay                                                                       | `15`                                              |
| `webserver.readinessProbe.timeoutSeconds`             | Webserver ReadinessProbe timeout seconds                                                                     | `30`                                              |
| `webserver.readinessProbe.failureThreshold`           | Webserver ReadinessProbe failure threshold                                                                   | `20`                                              |
| `webserver.readinessProbe.periodSeconds`              | Webserver ReadinessProbe period seconds                                                                      | `5`                                               |
| `webserver.replicas`                                  | How many Airflow webserver replicas should run                                                               | `1`                                               |
| `webserver.resources.limits.cpu`                      | CPU Limit of webserver                                                                                       | `~`                                               |
| `webserver.resources.limits.memory`                   | Memory Limit of webserver                                                                                    | `~`                                               |
| `webserver.resources.requests.cpu`                    | CPU Request of webserver                                                                                     | `~`                                               |
| `webserver.resources.requests.memory`                 | Memory Request of webserver                                                                                  | `~`                                               |
| `webserver.jwtSigningCertificateSecretName`           | Name of secret to mount Airflow Webserver JWT singing certificate from                                       | `~`                                               |
| `webserver.defaultUser`                               | Optional default airflow user information                                                                    | `{}`                                              |


Specify each parameter using the `--set key=value[,key=value]` argument to `helm install`. For example,

```console
helm install --name my-release \
  --set executor=CeleryExecutor \
  --set enablePodLaunching=false .
```

##  Autoscaling with KEDA

KEDA stands for Kubernetes Event Driven Autoscaling. [KEDA](https://github.com/kedacore/keda) is a custom controller that allows users to create custom bindings
to the Kubernetes [Horizontal Pod Autoscaler](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/).
We've built an experimental scaler that allows users to create scalers based on postgreSQL queries. For the moment this exists
on a seperate branch, but will be merged upstream soon. To install our custom version of KEDA on your cluster, please run

```console
helm repo add kedacore https://kedacore.github.io/charts

helm repo update

helm install \
    --set image.keda=docker.io/kedacore/keda:1.2.0 \
    --set image.metricsAdapter=docker.io/kedacore/keda-metrics-adapter:1.2.0 \
    --namespace keda --name keda kedacore/keda
```

Once KEDA is installed (which should be pretty quick since there is only one pod). You can try out KEDA autoscaling 
on this chart by setting `workers.keda.enabled=true` your helm command or in the `values.yaml`. 
(Note: KEDA does not support StatefulSets so you need to set `worker.persistence.enabled` to `false`)

```console
helm install \
    --name airflow \
    --set executor=CeleryExecutor \
    --set workers.keda.enabled=true \
    --set workers.persistence.enabled=false \
    --namespace airflow \
    -f values.yaml .
```

## Contributing

Check out [our contributing guide!](CONTRIBUTING.md)
