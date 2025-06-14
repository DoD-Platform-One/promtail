# Changelog

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/), and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

---

## [6.16.6-bb.5] 2025-05-29

### Updated

- Updated ironbank/opensource/grafana/promtail v3.5.0 -> v3.5.1

## [6.16.6-bb.4] 2025-05-02

### Updated

- Update promtail from `v3.4.3` -> `v3.5.0`
- Update configmap-reload from `v0.14.0` -> `v0.15.0`

## [6.16.6-bb.3] 2025-04-08

### Updated

- Update promtail from `v3.4.2` -> `v3.4.3`

## [6.16.6-bb.2] 2025-02-19

### Updated

- Update promtail from `v3.3.2` -> `v3.4.2`

## [6.16.6-bb.1] 2025-02-12

### Updated

- Adds support to package for Istio Operatorless Network Policy dynamic values.

## [6.16.6-bb.0] 2025-01-13

### Updated

- Update promtail from `v3.0.0` -> `v3.3.2`
- Update configmap-reload from `v0.13.1` -> `v0.14.0`

## [6.16.2-bb.4] - 2024-08-30

### Changed

- Updating Promtail `oscal-component.yaml` to include Lula validations for automated assessment
- Added the maintenance track annotation and badge

### Added

- Added `oscal-assessment-results.yaml` as a threshold for automated governance

## [6.16.2-bb.3] - 2024-08-20

### Changed

- Fixed BB docs

## [6.16.2-bb.2] - 2024-08-15

### Changed

- Run podLabels through tpl

## [6.16.2-bb.1] 2024-07-02

### Removed

- Removed shared authPolicies set at the Istio level

## [6.16.2-bb.0] 2024-07-02

### Updated

- Update promtail from `v2.9.4` -> `v3.0.0`
- Update configmap-reload from `v0.12.0` -> `v0.13.1`

## [6.15.5-bb.7] 2024-06-27

### Updated

- Set new default labels according to best practices

## [6.15.5-bb.6] 2024-06-26

### Added

- Drop unnecessary labels for Loki 3.0 support
- Fixed duplicate exportTo attribute

## [6.15.5-bb.5] 2024-05-01

### Added

- Drop unnecessary labels for Loki 3.0 support
- Fixed duplicate exportTo attribute

## [6.15.5-bb.4] 2024-04-19

### Added

- Added custom network policy

## [6.15.5-bb.3] 2024-03-08

### changed

- Adding Sidecar to deny egress that is external to istio services
- Adding customServiceEntries to allow egress to override sidecar

## [6.15.5-bb.2] 2024-03-08

### Updated

- Openshift update for deploying Promtail into Openshift cluster

## [6.15.5-bb.1] 2024-03-05

### Updated

- Moved machine-id volume mounts to default section to allow users to easily disable /var/log logging

## [6.15.5-bb.0] 2024-02-06

### Updated

- Updated ironbank/opensource/grafana/promtail v2.9.2 -> v2.9.4
- Updated registry1.dso.mil/ironbank/opensource/grafana/promtail v2.9.2 -> v2.9.4
- Updated chart version to 6.15.5

## [6.15.3-bb.4] 2024-01-17

### Changed

- removed Istio.enabled from test-values

## [6.15.3-bb.4] 2024-01-12

### Changed

- Istio.enabled as false in test-values

## [6.15.3-bb.3] 2024-01-12

### Changed

- Enabled istio hardening in tests

## [6.15.3-bb.2] 2023-11-28

### Added

- Updating OSCAL Component file.

## [6.15.3-bb.1] 2023-11-15

### Added

- Added istio `allow-nothing` policy
- Added istio `allow-prometheus` policy
- Added istio custom policy template

## [6.15.3-bb.0] 2023-10-23

### Updated

- Updated ironbank/opensource/grafana/promtail v2.9.1 -> v2.9.2
- Updated registry1.dso.mil/ironbank/opensource/grafana/promtail v2.9.1 -> v2.9.2
- Updated chart version to 6.15.3

## [6.15.0-bb.3] 2023-10-16

### Updated

- Updated registry1.dso.mil/ironbank/opensource/jimmidyson/configmap-reload v0.11.1 -> v0.12.0

## [6.15.0-bb.2] 2023-10-11

### Updated

- Update OSCAL version from 1.0.0 to 1.1.1

## [6.15.0-bb.1] 2023-09-27

### Updated

- Updated ironbank/opensource/grafana/promtail v2.8.4 -> v2.9.1
- Updated registry1.dso.mil/ironbank/opensource/grafana/promtail v2.8.4 -> v2.9.1

## [6.15.0-bb.0]

### Updated

- Updated chart version to 6.15.0
- Updated ironbank/opensource/grafana/promtail v2.8.3 -> v2.8.4
- Updated registry1.dso.mil/ironbank/opensource/grafana/promtail v2.8.3 -> v2.8.4
- Updated registry1.dso.mil/ironbank/opensource/jimmidyson/configmap-reload v0.9.0 -> v0.11.1

## [6.13.1-bb.0]

### Updated

- Updated chart version to 6.13.1
- Updated ironbank/opensource/grafana/promtail v2.8.2 -> v2.8.3
- Updated registry1.dso.mil/ironbank/opensource/grafana/promtail v2.8.2 -> v2.8.3

## [6.11.3-bb.0]

### Added

- Bumped chart version to 6.11.3
- Bumped appversion to 2.8.2
- Bumped jimmidyson/configmap-reload to 0.9.0

## [6.11.0-bb.0]

### Added

- added 'vpa.yaml' chart template
- added VPA logic to 'values.yaml'

## [6.11.0-bb.0]

### Added

- Bumped chart version to 6.11.0
- Bumped appversion to 2.8.1

## [6.10.0-bb.0]

### Added

- Bumped appversion to 2.8.0

## [6.10.0-bb.0]

### Added

- Bumped chart version to 6.10.0
- Bumped appversion to 2.7.5

## [6.8.1-bb.3]

### Changed

- Modified network policy for loki egress

## [6.8.1-bb.2]

### Added

- Added network policy for loki egress

## [6.8.1-bb.1]

### Added

- ENV var for node hostname for varlog config
- varlog '/var/log/' system log scrapeconfig support
- '/var/log' & '/etc/machine-id' volume & volumeMounts

## [6.8.1-bb.0]

### Changed

- Bumped chart version to 6.8.1

## [6.7.2-bb.0]

### Changed

- Bumped chart version to 6.7.2
- Bumped appversion to 2.7.0

## [6.2.2-bb.2]

### Added

- Added mTLS to pod monitor

## [6.2.2-bb.1]

### Added

- NetworkPolicy template for `kube-api` egress specifically for Promtail so it can discover pods in cluster

## [6.2.2-bb.0]

### Changed

- Bumped chart version to 6.2.2
- Bumped promtail tag to 2.6.1

## [4.2.0-bb.2]

### Added

- Added network policies to Promtail

## [4.2.0-bb.1]

### Added

- Added initial OSCAL component for Promtail

## [4.2.0-bb.0]

### Changed

- Bumped chart version to 4.2.0
- Bumped promtail tag to v2.5.0

## [3.11.0-bb.1]

### Changed

- Add default PeerAuthentication to enable STRICT mTLS

## [3.11.0-bb.0]

### Changed

- Updated the chart using kpt to be the same version as the upstream 2.4.2 version

## [3.8.1-bb.3]

### Added

- Update Chart.yaml to follow new standardization for release automation

## [3.8.1-bb.2]

### Added

- Added seLinuxOptions to containerSecurityContext

## [3.8.1-bb.1]

### Changed

- Changed resources.requests to equal limits

## [3.8.1-bb.0]

### Added

- Initial commit and release of promtail based on chart from github.com/grafana/helm-charts/charts/promtail
