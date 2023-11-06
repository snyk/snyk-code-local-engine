# Snyk Configuration

_Note: At this point, you should have a running installation of Snyk Code Local Engine. As IP addresses may be dynamic/change, it is highly recommended to set up a DNS record._

If using either the Snyk CLI or PR Checks, Snyk requires one of the following once a Snyk Code Local Engine installation is complete:

- The DNS name of the ingress of the cluster, or
- The IP address of the ingress of the cluster

This allows the Snyk CLI/IDE plugins to function correctly.

To specify the DNS name or IP address of the ingress:

- Add the following to `values-customer-settings.yaml`:

```yaml
global:
  ...
  localEngineUrl: <either your assigned DNS record or your IP address - for example, http://local-engine.domain or http:/10.170.1.40>
```

- Execute ` helm upgrade --wait <DEPLOYMENT_NAME> snyk-code-local-engine-<VERSION>.tgz -i -f values-customer-settings.yaml`

**If an IP address was previously provided to your Snyk representative, please update us with your new DNS record.**

## Enabling Snyk Code Local Engine for Snyk CLI/IDE

Once enabled, the Snyk Code Local Engine URL/IP address will be shown in `Settings`, under the `Snyk Code` section.

### Configure the Snyk CLI

If using a particular Snyk Organization for Snyk Code Local Engine, ensure that either:

- The `--org=<LOCAL_ENGINE_ORG_NAME>` flag is set when using the Snyk CLI, or
- `snyk config set org=<LOCAL_ENGINE_ORG_NAME>` is executed

This ensures Snyk Code analysis requests are directed to Snyk Code Local Engine.

To confirm, check the output of `snyk code test -d`:

```bash
...
snyk-code ---> API request log  => HTTP GET http://<LOCAL_ENGINE_URL_OR_IP>/api/filters
...
```

## Updating Snyk Code Local Engine - Snyk Configuration

If either of these change:

- The DNS name assigned to the ingress of the cluster, or
- If not using DNS records, the IP address assigned to the ingress of the cluster, or
- [TLS encryption](networking.md#enabling-tls) is enabled/disabled,

Snyk must be provided with the updated DNS name/IP address.

## What Next?

Once your cluster is configured, Snyk Code Local Engine is ready for use. Results will appear on the Snyk Web UI after setting up PR Checks or importing Projects.

---