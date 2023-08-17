# Changelog

All notable changes to Code Local Engine project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## v2.4.2
#### 2023-08-17

### Added
- Snyk Code Local Engine now supports custom CAs towards SCMs via `global.privateCaCert.*` values.
- A subset of available Helm values are listed in documentation
- A subset of available Helm values are subject to input validation
- The IDE has been added to the Architecture diagram

#### Changed
- NGINX Ingress documentation has been updated to better reflect usage and deployment options
- The healthcheck calls made by `service-health-aggregator` are now cached

### Deprecated
- The `global.proxy.cert` and `global.proxy.useCustomCert` values are both _deprecated_. See documentation for `global.privateCaCert.*` values

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