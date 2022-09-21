# How to upgrade the Promtail Package chart

This chart is reconciled against the [upstream chart](https://github.com/grafana/helm-charts/tree/main/charts/promtail) as declared in the [Kptfile](../chart/Kptfile).

When an upgrade is required, `kpt` can be ran to pull the updates with a targeted tag.

(from the repository root)
`kpt pkg update chart@<new tag> --strategy alpha-git-patch`

Once completed, you will need to reconcile the modifications that Big Bang makes back into the orchestration.

# Modifications Made to the upstream chart

## /chart/Chart.yaml
- Added `bigbang.dev/applicationVersions` annotation with the promtail version
- Modified Version to include `-bb.x` suffix

## /chart/bigbang/networkpolicies/*
- Network Policies added to establish allowed communication in/out of namespace

## chart/values.yaml
- Add common values for Big Bang packages for domain, networkpolicies and Istio
- Update image registry/repository/tag as required by update
- Add image pull secret for `private-registry`
- Set resource requests/limits
- Append to containerSecurityContext
```
  runAsUser: 0
  privileged: false
  seLinuxOptions:
    type: spc_t
```
## chart/Kptfile
- Tracks current upstream chart

# Testing a new promtail version

Current testing is done manually. Deployment of Big Bang with Istio, Monitoring, Loki and Promtail enabled. Point the promtail git values at the feature branch. Deploy and ensure all resources reconcile and are healthy.

Once healthy - navigate in browser to `grafana.bigbang.dev` (or other specified domain) and navigate `Configuration -> Data sources -> Loki` and click on `Save & test` and ensure `Data source connected and labels found`.