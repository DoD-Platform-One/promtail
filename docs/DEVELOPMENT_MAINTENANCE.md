## Steps for KPT update
1. Navigate to the upstream chart repo and folder [here](https://github.com/grafana/helm-charts/tree/main/charts/promtail) and find the tag that corresponds with the new chart version for this update
2. Checkout the `renovate/ironbank` branch
3. From the root of the repo run `kpt pkg update chart@<tag> --strategy alpha-git-patch`, where tag is found in step 1, checkout the `chart/Kptfile` ref for tag naming
4. Modify the version in `Chart.yaml` and append `-bb.0` to the chart version from upstream. Update dependencies to latest BB gluon library version using: `helm dependency update ./chart`
5. Update `CHANGELOG.md` adding an entry for the new version and noting all changes (at minimum should include `Updated <chart or dependency> to x.x.x`).
6. Generate the `README.md` updates by following the [guide in gluon](https://repo1.dso.mil/big-bang/product/packages/gluon/-/blob/master/docs/bb-package-readme.md).
7. Push up your changes, add upgrade notices if applicable, validate that CI passes. If there are any failures, follow the information in the pipeline to make the necessary updates. Add the `debug` label to the MR for more detailed information. Reach out to the CODEOWNERS if needed.
8. As part of your MR that modifies bigbang packages, you should modify the bigbang  [bigbang/tests/test-values.yaml](https://repo1.dso.mil/big-bang/bigbang/-/blob/master/tests/test-values.yaml?ref_type=heads) against your branch for the CI/CD MR testing by enabling your packages. 
- To do this, at a minimum, you will need to follow the instructions at [bigbang/docs/developer/test-package-against-bb.md](https://repo1.dso.mil/big-bang/bigbang/-/blob/master/docs/developer/test-package-against-bb.md?ref_type=heads) with changes for Promtail enabled (the below is a reference, actual changes could be more depending on what changes where made to Promtail in the package MR).

### [test-values.yaml](https://repo1.dso.mil/big-bang/bigbang/-/blob/master/tests/test-values.yaml?ref_type=heads)
```yaml
promtail:
  enabled: true
  git:
    tag: null
    branch: "renovate/ironbank"
  values:
    istio:
      hardened:
        enabled: true
  ### Additional components of Promtail should be changed to reflect testing changes introduced in the package MR
```

9. Perform the steps below for manual testing

## Modifications made to upstream
> List of changes per file to be aware of for how Big Bang differs from upstream

### /chart/Chart.yaml
- Added `bigbang.dev/applicationVersions` annotation with the promtail version
- Modified Version to include `-bb.x` suffix

### /chart/ci/netpol-values.yaml
- Network Policies added to establish allowed communication in/out of namespace
- Extra ports added
  - tcp-syslog 1514
  - http-push 3500
  - grpc-push 3600

### chart/values.yaml
- Add common values for Big Bang packages for domain, networkpolicies and Istio
- Update image registry/repository/tag as required by update
- Add image pull secret for `private-registry`
- Set resource requests/limits
- Be wary of changes to `relabel_configs:` section, these may cause `field action already set` errors
- Append to containerSecurityContext
```yaml
  runAsUser: 0
  privileged: false
  seLinuxOptions:
    type: spc_t
```
### chart/Kptfile
- Tracks current upstream chart

## Manual Testing
> NOTE: For these testing steps it is good to do them on both a clean install and an upgrade. For clean install, point `promtail` to your branch. For an upgrade do an install with `promtail` pointing to the latest tag, then perform a helm upgrade with `promtail` pointing to your branch.
- You will want to install with Istio, Monitoring, Loki and Promtail enabled.

`overrides/promtail.yaml`
```yaml
monitoring:
  enabled: true
alloy:
  enabled: false
bbctl:
  enabled: false
promtail:
  enabled: true
  git:
    tag: null
    branch: "renovate/ironbank"
  values:
    istio:
      enabled: true
      hardened:
        enabled: true
        # enabled: false
      prometheus:
        enabled: true
        # namespaces: [] # this will break prometheus scraping
    serviceMonitor:
      enabled: true
loki:
  enabled: true
  strategy: scalable
  values:
    minio:
      enabled: true
grafana:
  enabled: true

addons:
  minioOperator:
    enabled: true
```

Current testing is done manually. Deployment of Big Bang with Istio, Monitoring, Loki and Promtail enabled. Point the promtail git values at the feature branch. Deploy and ensure all resources reconcile and are healthy.

Once healthy:
- Go to [https://grafana.dev.bigbang.mil](https://grafana.dev.bigbang.mil) in your browser and login with [default credentials](https://repo1.dso.mil/big-bang/bigbang/-/blob/master/docs/guides/using-bigbang/default-credentials.md)
- Navigate to `Connections -> Data sources -> Loki`
  - Click on `Save & test` and ensure `Data source successfully connected.` message appears
- Search dashboards for `Loki Dashboard Quick Search` and confirm log data is being populated/no error messages

> When in doubt with any testing or upgrade steps, reach out to the CODEOWNERS for assistance.
