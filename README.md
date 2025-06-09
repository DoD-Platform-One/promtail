<!-- Warning: Do not manually edit this file. See notes on gluon + helm-docs at the end of this file for more information. -->
# promtail

![Version: 6.16.6-bb.5](https://img.shields.io/badge/Version-6.16.6--bb.5-informational?style=flat-square) ![Type: application](https://img.shields.io/badge/Type-application-informational?style=flat-square) ![AppVersion: 3.5.1](https://img.shields.io/badge/AppVersion-3.5.1-informational?style=flat-square) ![Maintenance Track: bb_integrated](https://img.shields.io/badge/Maintenance_Track-bb_integrated-green?style=flat-square)

Promtail is an agent which ships the contents of local logs to a Loki instance

## Upstream References

- <https://grafana.com/loki>
- <https://github.com/grafana/loki>
- <https://grafana.com/oss/loki/>
- <https://grafana.com/docs/loki/latest/>

## Upstream Release Notes

- [Find upstream chart's release notes and CHANGELOG here](https://github.com/grafana/helm-charts/releases?q=promtail&expanded=true)
- [Find upstream application's release notes and CHANGELOG here](https://grafana.com/docs/loki/latest/release-notes/)

## Learn More

- [Application Overview](docs/overview.md)
- [Other Documentation](docs/)

## Pre-Requisites

- Kubernetes Cluster deployed
- Kubernetes config installed in `~/.kube/config`
- Helm installed

Install Helm

https://helm.sh/docs/intro/install/

## Deployment

- Clone down the repository
- cd into directory

```bash
helm install promtail chart/
```

## Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| nameOverride | string | `nil` | Overrides the chart's name |
| fullnameOverride | string | `nil` | Overrides the chart's computed fullname |
| vpa | object | `{"annotations":{},"controlledResources":[],"enabled":false,"kind":"DaemonSet","maxAllowed":{},"minAllowed":{},"updatePolicy":{"updateMode":"Auto"}}` | config for VerticalPodAutoscaler |
| daemonset.enabled | bool | `true` | Deploys Promtail as a DaemonSet |
| daemonset.autoscaling.enabled | bool | `false` | Creates a VerticalPodAutoscaler for the daemonset |
| daemonset.autoscaling.controlledResources | list | `[]` | List of resources that the vertical pod autoscaler can control. Defaults to cpu and memory |
| daemonset.autoscaling.maxAllowed | object | `{}` | Defines the max allowed resources for the pod |
| daemonset.autoscaling.minAllowed | object | `{}` | Defines the min allowed resources for the pod |
| deployment.enabled | bool | `false` | Deploys Promtail as a Deployment |
| deployment.replicaCount | int | `1` |  |
| deployment.autoscaling.enabled | bool | `false` | Creates a HorizontalPodAutoscaler for the deployment |
| deployment.autoscaling.minReplicas | int | `1` |  |
| deployment.autoscaling.maxReplicas | int | `10` |  |
| deployment.autoscaling.targetCPUUtilizationPercentage | int | `80` |  |
| deployment.autoscaling.targetMemoryUtilizationPercentage | string | `nil` |  |
| deployment.strategy | object | `{"type":"RollingUpdate"}` | Set deployment object update strategy |
| service.enabled | bool | `false` |  |
| service.labels | object | `{}` | Labels for the service |
| service.annotations | object | `{}` | Annotations for the service |
| secret.labels | object | `{}` | Labels for the Secret |
| secret.annotations | object | `{}` | Annotations for the Secret |
| configmap.enabled | bool | `false` | If enabled, promtail config will be created as a ConfigMap instead of a secret |
| initContainer | list | `[]` |  |
| image.registry | string | `"registry1.dso.mil"` | The Docker registry |
| image.repository | string | `"ironbank/opensource/grafana/promtail"` | Docker image repository |
| image.tag | string | `"v3.5.1"` | Overrides the image tag whose default is the chart's appVersion |
| image.pullPolicy | string | `"IfNotPresent"` | Docker image pull policy |
| imagePullSecrets | list | `[{"name":"private-registry"}]` | Image pull secrets for Docker images |
| hostAliases | list | `[]` | hostAliases to add |
| hostNetwork | string | `nil` | Controls whether the pod has the `hostNetwork` flag set. |
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
| namespace | string | `nil` | The name of the Namespace to deploy If not set, `.Release.Namespace` is used |
| serviceAccount.create | bool | `true` | Specifies whether a ServiceAccount should be created |
| serviceAccount.name | string | `nil` | The name of the ServiceAccount to use. If not set and `create` is true, a name is generated using the fullname template |
| serviceAccount.imagePullSecrets | list | `[]` | Image pull secrets for the service account |
| serviceAccount.annotations | object | `{}` | Annotations for the service account |
| serviceAccount.automountServiceAccountToken | bool | `true` | Automatically mount a ServiceAccount's API credentials |
| automountServiceAccountToken | bool | `true` | Automatically mount API credentials for a particular Pod |
| nodeSelector | object | `{}` | Node selector for pods |
| affinity | object | `{}` | Affinity configuration for pods |
| tolerations | list | `[{"effect":"NoSchedule","key":"node-role.kubernetes.io/master","operator":"Exists"},{"effect":"NoSchedule","key":"node-role.kubernetes.io/control-plane","operator":"Exists"}]` | Tolerations for pods. By default, pods will be scheduled on master/control-plane nodes. |
| defaultVolumes | list | See `values.yaml` | Default volumes that are mounted into pods. In most cases, these should not be changed. Use `extraVolumes`/`extraVolumeMounts` for additional custom volumes. |
| defaultVolumeMounts | list | See `values.yaml` | Default volume mounts. Corresponds to `volumes`. |
| extraVolumes[0].name | string | `"varlog"` |  |
| extraVolumes[0].hostPath.path | string | `"/var/log"` |  |
| extraVolumeMounts[0].name | string | `"varlog"` |  |
| extraVolumeMounts[0].mountPath | string | `"/var/log"` |  |
| extraVolumeMounts[0].readOnly | bool | `true` |  |
| extraArgs | list | `["-config.expand-env=true"]` | - -client.external-labels=hostname=$(HOSTNAME) |
| extraEnv | list | `[{"name":"NODE_HOSTNAME","valueFrom":{"fieldRef":{"fieldPath":"spec.nodeName"}}}]` | Extra environment variables |
| extraEnvFrom | list | `[]` | Extra environment variables from secrets or configmaps |
| enableServiceLinks | bool | `true` | Configure enableServiceLinks in pod |
| serviceMonitor.enabled | bool | `false` | If enabled, ServiceMonitor resources for Prometheus Operator are created |
| serviceMonitor.namespace | string | `nil` | Alternative namespace for ServiceMonitor resources |
| serviceMonitor.namespaceSelector | object | `{}` | Namespace selector for ServiceMonitor resources |
| serviceMonitor.annotations | object | `{}` | ServiceMonitor annotations |
| serviceMonitor.labels | object | `{}` | Additional ServiceMonitor labels |
| serviceMonitor.interval | string | `nil` | ServiceMonitor scrape interval |
| serviceMonitor.scrapeTimeout | string | `nil` | ServiceMonitor scrape timeout in Go duration format (e.g. 15s) |
| serviceMonitor.relabelings | list | `[]` | ServiceMonitor relabel configs to apply to samples before scraping https://github.com/prometheus-operator/prometheus-operator/blob/master/Documentation/api.md#relabelconfig (defines `relabel_configs`) |
| serviceMonitor.metricRelabelings | list | `[]` | ServiceMonitor relabel configs to apply to samples as the last step before ingestion https://github.com/prometheus-operator/prometheus-operator/blob/master/Documentation/api.md#relabelconfig (defines `metric_relabel_configs`) |
| serviceMonitor.targetLabels | list | `[]` | ServiceMonitor will add labels from the service to the Prometheus metric https://github.com/prometheus-operator/prometheus-operator/blob/main/Documentation/api.md#servicemonitorspec |
| serviceMonitor.scheme | string | `"http"` | ServiceMonitor will use http by default, but you can pick https as well |
| serviceMonitor.tlsConfig | string | `nil` | ServiceMonitor will use these tlsConfig settings to make the health check requests |
| serviceMonitor.prometheusRule | object | `{"additionalLabels":{},"enabled":false,"rules":[]}` | Prometheus rules will be deployed for alerting purposes |
| extraContainers | object | `{}` |  |
| extraPorts | object | `{}` | Configure additional ports and services. For each configured port, a corresponding service is created. See values.yaml for details |
| podSecurityPolicy | object | See `values.yaml` | PodSecurityPolicy configuration. |
| config | object | See `values.yaml` | Section for crafting Promtails config file. The only directly relevant value is `config.file` which is a templated string that references the other values and snippets below this key. |
| config.enabled | bool | `true` | Enable Promtail config from Helm chart Set `configmap.enabled: true` and this to `false` to manage your own Promtail config See default config in `values.yaml` |
| config.logLevel | string | `"info"` | The log level of the Promtail server Must be reference in `config.file` to configure `server.log_level` See default config in `values.yaml` |
| config.logFormat | string | `"logfmt"` | The log format of the Promtail server Must be reference in `config.file` to configure `server.log_format` Valid formats: `logfmt, json` See default config in `values.yaml` |
| config.serverPort | int | `3101` | The port of the Promtail server Must be reference in `config.file` to configure `server.http_listen_port` See default config in `values.yaml` |
| config.clients | list | See `values.yaml` | The config of clients of the Promtail server Must be reference in `config.file` to configure `clients` |
| config.positions | object | `{"filename":"/run/promtail/positions.yaml"}` | Configures where Promtail will save it's positions file, to resume reading after restarts. Must be referenced in `config.file` to configure `positions` |
| config.enableTracing | bool | `false` | The config to enable tracing |
| config.snippets | object | See `values.yaml` | A section of reusable snippets that can be reference in `config.file`. Custom snippets may be added in order to reduce redundancy. This is especially helpful when multiple `kubernetes_sd_configs` are use which usually have large parts in common. |
| config.snippets.extraLimitsConfig | string | empty | You can put here any keys that will be directly added to the config file's 'limits_config' block. |
| config.snippets.extraServerConfigs | string | empty | You can put here any keys that will be directly added to the config file's 'server' block. |
| config.snippets.extraScrapeConfigs | string | empty | You can put here any additional scrape configs you want to add to the config file. |
| config.snippets.extraRelabelConfigs | list | `[]` | You can put here any additional relabel_configs to "kubernetes-pods" job |
| config.file | string | See `values.yaml` | Config file contents for Promtail. Must be configured as string. It is templated so it can be assembled from reusable snippets in order to avoid redundancy. |
| networkPolicy.enabled | bool | `false` | Specifies whether Network Policies should be created |
| networkPolicy.metrics.podSelector | object | `{}` | Specifies the Pods which are allowed to access the metrics port. As this is cross-namespace communication, you also neeed the namespaceSelector. |
| networkPolicy.metrics.namespaceSelector | object | `{}` | Specifies the namespaces which are allowed to access the metrics port |
| networkPolicy.metrics.cidrs | list | `[]` | Specifies specific network CIDRs which are allowed to access the metrics port. In case you use namespaceSelector, you also have to specify your kubelet networks here. The metrics ports are also used for probes. |
| networkPolicy.k8sApi.port | int | `8443` | Specify the k8s API endpoint port |
| networkPolicy.k8sApi.cidrs | list | `[]` | Specifies specific network CIDRs you want to limit access to |
| httpPathPrefix | string | `""` | Base path to server all API routes fro |
| sidecar.configReloader.enabled | bool | `false` |  |
| sidecar.configReloader.image.registry | string | `"registry1.dso.mil"` | The Docker registry for sidecar config-reloader |
| sidecar.configReloader.image.repository | string | `"ironbank/opensource/jimmidyson/configmap-reload"` | Docker image repository for sidecar config-reloader |
| sidecar.configReloader.image.tag | string | `"v0.15.0"` | Docker image tag for sidecar config-reloader |
| sidecar.configReloader.image.pullPolicy | string | `"IfNotPresent"` | Docker image pull policy for sidecar config-reloader |
| sidecar.configReloader.extraArgs | list | `[]` |  |
| sidecar.configReloader.extraEnv | list | `[]` | Extra environment variables for sidecar config-reloader |
| sidecar.configReloader.extraEnvFrom | list | `[]` | Extra environment variables from secrets or configmaps for sidecar config-reloader |
| sidecar.configReloader.containerSecurityContext | object | `{"allowPrivilegeEscalation":false,"capabilities":{"drop":["ALL"]},"readOnlyRootFilesystem":true}` | The security context for containers for sidecar config-reloader |
| sidecar.configReloader.readinessProbe | object | `{}` | Readiness probe for sidecar config-reloader |
| sidecar.configReloader.livenessProbe | object | `{}` | Liveness probe for sidecar config-reloader |
| sidecar.configReloader.resources | object | `{}` | Resource requests and limits for sidecar config-reloader |
| sidecar.configReloader.config.serverPort | int | `9533` | The port of the config-reloader server |
| sidecar.configReloader.serviceMonitor.enabled | bool | `true` |  |
| extraObjects | list | `[]` | Extra K8s manifests to deploy |
| istio.enabled | bool | `false` | Toggle interaction with Istio |
| istio.hardened.enabled | bool | `false` |  |
| istio.hardened.outboundTrafficPolicyMode | string | `"REGISTRY_ONLY"` |  |
| istio.hardened.customServiceEntries | list | `[]` |  |
| istio.hardened.customAuthorizationPolicies | list | `[]` |  |
| istio.mtls.mode | string | `"STRICT"` | STRICT = Allow only mutual TLS traffic PERMISSIVE = Allow both plain text and mutual TLS traffic |
| networkPolicies.enabled | bool | `false` | Toggle networkPolicies |
| networkPolicies.controlPlaneCidr | string | `"0.0.0.0/0"` | Control Plane CIDR, defaults to 0.0.0.0/0, use `kubectl get endpoints -n default kubernetes` to get the CIDR range needed for your cluster Must be an IP CIDR range (x.x.x.x/x - ideally with /32 for the specific IP of a single endpoint, broader range for multiple masters/endpoints) Used by package NetworkPolicies to allow Kube API access |
| networkPolicies.additionalPolicies | list | `[]` |  |
| openshift | bool | `false` | Toggle or openshift specific config |
| loki | object | `{"enabled":false}` | Toggle Loki network policy enabling |

## Contributing

Please see the [contributing guide](./CONTRIBUTING.md) if you are interested in contributing.

---

_This file is programatically generated using `helm-docs` and some BigBang-specific templates. The `gluon` repository has [instructions for regenerating package READMEs](https://repo1.dso.mil/big-bang/product/packages/gluon/-/blob/master/docs/bb-package-readme.md)._

