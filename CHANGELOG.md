# Changelog

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/), and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

---
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
