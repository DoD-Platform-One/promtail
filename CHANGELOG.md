# Changelog

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/), and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

---
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
