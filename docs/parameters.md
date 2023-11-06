# Helm Parameters

The available Helm Parameters are listed below.

## Parameters

### Enable Snyk Code Services

| Name              | Description                                | Value   |
| ----------------- | ------------------------------------------ | ------- |
| `tags.scm`        | Enable services required for SCM Imports   | `false` |
| `tags.scmPrCheck` | Enable services required for SCM PR Checks | `false` |
| `tags.cli`        | Enable services required for CLI           | `false` |
| `tags.ide`        | Enable services required for IDE           | `false` |

### Global Parameters

| Name                                          | Description                                                                                                                                                                | Value                                    |
| --------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------- |
| `global.imagePullSecret.enabled`              | Disable if an Image Pull Secret is not required                                                                                                                            | `true`                                   |
| `global.imagePullSecret.name`                 | The name of the Image Pull Secret to be created                                                                                                                            | `snyk-code-local-engine-pull-secret`     |
| `global.imagePullSecret.credentials.username` | A Username to authenticate against a Docker registry                                                                                                                       | `""`                                     |
| `global.imagePullSecret.credentials.password` | A Password to authenticate against a Docker registry                                                                                                                       | `""`                                     |
| `global.imagePullSecrets`                     | Set to '[]' if your image registry does not require authentication, or '[ `<existing-secret-name>` ] if re-using credentials that already exist on your Kubernetes cluster | `["snyk-code-local-engine-pull-secret"]` |
| `global.imageRegistry`                        | Optionally define a private image registry address if using non-default image registries (hostname/port only, no protocol)                                                 | `""`                                     |
| `global.localEngineUrl`                       | The full URL including schema that points to the cluster ingress. Required for CLI/PR Checks.                                                                            | `""`                                     |
| `global.proxy.enabled`                        | Set to true to enable outbound proxy support                                                                                                                               | `false`                                  |
| `global.proxy.url`                            | Proxy URL, including schema: http[s]://username:password@proxy:port                                                                                                        | `""`                                     |
| `global.proxy.tlsRejectUnauthorized`          | Set to true to trust any and all certificates presented by the proxy                                                                                                       | `false`                                  |
| `global.proxy.usePrivateCaCert`               | Set to true to enable private CA support for the proxy - see global.privateCaCert                                                                                          | `false`                                  |
| `global.privateCaCert.enabled`                | Set to true to enable trust of private CA certificate(s) towards the SCM and/or proxy                                                                                      | `false`                                  |
| `global.privateCaCert.cert`                   | Multiline string containing any/all certificates for connections to the SCM and/or proxy in PEM format ()                                                                  | `""`                                     |
| `global.privateCaCert.certMountPath`          | A fully qualified path to mount the certificate                                                                                                                            | `/etc/config`                            |
| `global.ingress.enabled`                      | Set to true to create an Ingress for CLI/PR Checks                                                                                                                         | `false`                                  |
| `global.ingress.host`                         | Optionally define the host associated with this ingress - otherwise leave blank                                                                                            | `""`                                     |
| `global.ingress.ingressClassName`             | Optionally define the Ingress Class for this ingress - otherwise leave blank                                                                                               | `""`                                     |
| `global.ingress.tls.enabled`                  | Set to true to enable TLS on the in-built ingress                                                                                                                          | `false`                                  |
| `global.ingress.tls.secret.name`              | Either specify the name of a pre-existing Kubernetes secret containing TLS secrets, or leave blank to create a new secret                                                  | `{{ .Release.Name }}-ingress-tls`        |
| `global.ingress.tls.secret.key`               | The TLS key for TLS encryption, in PEM format                                                                                                                              | `""`                                     |
| `global.ingress.tls.secret.cert`              | The TLS certificate for TLS encryption, in PEM format                                                                                                                      | `""`                                     |
| `global.localEngine.redisSecretName`          | Optionally specify the name of a pre-existing Kubernetes secret containing Redis authentication data                                                                       | `""`                                     |
| `global.localEngine.sessionSecretName`        | Optionally specify the name of a pre-existing Kubernetes secret containing session secret data                                                                             | `""`                                     |
| `global.localEngine.mongodbSecretName`        | Optionally specify the name of a pre-existing Kubernetes secret containing MongoDB authentication data                                                                     | `""`                                     |

### Broker Client parameters (any SCM flows only)

| Name                                | Description                                                                                                            | Value   |
| ----------------------------------- | ---------------------------------------------------------------------------------------------------------------------- | ------- |
| `broker-client.brokerToken`         | Broker Token is a value from Snyk. Get this from the Snyk integration settings page or your Snyk Representative        | `""`    |
| `broker-client.brokerType`          | Define the SCM Broker will connect to                                                                                  | `""`    |
| `broker-client.githubHost`          | GHE URL - Ex: your.ghe.domain.com (do not prepend HTTPS) - For GHE Cloud use api.github.com                            | `""`    |
| `broker-client.githubApi`           | GHE API Address - do not prepend HTTPS                                                                                 | `""`    |
| `broker-client.githubGraphQl`       | GHE Graph QL Address - do not prepend HTTPS                                                                            | `""`    |
| `broker-client.githubToken`         | Github Token - for GitHub or GitHub Enterprise                                                                         | `""`    |
| `broker-client.bitbucketUsername`   | Bitbucket Server Username                                                                                              | `""`    |
| `broker-client.bitbucketPassword`   | Bitbucket Server Password                                                                                              | `""`    |
| `broker-client.bitbucketHost`       | Bitbucket Server Host URL - do not prepend HTTPS                                                                       | `""`    |
| `broker-client.gitlabHost`          | GitLab URL - do not prepend HTTPS                                                                                      | `""`    |
| `broker-client.gitlabToken`         | GitLab Token                                                                                                           | `""`    |
| `broker-client.azureReposOrg`       | Azure Repos Organization                                                                                               | `""`    |
| `broker-client.azureReposHost`      | Azure Repos Hostname - do not prepend HTTPS                                                                            | `""`    |
| `broker-client.azureReposToken`     | Azure Repos Token                                                                                                      | `""`    |
| `broker-client.codeSnippet.enabled` | Set to enable viewing of analysis results in the Snyk UI. Caution - will allow Snyk servers to fetch source code files | `false` |
| `broker-client.largeManifestFileRule.enabled` | Set to enable in order to be able to fetch large manifest files (> 1Mb). Avaliable only for 'github' and 'github-enterprise' | `false` |
| `broker-client.brokerServerUrl`     | Required if using Snyk Private Cloud                                                                                   | `""`    |
