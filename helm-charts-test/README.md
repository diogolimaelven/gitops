# 1P Helm Chart

This is a chart to deployment all aplications 1P
## TL;DR

helm repo add elvenworks https://[your-github-persion-token](https://docs.github.com/en/github/authenticating-to-github/keeping-your-account-and-data-secure/creating-a-personal-access-token)@raw.githubusercontent.com/elvenworks/helm-charts/master<br>
helm install my-release elvenworks/1p-chart -f values.yaml

## Introduction

This chart bootstraps Applications And Cronjobs to Kubernetes.

## Prerequisites

- Kubernetes 1.16+
- Helm 3.1.0

## Installing the Chart
To install the chart with the release name `my-release`:

```console
$ helm install my-release 1p-chart/1p-chart -f values.yaml
```

The command deploys applcation on the Kubernetes cluster in the default configuration. The [Parameters](#parameters) section lists the parameters that can be configured during installation.

> **Tip**: List all releases using `helm list`
## Parameters

The following tables lists the configurable parameters of the 1P chart and their default values.

| Parameter                                     | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                | Default                                                       |
|-----------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------|
| `replicaCount`                        | 	desired number of  pods.                                                                                                                                                                                                                                                                                                                                                                                         | `1`                                                         |
| `hpa.enabled`        | If true, creates Horizontal Pod Autoscaler. (`Only Production`)                                                                                                                                                                                                                                                                                                                                                                                              | `false`                                                         |
| `hpa.minReplicas`        | If hpa enabled, this field sets minimum replica count.                                                                                                                                                                                                                                                                                                                                                                                                       | `3`                                                         |
| `hpa.maxReplicas`            | If hpa enabled, this field sets maximum replica count.                                                                                                                                                                                                                                                                                                                                                                                      | `9`                                                         |
| `hpa.memory_max`        | Target the maximum memory utilization percentage to scale.                                                                                                                                                                                                                                                                                                                                                                                 | `80`                                                         |
| `hpa.cpu_max`               |	Target the maximum CPU utilization percentage to scale.                                                                                                                                                                                                                                                                                                                                                                             | `80`                                                         |
| `rollingUpdate.enabled`       | If true, update to take place with zero downtime by incrementally updating Pods instances with new ones. (`Only Work With replicaCount equal 3`)                                                                                                                                                                                                                                                                                                                                          | `true`                                                         |
| `rollingUpdate.maxSurge`                     | If rollingUpdate enabled, the maximum number of Pods that can be created over the desired number of Pods. The value can be an absolute number (for example, 5) or a percentage of desired Pods (for example, 10%).                                                                                                                                                                                                                                                                                                                                              | `1`     |
| `rollingUpdate.maxUnavailable`                         | If rollingUpdate enabled ,the maximum number of Pods that can be unavailable during the update process. The value can be an absolute number (for example, 5) or a percentage of desired Pods (for example, 10%.                                                                                                                                                                                                                                                                                                                                               | `1`                                                         |
| `image.repository`                              | Container image repository.                                                                                                                                                                                                                                                                                                                                                                                       | `740561009084.dkr.ecr.us-east-1.amazonaws.com/your-ms-name` (`AWS ElvenWorks`)                                                                                         |
|`image.tag`                                   | Container image tag.                                                                                                                                                                                                                                                                                                                                                                                                                                                     | `stable`                                                  |
| `image.pullPolicy`                            | Container image pull policy.                                                                                                                                                                                                                                                                                                                                                                               | `IfNotPresent`                                                                                                |
| `nameOverride`                                | String to partially override Fullname template with a string (will prepend the release name).                                                                                                                                                                                                                                                                                                                                                                    | `nil`                                                         |
| `fullnameOverride`                            | String to fully override Name template with a string.                                                                                                                                                                                                                                                                                                                                                                                                        | `nil`                                                         |
| `service.type`                   | 	type of service to create, allow values (`ClusterIP`, `NodePort`,`LoadBalancer`).                                                                                                                                                                                   | `ClusterIP`                                                       |
| `service.ports`            | Ports to be added to service (Multiple Allowed). | `nil`     
| `service.ports.{{portName}}`            | The name of the service port. | `http`     
| `service.ports.{{portName}}.protocol`            | The protocol of the port. | `TCP`     
| `service.ports.{{portName}}.port`            | The number of the port. | `80`     
| `service.ports.{{portName}}.targetPort`            | The number of the target port . | `3100`     
| `service.annotations`                 | annotations to be added to service.                                                                                                                                                                                                                                                                                                                                                                         | `prometheus.io/scrape: "true"`                                                        |
| `ingress.enabled`          |  If true, create nginx-ingress.                                                                                                                                                                                                                                                                                                                                                    | `false`                                                      |
| `ingress.annotations`         | annotations to be added to ingress, allow values (`nginx`, `nginx-internal`,  `nginx-public`).                                                                                                                                                                                                                                                                                                                                                                                                | `kubernetes.io/ingress.class: nginx`                                                          |
| `ingress.paths` | list of paths match the HTTP request in the Ingress objects.                                                                                                                                                                                                                                                                                                                                                                          | `/`                                                           |
| `ingress.hosts`                             | list of hosts match the HTTP request in the Ingress objects, common values wildcard (`my-ms.dev.elven.works`, `my-ms.st.elven.works`, `my-ms.elven.works`).                                                                                                                                                                                                                                                                                                                                                                                                                  | `test.elven.works`                                                       |
| `resources.limits.cpu`                                |CPU resource limit.                                                                                                                                                                                                                                                                                                                                                                                                                                                         | `75m`                                                       |
| `resources.limits.memory`                         | Memory resource limit.                                                                                                                                                                                                                                                                                                                                                                                                                            | `192Mi`                                                         |
| `resources.requests.cpu`                                    |  CPU resource request.                                                                                                                                                                                                                                                                                                                                                                           | `50m`                                                         |
| `resources.requests.memory`                                 | Memory resource request.                                                                                                                                                                                                                                                                                                    | `144Mi`                                                         |
| `env`                                   | any environment variables to set in the deployments and cronjobs pods.                                                                                                                                                                                                                                                                                                                                                                                                                               | `ELVEN_EXAMPLE: "1p-chart"`                                                         |
| `cronjob.enabled`                                 | If true, create just a cronjob.                                                                                                                                                                                                                                                                                                                                                                                                                                              | `false`                                                         |
| `cronjob.schedule`                                    | Scheduling cronjob time.                                                                                                                                                                                                                                                                                                                                                                                                                                         | `"*/10 * * * *"`                                                         |
| `cronjob.restartPolicy`                                 | restartPolicy to cronjob.                                                                                                                                                                                                                                                                                                                                                                                                             | `OnFailure`                                                         |
| `cronjob.containers.name`                                 | Name to containers cronjob.                                                                                                                                                                                                                                                                                                                                                                                                            | `1p-chart`         
| `nodeSelector`                        | Node labels for application pods assignment.                                                                        | `{}`                               |
| `tolerations`                         | Tolerations for application pods assignment.                                                                        | `[]`                                                          |
| `affinity`                       | Affinity for application pods assignment.                                                                           | `{}`                               |
| `applicationConfig`                   | Configmap for application pods assignment.                                                                        | `{}`
| `livenessProbe.enabled`                       | If true, custom the default livenessProbe.                                                                         | `false`                                                        |
| `livenessProbe.path`              | If livenessProbe enabled, this field set custom check path. | `/health`
| `livenessProbe.port`              | If livenessProbe enabled, this field set the port number (or name) that will be checked. | `http`
| `livenessProbe.scheme`              | If livenessProbe enabled, this field set the scheme that will be used. Options (`HTTP`,`TCP`) | `HTTP` | 
| `livenessProbe.initialDelaySeconds`           |If livenessProbe enabled, this field set delay before liveness probe is initiated.                                                                                                                                                                                                                                                                                                                                                                                                                                   | 60                                                            |
| `livenessProbe.periodSeconds`                 |If livenessProbe enabled, this field set how often to perform the probe.                                                                                                                                                                                                                                                                                                                                                                                                                                             | 10                                                            |
| `livenessProbe.timeoutSeconds`                |If livenessProbe enabled, this field set the probe times out.                                                                                                                                                                                                                                                                                                                                                                                                                                                   | 5                                                             |
| `livenessProbe.failureThreshold`              | If livenessProbe enabled, this field set minimum consecutive failures for the probe to be considered failed after having succeeded.                                                                                                                                                                                                                                                                                                                                                                                 | 6                                                             |
| `livenessProbe.successThreshold`              | If livenessProbe enabled, this field set minimum consecutive successes for the probe to be considered successful after having failed.                                                                                                                                                                                                                                                                                                                                                                               | 1                                                             |
| `readinessProbe.enabled`                      | if true, custom the default readinessProbe.                                                                                                                                                                                                                                                                                                                                                                                                                                                     | `false`                                                        |
| `readinessProbe.path`                                 | If readinessProbe enabled, this field set custom check path.                                                                                                                                                                                                                                         | `/health`.                                                                                                         
| `readinessProbe.port`              | If readinessProbe enabled, this field set the port number (or name) that will be checked. | `http`
| `readinessProbe.scheme`              | If readinessProbe enabled, this field set the scheme that will be used. Options (`HTTP`,`TCP`) | `HTTP` | 
| `readinessProbe.initialDelaySeconds`          |If readinessProbe enabled, this field set delay before readiness probe is initiated.                                                                                                                                                                                                                                                                                                                                                                                                                                  | 60                                                             |
| `readinessProbe.periodSeconds`                | If readinessProbe enabled, this field set how often to perform the probe.                                                                                                                                                                                                                                                                                                                                                                                                                                             | 10                                                            |
| `readinessProbe.timeoutSeconds`               | If readinessProbe enabled, this field set when the probe times out.                                                                                                                                                                                                                                                                                                                                                                                                                                                   | 5                                                             |
| `readinessProbe.failureThreshold`             |If readinessProbe enabled, this field set minimum consecutive failures for the probe to be considered failed after having succeeded.                                                                                                                                                                                                                                                                                                                                                                                 | 6                                                             |
| `readinessProbe.successThreshold`             |If readinessProbe enabled, this field set minimum consecutive successes for the probe to be considered successful after having failed.                                                                                                                                                                                                                                                                                                                                                                               | 1                                                             |



     
                                                                                                                               

Specify each parameter using the `--set key=value[,key=value]` argument to `helm install`. For example,

```console
$ helm install my-release \
  --set resources.limits.cpu=100m ,resources.limits.memory=256Mi \
    1p-chart/1p-chart
```