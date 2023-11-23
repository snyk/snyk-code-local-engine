# Basic Deployments

Snyk Code Local Engine leverages Helm for installation. Follow these steps to obtain the Snyk Code Local Engine Helm Chart and perform basic configuration.

## Pulling the Snyk Code Local Engine Helm Chart

Snyk Code Local Engine Helm Charts are hosted on Dockerhub, and can be obtained by:

```sh
$ helm registry login -u <username> registry-1.docker.io
Password:
Login suceeded
```

Where `username` and `password` are those provided by Snyk.

Once authenticated, pull the Snyk Code Local Engine Helm Chart:

```
helm pull oci://registry-1.docker.io/snyk/snyk-code-local-engine <--version x.y.z>
```

Optionally provide a version, otherwise Helm will fetch the latest version of the Chart.

## Configuring the Snyk Code Local Engine Helm Chart

Configuration of Snyk Code Local Engine to suit your environment and requirements is carried out by modifying the `values-customer-settings.yaml` file provided with this documentation.

This YAML file will be used to keep settings between Snyk Code Local Engine upgrades, and may contain sensitive information.

### Pulling Snyk Code Local Engine Docker Images

Snyk Code Local Engine uses a mixture of Snyk and third party Docker images. Add your `imgePullSecret` information to the `values-customer-settings.yaml` file:

```yaml
global:
  imagePullSecret:
    credentials:
      username: <username>
      password: <password>
```

Helm will create a Kubernetes Secret with these credentials to allow your cluster to pull Snyk images from Dockerhub.

### Enabling Required Services

Snyk Code Local Engine consists of multiple services which provide functionality for different integration methods and features.

To enable and deploy the services required for your desired usage set the tag values to `true` in `values-customer-settings.yaml`:

```yaml
tags:
  scm: false # set to true to deploy services required for SCM integration
  scmPrCheck: false # set to true to deploy services required for SCM PR check functionality
  cli: false # set to true to deploy services required for CLI integration
  ide: false # set to true to deploy services required for supported IDE plugins
```

### Enabling CLI, IDEs or PR Checks

Snyk Code Local Engine needs to be accessible from outside the cluster for CLI, IDEs or PR checks to work. By default, this functionality is disabled.

If using the inbuilt ingress template, set the following in `values-customer-settings.yaml`:

```yaml
global:
  ingress:
    enabled: true
```

Depending on your Kubernetes Cluster you may wish to use a [custom Ingress Controller and/or a custom Ingress template](networking.md#).

***JetBrains IDE limitation:**
To ensure Snyk Code scans are directed to SCLE please set up your SCLE organization ID in the JetBrains IDE settings and restart the IDE. Having to change organization only applies if Snyk Code without using the SCLE was used beforehand.*

## Installing Snyk Code Local Engine with Helm

The following should be used to install Snyk Code Local Engine for the first time on a Kubernetes cluster:

```bash
helm upgrade --wait <DEPLOYMENT_NAME> ./snyk-code-local-engine-<version>.tgz -i -f values-customer-settings.yaml -n <NAMESPACE>
```

Where:

- `<DEPLOYMENT_NAME>` will identify the Helm Chart controlled resources on the Kubernetes Cluster,
- `-f values-customer-settings.yaml` passes any values from this YAML file to the Helm Chart, and
- `-n <NAMESPACE>` is optional - use this if installing Snyk Code Local Engine to a [Shared Cluster](deployment-advanced.md#shared-cluster). If not specified, the `default` namespace is used.

If a re-install is needed (i.e a resource that cannot be patched is changed), first [remove the installation](#removing-snyk-code-local-engine) and then install again.

## Upgrading Snyk Code Local Engine with Helm

As Snyk Code Local Engine is stateless, upgrades may be performed without considering any data loss. The same command may be used to perform an upgrade:

```bash
helm upgrade --wait <DEPLOYMENT_NAME> ./snyk-code-local-engine-<version>.tgz -i -f values-customer-settings.yaml -n <NAMESPACE>
```

Versions within the same minor or patch increment are safe to install without removal. Upgrades from a previous major version should be a clean installation (remove and install).

| Version Change | Upgrade/Clean Install | Process |
| --- | --- | --- |
| `x.y.1` to `x.y.2` | `Upgrade` | Perform the `helm upgrade ...` command |
| `x.1.z` to `x.2.z` | `Upgrade` | Perform the `helm upgrade ...` command |
| `1.y.z` to `2.y.z` | `Clean Install` | First [remove the installation](#removing-snyk-code-local-engine) and perform the `helm upgrade ...` command |

## Removing Snyk Code Local Engine

Snyk Code Local Engine may be removed with:

```
helm delete <DEPLOYMENT_NAME> -n <NAMESPACE>
```

This removes all components deployed by the Helm Chart. The `--purge` option may be used to delete all history if required.

## Private Registry

Use the following steps in order to work with your private registry:

1. Configure `customer-values-settings` file, make sure you mark which analysis flows will be used by settings the `tags`

2. Get the list of images and tags to pull by executing the following command:
   `helm template ./snyk-code-local-engine-<version>.tgz -f values-customer-settings.yaml | yq '.. | .image? | select(.)' | sort | uniq`

3. Push the images to your registry in the following format:

   - Any `snyk` images should reside within the `snyk` namespace
   - `mongodb`, `redis` images should reside within the `bitnami` namespace
   - `envoy` image should reside within the `envoyproxy` namespace
   - `mongo` image should reside within the root of the registry with no namespace

  ```
  snyk/
  ├── broker/
  ├── code-pr-check-service/
  ├── deepcode-suggest_v6/
  ├── deeproxy/
  ├── files-bundle-store/
  ├── sast-analysis-api/
  ├── scm-bundle-store/
  ├── service-health-aggregator/
  envoyproxy/
  ├── envoy/
  bitnami/
  ├── mongodb/
  ├── redis/
  ```

1. Make sure you set the `global.imageRegistry` value in `values-customer-settings.yaml` to your private registry include any namespace paths above those mentioned previously. For example: `global.imageRegistry: my.private.registry.io/parent/path` will pull snyk images from `my.private.registry.io/parent/path/snyk` and so on.

The note formatting used elsewhere is:

_Note: If your private registry requires authentication, ensure to set credentials as describe in `values-customer-settings.yaml`._

After setting previous steps Helm will now use images from the specified registry.

## What Next?

- Check the [health of a running Snyk Code Local Engine installation](health.md)
- Set up any [networking for your installation](networking.md)
- Configure your [installation with Snyk](post-install-snyk.md)

---
