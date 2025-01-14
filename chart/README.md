# promtail

![Version: 6.16.6](https://img.shields.io/badge/Version-6.16.6-informational?style=flat-square) ![Type: application](https://img.shields.io/badge/Type-application-informational?style=flat-square) ![AppVersion: 3.0.0](https://img.shields.io/badge/AppVersion-3.0.0-informational?style=flat-square)

Promtail is an agent which ships the contents of local logs to a Loki instance

## Source Code

* <https://github.com/grafana/loki>
* <https://grafana.com/oss/loki/>
* <https://grafana.com/docs/loki/latest/>

## Chart Repo

Add the following repo to use the chart:

```console
helm repo add grafana https://grafana.github.io/helm-charts
```

## Upgrading

A major chart version change indicates that there is an incompatible breaking change needing manual actions.

### From Chart Versions >= 3.0.0

* Customizeable initContainer added.

### From Chart Versions < 3.0.0

#### Notable Changes

* Helm 3 is required
* Labels have been updated to follow the official Kubernetes [label recommendations](https://kubernetes.io/docs/concepts/overview/working-with-objects/common-labels/)
* The default scrape configs have been updated to take new and old labels into consideration
* The config file must be specified as string which can be templated.
  See below for details
* The config file is now stored in a Secret and no longer in a ConfigMap because it may contain sensitive data, such as basic auth credentials

Due to the label changes, an existing installation cannot be upgraded without manual interaction.
There are basically two options:

##### Option 1

Uninstall the old release and re-install the new one.
There will be no data loss.
Promtail will cleanly shut down and write the `positions.yaml`.
The new release which will pick up again from the existing `positions.yaml`.

##### Option 2

* Add new selector labels to the existing pods:

  ```
  kubectl label pods -n <namespace> -l app=promtail,release=<release> app.kubernetes.io/name=promtail app.kubernetes.io/instance=<release>
  ```

* Perform a non-cascading deletion of the DaemonSet which will keep the pods running:

  ```
  kubectl delete daemonset -n <namespace> -l app=promtail,release=<release> --cascade=false
  ```

* Perform a regular Helm upgrade on the existing release.
  The new DaemonSet will pick up the existing pods and perform a rolling upgrade.

## Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| nameOverride | string | `nil` | Overrides the chart's name |
| fullnameOverride | string | `nil` | Overrides the chart's computed fullname |
| vpa | object | `{"annotations":{},"controlledResources":[],"enabled":false,"kind":"DaemonSet","maxAllowed":{},"minAllowed":{},"updatePolicy":{"updateMode":"Auto"}}` | config for VerticalPodAutoscaler |
| daemonset.enabled | bool | `true` | Deploys Promtail as a DaemonSet |
| daemonset.autoscaling.enabled | bool | `false` | Creates a VerticalPodAutoscaler for the daemonset |
| affinity | object | `{}` | Affinity configuration for pods |
| annotations | object | `{}` | Annotations for the DaemonSet |
| automountServiceAccountToken | bool | `true` | Automatically mount API credentials for a particular Pod |
| config | object | See `values.yaml` | Section for crafting Promtails config file. The only directly relevant value is `config.file` which is a templated string that references the other values and snippets below this key. |
| config.clients | list | See `values.yaml` | The config of clients of the Promtail server Must be reference in `config.file` to configure `clients` |
| config.enableTracing | bool | `false` | The config to enable tracing |
| config.enabled | bool | `true` | Enable Promtail config from Helm chart Set `configmap.enabled: true` and this to `false` to manage your own Promtail config See default config in `values.yaml` |
| config.file | string | See `values.yaml` | Config file contents for Promtail. Must be configured as string. It is templated so it can be assembled from reusable snippets in order to avoid redundancy. |
| config.logFormat | string | `"logfmt"` | The log format of the Promtail server Must be reference in `config.file` to configure `server.log_format` Valid formats: `logfmt, json` See default config in `values.yaml` |
| config.logLevel | string | `"info"` | The log level of the Promtail server Must be reference in `config.file` to configure `server.log_level` See default config in `values.yaml` |
| config.positions | object | `{"filename":"/run/promtail/positions.yaml"}` | Configures where Promtail will save it's positions file, to resume reading after restarts. Must be referenced in `config.file` to configure `positions` |
| config.serverPort | int | `3101` | The port of the Promtail server Must be reference in `config.file` to configure `server.http_listen_port` See default config in `values.yaml` |
| config.snippets | object | See `values.yaml` | A section of reusable snippets that can be reference in `config.file`. Custom snippets may be added in order to reduce redundancy. This is especially helpful when multiple `kubernetes_sd_configs` are use which usually have large parts in common. |
| config.snippets.extraLimitsConfig | string | empty | You can put here any keys that will be directly added to the config file's 'limits_config' block. |
| config.snippets.extraRelabelConfigs | list | `[]` | You can put here any additional relabel_configs to "kubernetes-pods" job |
| config.snippets.extraScrapeConfigs | string | empty | You can put here any additional scrape configs you want to add to the config file. |
| config.snippets.extraServerConfigs | string | empty | You can put here any keys that will be directly added to the config file's 'server' block. |
| configmap.enabled | bool | `false` | If enabled, promtail config will be created as a ConfigMap instead of a secret |
| containerSecurityContext | object | `{"allowPrivilegeEscalation":false,"capabilities":{"drop":["ALL"]},"readOnlyRootFilesystem":true}` | The security context for containers |
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
| secret.labels | object | `{}` | Labels for the Secret |
| secret.annotations | object | `{}` | Annotations for the Secret |
| configmap.enabled | bool | `false` | If enabled, promtail config will be created as a ConfigMap instead of a secret |
| initContainer | list | `[]` |  |
| image.registry | string | `"registry1.dso.mil"` | The Docker registry |
| image.repository | string | `"ironbank/opensource/grafana/promtail"` | Docker image repository |
| image.tag | string | `"v2.9.4"` | Overrides the image tag whose default is the chart's appVersion |
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
| readinessProbe | object | See `values.yaml` | Readiness probe |
| resources | object | `{}` | Resource requests and limits |
| secret.annotations | object | `{}` | Annotations for the Secret |
| secret.labels | object | `{}` | Labels for the Secret |
| service.annotations | object | `{}` | Annotations for the service |
| service.enabled | bool | `false` |  |
| service.labels | object | `{}` | Labels for the service |
| serviceAccount.annotations | object | `{}` | Annotations for the service account |
| serviceAccount.automountServiceAccountToken | bool | `true` | Automatically mount a ServiceAccount's API credentials |
| serviceAccount.create | bool | `true` | Specifies whether a ServiceAccount should be created |
| serviceAccount.name | string | `nil` | The name of the ServiceAccount to use. If not set and `create` is true, a name is generated using the fullname template |
| serviceAccount.imagePullSecrets | list | `[]` | Image pull secrets for the service account |
| serviceAccount.annotations | object | `{}` | Annotations for the service account |
| nodeSelector | object | `{}` | Node selector for pods |
| affinity | object | `{}` | Affinity configuration for pods |
| tolerations | list | `[{"effect":"NoSchedule","key":"node-role.kubernetes.io/master","operator":"Exists"},{"effect":"NoSchedule","key":"node-role.kubernetes.io/control-plane","operator":"Exists"}]` | Tolerations for pods. By default, pods will be scheduled on master/control-plane nodes. |
| defaultVolumes | list | See `values.yaml` | Default volumes that are mounted into pods. In most cases, these should not be changed. Use `extraVolumes`/`extraVolumeMounts` for additional custom volumes. |
| defaultVolumeMounts | list | See `values.yaml` | Default volume mounts. Corresponds to `volumes`. |
| extraVolumes[0].name | string | `"varlog"` |  |
| extraVolumes[0].hostPath.path | string | `"/var/log"` |  |
| extraVolumes[1].name | string | `"machine-id"` |  |
| extraVolumes[1].hostPath.path | string | `"/etc/machine-id"` |  |
| extraVolumeMounts[0].name | string | `"varlog"` |  |
| extraVolumeMounts[0].mountPath | string | `"/var/log"` |  |
| extraVolumeMounts[0].readOnly | bool | `true` |  |
| extraVolumeMounts[1].name | string | `"machine-id"` |  |
| extraVolumeMounts[1].mountPath | string | `"/etc/machine-id"` |  |
| extraVolumeMounts[1].readOnly | bool | `true` |  |
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
| sidecar.configReloader.image.tag | string | `"v0.12.0"` | Docker image tag for sidecar config-reloader |
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
| istio.hardened.customAuthorizationPolicies | list | `[]` |  |
| istio.hardened.prometheus.enabled | bool | `true` |  |
| istio.hardened.prometheus.namespaces[0] | string | `"monitoring"` |  |
| istio.hardened.prometheus.principals[0] | string | `"cluster.local/ns/monitoring/sa/monitoring-grafana"` |  |
| istio.hardened.prometheus.principals[1] | string | `"cluster.local/ns/monitoring/sa/monitoring-monitoring-kube-alertmanager"` |  |
| istio.hardened.prometheus.principals[2] | string | `"cluster.local/ns/monitoring/sa/monitoring-monitoring-kube-operator"` |  |
| istio.hardened.prometheus.principals[3] | string | `"cluster.local/ns/monitoring/sa/monitoring-monitoring-kube-prometheus"` |  |
| istio.hardened.prometheus.principals[4] | string | `"cluster.local/ns/monitoring/sa/monitoring-monitoring-kube-state-metrics"` |  |
| istio.hardened.prometheus.principals[5] | string | `"cluster.local/ns/monitoring/sa/monitoring-monitoring-prometheus-node-exporter"` |  |
| istio.mtls.mode | string | `"STRICT"` | STRICT = Allow only mutual TLS traffic PERMISSIVE = Allow both plain text and mutual TLS traffic |
| networkPolicies.enabled | bool | `false` | Toggle networkPolicies |
| networkPolicies.controlPlaneCidr | string | `"0.0.0.0/0"` | Control Plane CIDR, defaults to 0.0.0.0/0, use `kubectl get endpoints -n default kubernetes` to get the CIDR range needed for your cluster Must be an IP CIDR range (x.x.x.x/x - ideally with /32 for the specific IP of a single endpoint, broader range for multiple masters/endpoints) Used by package NetworkPolicies to allow Kube API access |
| openshift | bool | `false` | Toggle or openshift specific config |
| loki | object | `{"enabled":false}` | Toggle Loki network policy enabling |

## Configuration

The config file for Promtail must be configured as string.
This is necessary because the contents are passed through the `tpl` function.
With this, the file can be templated and assembled from reusable YAML snippets.
It is common to have multiple `kubernetes_sd_configs` that, in turn, usually need the same `pipeline_stages`.
Thus, extracting reusable snippets helps reduce redundancy and avoid copy/paste errors.
See `values.yaml´ for details.
Also, the following examples make use of this feature.

For additional reference, please refer to Promtail's docs:

https://grafana.com/docs/loki/latest/clients/promtail/configuration/

### Syslog Support

```yaml
extraPorts:
  syslog:
    name: tcp-syslog
    containerPort: 1514
    service:
      port: 80
      type: LoadBalancer
      externalTrafficPolicy: Local
      loadBalancerIP: 123.234.123.234

config:
  snippets:
    extraScrapeConfigs: |
      # Add an additional scrape config for syslog
      - job_name: syslog
        syslog:
          listen_address: 0.0.0.0:{{ .Values.extraPorts.syslog.containerPort }}
          labels:
            job: syslog
        relabel_configs:
          - source_labels:
              - __syslog_message_hostname
            target_label: hostname

          # example label values: kernel, CRON, kubelet
          - source_labels:
              - __syslog_message_app_name
            target_label: app

          # example label values: debug, notice, informational, warning, error
          - source_labels:
              - __syslog_message_severity
            target_label: level
```

Find additional source labels in the Promtail's docs:

https://grafana.com/docs/loki/latest/clients/promtail/configuration/#syslog

### Journald Support

```yaml
config:
  snippets:
    extraScrapeConfigs: |
      # Add an additional scrape config for syslog
      - job_name: journal
        journal:
          path: /var/log/journal
          max_age: 12h
          labels:
            job: systemd-journal
        relabel_configs:
          - source_labels:
              - __journal__hostname
            target_label: hostname

          # example label values: kubelet.service, containerd.service
          - source_labels:
              - __journal__systemd_unit
            target_label: unit

          # example label values: debug, notice, info, warning, error
          - source_labels:
              - __journal_priority_keyword
            target_label: level

# Mount journal directory and machine-id file into promtail pods
extraVolumes:
  - name: journal
    hostPath:
      path: /var/log/journal
  - name: machine-id
    hostPath:
      path: /etc/machine-id

extraVolumeMounts:
  - name: journal
    mountPath: /var/log/journal
    readOnly: true
  - name: machine-id
    mountPath: /etc/machine-id
    readOnly: true
```

Find additional configuration options in Promtail's docs:

https://grafana.com/docs/loki/latest/clients/promtail/configuration/#journal

More journal source labels can be found here https://www.freedesktop.org/software/systemd/man/systemd.journal-fields.html.
> Note that each message from the journal may have a different set of fields and software may write an arbitrary set of custom fields for their logged messages. [(related issue)](https://github.com/grafana/loki/issues/2048#issuecomment-626234611)

The machine-id needs to be available in the container as it is required for scraping.
This is described in Promtail's scraping docs:

https://grafana.com/docs/loki/latest/clients/promtail/scraping/#journal-scraping-linux-only

### Push API Support

```yaml
extraPorts:
  httpPush:
    name: http-push
    containerPort: 3500
  grpcPush:
    name: grpc-push
    containerPort: 3600

config:
  file: |
    server:
      log_level: {{ .Values.config.logLevel }}
      http_listen_port: {{ .Values.config.serverPort }}

    clients:
      - url: {{ .Values.config.lokiAddress }}

    positions:
      filename: /run/promtail/positions.yaml

    scrape_configs:
      {{- tpl .Values.config.snippets.scrapeConfigs . | nindent 2 }}

      - job_name: push1
        loki_push_api:
          server:
            http_listen_port: {{ .Values.extraPorts.httpPush.containerPort }}
            grpc_listen_port: {{ .Values.extraPorts.grpcPush.containerPort }}
          labels:
            pushserver: push1
```

### Customize client config options

By default, promtail send logs scraped to `loki` server at `http://loki-gateway/loki/api/v1/push`.
If you want to customize clients or add additional options to `loki`, please use the `clients` section. For example, to enable HTTP basic auth and include OrgID header, you can use:

```yaml
config:
  clients:
    - url: http://loki.server/loki/api/v1/push
      tenant_id: 1
      basic_auth:
        username: loki
        password: secret
```
