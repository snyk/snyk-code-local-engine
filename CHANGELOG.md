# Changelog

All notable changes to Code Local Engine project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

<a id="v2.7.6" />

## v2.7.6

#### 2023-12-07

### Added

- CronJobs to clean up older/expired data in MongoDB

### Changed

- Minimum Kubernetes version is now 1.21

<a id="v2.7.5" />

## v2.7.5

#### 2023-11-27

### Fixed

- Fixed non-reachable vulnerabilities in the scm-bundle-store and mongodb components.

<a id="v2.7.4" />

## v2.7.4

#### 2023-11-23

### Changed

- Corrected the list of images under the Private Registry section

### Removed

- The `scm-meld` component is no longer required and has been removed

### Fixed

- Resolved an "Unauthorized" failure during IDE and CLI scans occurring after a proxy CA certificate change. The fix ensures that Snyk Code Local Engine properly picks up the new configuration when redeployed.

<a id="v2.7.3" />

## v2.7.3

#### 2023-11-10

### Changed

- The `/status` endpoint is now also presented on `/` for better compatibility with Load Balancer health checks

<a id="v2.7.2" />

## v2.7.2

#### 2023-11-08

### Changed

- Updated the `broker-client` to [v4.169.2](https://github.com/snyk/broker/releases/tag/v4.169.2)
- Updates to latest Snyk Code rules
- Updates to Snyk Code services

### Fixed

- Fixed outbound CA support for SCM when a proxy is not utilized

<a id="v2.7.1" />

## v2.7.1

#### 2023-10-17

### Changed

- Updated the architectural diagram with `suggest-sticky` component

<a id="v2.7.0" />

## v2.7.0

#### 2023-10-17

### Added

- Introduced a new `largeManifestFileRule` value, gives the option to add rule for fetching large manifest file. Avaliable for Github and Github Enterprise only.
- Caching mechanism for IDE scans by the new `suggest-sticky` component.

### Changed

- Updated architecture diagram with new internal-proxy connectivity

## v2.6.1

#### 2023-09-20

### Changed

- Updated Ingress template to include the `host` key if specified
- Updated documentation for JetBrains IDE

## v2.6.0

#### 2023-09-18

### Added

- `internal-proxy` component (based on `envoy`) replaces routes previously defined by the Ingress resource

### Changed

- Updated the `broker-client` to [v4.163.0](https://github.com/snyk/broker/releases/tag/v4.161.0)
- Ingress resource simplified - defies one route (`/`) and removes the need for request rewriting/regex capture groups

### Fixed

- Updated documentation for proxy and custom Certificate Authority support for better clarity
- Specify the `brokerServerUrl` by default in the `values-customer-settings.yaml` file

### Removed

- Any references to the previously-used MongoDB Sharded cluster in documentation
- The pre-packaged NGINX Ingress Controller is removed. Functionality is handled internally by the `internal-proxy` component

## v2.5.0

#### 2023-09-04

### Added

- Allows python projects that use poetry to be scanned by Snyk Open Source through the broker

### Changed

- Updated the `broker-client` to [v4.161.0](https://github.com/snyk/broker/releases/tag/v4.161.0)
- Updated Snyk Code services for latest analysis rules
- Changed database infrastructure for `scm-bundle-store` from a sharded MongoDB cluster to a single MongoDB instance

### Fixed

- Fixed an upgrade/stability issue with MongoDB by migrating to a single MongoDB instance

## v2.4.2

#### 2023-08-17

### Added

- Snyk Code Local Engine now supports custom CAs towards SCMs via `global.privateCaCert.*` values.
- A subset of available Helm values are listed in documentation
- A subset of available Helm values are subject to input validation
- The IDE has been added to the Architecture diagram

#### Changed

- NGINX Ingress documentation has been updated to better reflect usage and deployment options

### Deprecated

- The `global.proxy.cert` and `global.proxy.useCustomCert` values are both _deprecated_.

## v2.4.1

#### 2023-07-14

### Added

- The inbuilt NGINX Ingress Controller is now disabled by default, and is separate from the Ingress resource. This enables customers to re-use their own instance of NGINX Ingress Controller without manually manipulating the Chart.
  - To enable the NGINX Ingress Controller, set `global.ingressController.enabled: true`.

## v2.4.0

#### 2023-07-13

### Added

- Support for custom image registries:
  - Authenticated/unauthenticated private registries
  - Custom image pull secrets

## v2.3.0

#### 2023-07-12

### Added

- IDE Scans for VSCode v1.21 and higher

## v2.2.3

#### 2023-06-15

### Added

- Update of scm-meld to support custom CA override
- Update of files-bundle-store to improve CPU usage, and concurrency

## v2.2.2

#### 2023-06-13

### Fixed

- Suggest has been upgraded with some key bug fixes:
  - Better queueing mechanism to reduce stuck analyses
  - Introduced better analyses timeout mechanisms
  - Suggest runs as non-root

## v2.2.1

#### 2023-06-09

### Added

- Migrates additional services to run as non-root

### Fixed

- Inconsistency when deploying Local Engine to a custom namespace
- Webhook creation for PR checks

## v2.2.0

#### 2023-05-12

### Added

- Modular service deployment, only deploy the services needed for the intended use case
- PR check functionality
- Ability to configure self managed secrets
- Partial standardisation of service base images (more to follow)
- Partial migration of services not to run as root anymore (more to follow)
- Updates core Snyk Code services to include new rule sets

### Removed

- We removed CRDs and ClusterRoles - no more cluster-wide access needed.

## v2.0.0

#### 2023-04-20

### Added

- Includes the “new” Snyk Code stack, giving customers parity between Snyk SaaS and Local Engine environments.
- We host the Helm Chart on Dockerhub - customers can pull the Helm Chart with the same credentials for v1.Documentation and the values-customer-settings.yaml are still shared manually with the customer.

### Removed

- We removed CRDs and ClusterRoles - no more cluster-wide access needed.

### Changed

- This release does not include PR Checks. CLI scans/ Imports are currently supported.
- This release does not include pulling images from a custom registry.
- This release does not include centralised logging.
