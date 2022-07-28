# Promtail

## Overview

This package contains an extensible and configurable installation of Grafana Promtail based on the upstream chart provided by grafana.

## Loki

[Promtail](https://grafana.com/docs/loki/latest/clients/promtail/) is an agent which ships the contents of local logs to a private Loki instance or Grafana Cloud. It is usually deployed to every machine that has applications needed to be monitored. Currently, Promtail can tail logs from two sources: local log files and the systemd journal (on AMD64 machines only). Promtail borrows the same service discovery mechanism from Prometheus, although it currently only supports static and kubernetes service discovery. This limitation is due to the fact that Promtail is deployed as a daemon to every local machine and, as such, does not discover label from other machines. kubernetes service discovery fetches required labels from the Kubernetes API server while static usually covers all other use cases.

## How it works

Just like Prometheus, promtail is configured using a scrape_configs stanza. relabel_configs allows for fine-grained control of what to ingest, what to drop, and the final metadata to attach to the log line. Refer to the docs for configuring Promtail for more details. Once Promtail has a set of targets (i.e., things to read from, like files) and all labels are set correctly, it will start tailing (continuously reading) the logs from targets. Once enough data is read into memory or after a configurable timeout, it is flushed as a single batch to Loki.

As Promtail reads data from sources (files and systemd journal, if configured), it will track the last offset it read in a positions file. By default, the positions file is stored at /var/log/positions.yaml. The positions file helps Promtail continue reading from where it left off in the case of the Promtail instance restarting.

## Extra Client configurations
See [Chart README](../chart/README.md#customize-client-config-options) for options for adding/customizing clients.