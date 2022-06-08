# promtail

![Version: 4.2.0-bb.1](https://img.shields.io/badge/Version-4.2.0--bb.1-informational?style=flat-square) ![Type: application](https://img.shields.io/badge/Type-application-informational?style=flat-square) ![AppVersion: v2.5.0](https://img.shields.io/badge/AppVersion-v2.5.0-informational?style=flat-square)

Promtail is an agent which ships the contents of local logs to a Loki instance

## Upstream References
* <https://grafana.com/loki>

* <https://github.com/grafana/loki>
* <https://grafana.com/oss/loki/>
* <https://grafana.com/docs/loki/latest/>

## Learn More
* [Application Overview](docs/overview.md)
* [Other Documentation](docs/)

## Pre-Requisites

* Kubernetes Cluster deployed
* Kubernetes config installed in `~/.kube/config`
* Helm installed

Install Helm

https://helm.sh/docs/intro/install/

## Deployment

* Clone down the repository
* cd into directory
```bash
helm install promtail chart/
```

## Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| nameOverride | string | `nil` | Overrides the chart's name |
| fullnameOverride | string | `nil` | Overrides the chart's computed fullname |
| initContainer.enabled | bool | `false` | Specifies whether the init container for setting inotify max user instances is to be enabled |
| initContainer.image.registry | string | `"docker.io"` | The Docker registry for the init container |
| initContainer.image.repository | string | `"busybox"` | Docker image repository for the init container |
| initContainer.image.tag | float | `1.33` | Docker tag for the init container |
| initContainer.image.pullPolicy | string | `"IfNotPresent"` | Docker image pull policy for the init container image |
| initContainer.fsInotifyMaxUserInstances | int | `128` | The inotify max user instances to configure |
| image.registry | string | `"registry1.dso.mil"` | The Docker registry |
| image.repository | string | `"ironbank/opensource/grafana/promtail"` | Docker image repository |
| image.tag | string | `"v2.5.0"` | Overrides the image tag whose default is the chart's appVersion |
| image.pullPolicy | string | `"IfNotPresent"` | Docker image pull policy |
| imagePullSecrets | list | `[{"name":"private-registry"}]` | Image pull secrets for Docker images |
| annotations | object | `{}` | Annotations for the DaemonSet |
| updateStrategy | object | `{}` | The update strategy for the DaemonSet |
| podLabels | object | `{}` | Pod labels |
| podAnnotations | object | `{}` | Pod annotations |
| priorityClassName | string | `nil` | The name of the PriorityClass |
| livenessProbe | object | `{}` | Liveness probe |
| readinessProbe | object | See `values.yaml` | Readiness probe |
| resources | object | `{"limits":{"cpu":"200m","memory":"128Mi"},"requests":{"cpu":"200m","memory":"128Mi"}}` | Resource requests and limits |
| podSecurityContext | object | `{"runAsGroup":0,"runAsUser":0}` | The security context for pods |
| containerSecurityContext | object | `{"allowPrivilegeEscalation":false,"capabilities":{"drop":["ALL"]},"privileged":false,"readOnlyRootFilesystem":true,"runAsUser":0,"seLinuxOptions":{"type":"spc_t"}}` | The security context for containers |
| rbac.create | bool | `true` | Specifies whether RBAC resources are to be created |
| rbac.pspEnabled | bool | `false` | Specifies whether a PodSecurityPolicy is to be created |
| serviceAccount.create | bool | `true` | Specifies whether a ServiceAccount should be created |
| serviceAccount.name | string | `nil` | The name of the ServiceAccount to use. If not set and `create` is true, a name is generated using the fullname template |
| serviceAccount.imagePullSecrets | list | `[]` | Image pull secrets for the service account |
| serviceAccount.annotations | object | `{}` | Annotations for the service account |
| nodeSelector | object | `{}` | Node selector for pods |
| affinity | object | `{}` | Affinity configuration for pods |
| tolerations | list | `[{"effect":"NoSchedule","key":"node-role.kubernetes.io/master","operator":"Exists"},{"effect":"NoSchedule","key":"node-role.kubernetes.io/control-plane","operator":"Exists"}]` | Tolerations for pods. By default, pods will be scheduled on master/control-plane nodes. |
| defaultVolumes | list | See `values.yaml` | Default volumes that are mounted into pods. In most cases, these should not be changed. Use `extraVolumes`/`extraVolumeMounts` for additional custom volumes. |
| defaultVolumeMounts | list | See `values.yaml` | Default volume mounts. Corresponds to `volumes`. |
| extraVolumes | list | `[]` |  |
| extraVolumeMounts | list | `[]` |  |
| extraArgs | list | `[]` |  |
| extraEnv | list | `[]` | Extra environment variables |
| extraEnvFrom | list | `[]` | Extra environment variables from secrets or configmaps |
| serviceMonitor.enabled | bool | `false` | If enabled, ServiceMonitor resources for Prometheus Operator are created |
| serviceMonitor.namespace | string | `nil` | Alternative namespace for ServiceMonitor resources |
| serviceMonitor.namespaceSelector | object | `{}` | Namespace selector for ServiceMonitor resources |
| serviceMonitor.annotations | object | `{}` | ServiceMonitor annotations |
| serviceMonitor.labels | object | `{}` | Additional ServiceMonitor labels |
| serviceMonitor.interval | string | `nil` | ServiceMonitor scrape interval |
| serviceMonitor.scrapeTimeout | string | `nil` | ServiceMonitor scrape timeout in Go duration format (e.g. 15s) |
| serviceMonitor.relabelings | list | `[]` | ServiceMonitor relabel configs to apply to samples before scraping https://github.com/prometheus-operator/prometheus-operator/blob/master/Documentation/api.md#relabelconfig |
| extraPorts | object | `{}` | Configure additional ports and services. For each configured port, a corresponding service is created. See values.yaml for details |
| podSecurityPolicy | object | See `values.yaml` | PodSecurityPolicy configuration. |
| config | object | See `values.yaml` | Section for crafting Promtails config file. The only directly relevant value is `config.file` which is a templated string that references the other values and snippets below this key. |
| config.logLevel | string | `"info"` | The log level of the Promtail server Must be reference in `config.file` to configure `server.log_level` See default config in `values.yaml` |
| config.serverPort | int | `3101` | The port of the Promtail server Must be reference in `config.file` to configure `server.http_listen_port` See default config in `values.yaml` |
| config.lokiAddress | string | `"http://loki-gateway/loki/api/v1/push"` | The Loki address to post logs to. Must be reference in `config.file` to configure `client.url`. See default config in `values.yaml` |
| config.snippets | object | See `values.yaml` | A section of reusable snippets that can be reference in `config.file`. Custom snippets may be added in order to reduce redundancy. This is especially helpful when multiple `kubernetes_sd_configs` are use which usually have large parts in common. |
| config.snippets.extraClientConfigs | list | empty | You can put here any keys that will be directly added to the config file's 'client' block. |
| config.snippets.extraScrapeConfigs | string | empty | You can put here any additional scrape configs you want to add to the config file. |
| config.snippets.extraRelabelConfigs | list | `[]` | You can put here any additional relabel_configs to "kubernetes-pods" job |
| config.file | string | See `values.yaml` | Config file contents for Promtail. Must be configured as string. It is templated so it can be assembled from reusable snippets in order to avoid redundancy. |
| networkPolicy.enabled | bool | `false` | Specifies whether Network Policies should be created |
| networkPolicy.metrics.podSelector | object | `{}` | Specifies the Pods which are allowed to access the metrics port. As this is cross-namespace communication, you also neeed the namespaceSelector. |
| networkPolicy.metrics.namespaceSelector | object | `{}` | Specifies the namespaces which are allowed to access the metrics port |
| networkPolicy.metrics.cidrs | list | `[]` | Specifies specific network CIDRs which are allowed to access the metrics port. In case you use namespaceSelector, you also have to specify your kubelet networks here. The metrics ports are also used for probes. |
| networkPolicy.k8sApi.port | int | `8443` | Specify the k8s API endpoint port |
| networkPolicy.k8sApi.cidrs | list | `[]` | Specifies specific network CIDRs you want to limit access to |
| istio.enabled | bool | `false` |  |
| istio.mtls.mode | string | `"STRICT"` |  |
| extraObjects | list | `[]` | Extra K8s manifests to deploy |

## Contributing

Please see the [contributing guide](./CONTRIBUTING.md) if you are interested in contributing.
