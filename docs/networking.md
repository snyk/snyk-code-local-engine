# Core Networking

## Re-using the Ingress definition

Follow this documentation to re-use this definition with your own Ingress Controller.

### Pre-requisites

- [REQUIRED]: Obtain the Ingress Class Name for your clusters Ingress Controller.
- [RECOMMENDED]: Set up a DNS record for Snyk Code Local Engine.

### Updating the inbuilt Ingress definition

Set the following in `values-customer-settings.yaml`:

```yaml
global:
  ingress:
    enabled: true
    ingressClassName: <<name-of-ingress-class>>
    host: <<fdqn-dns-record>>
```

Where:

- `ingressClassName` is the name of the IngressClass definition on your cluster
- `host` is an optional DNS record resolving to your Ingress, which will be used by the Ingress Controller.

## Custom Ingress

The inbuilt Ingress definition is used by default to automatically provision routes to the appropriate Snyk Code Local Engine services. If you wish to recreate the Ingress for any other reason, follow these steps.

### Pre-requisites

All the `inbound` requests should be forwarded to `internal-proxy` service, port `10000`.

The `internal-proxy` Service is created by default, and will expose its respective backend pod(s) via `ClusterIP`.

### Disabling the built-in Ingress

Set the following in `values-customer-settings.yaml`:

```yaml
global:
  ingress:
    enabled: false
```

---

## Enabling TLS

_Note: If using a custom ingress, these steps may not apply._

### Adding a Certificate

By default, Snyk Code Local Engine is not secured by TLS. To enable TLS on the provided Ingress:

- a DNS record must be created,
- a certificate generated,
- the following changes made in the `values-customer-settings.yaml` file:

```yaml
global:
  ...
  localEngineUrl: "https://<HOST>"
  ...
  ingress:
    ...
    host: "<HOST>"
    tls:
      enabled: true
      secret:
        ...
        key: |
          <KEY>
        cert: |
          <CERT>
```

Where `<HOST>` is the domain used when the TLS `<CERT>` and `<KEY>` were generated.

Read more about securing an ingress with TLS [here](https://kubernetes.io/docs/concepts/services-networking/ingress/#tls) and TLS secrets [in general](https://kubernetes.io/docs/concepts/configuration/secret/#tls-secrets).

#### Self Signed Certificates

If using a self-signed certificate to secure Snyk Code Local Engine, append the `--insecure` option to any `snyk` commands, or provide the CA to `snyk` by setting `NODE_EXTRA_CA_CERTS`. This [help article](https://support.snyk.io/hc/en-us/articles/360000925358-How-can-I-use-Snyk-behind-a-proxy-) provides further information and support.

---

## Outbound Proxy Support

_Note: Snyk Code Local Engine does not support proxied connections to an external SCM. The following section is for proxying outbound traffic to Snyk domains only._

Components in Snyk Code Local Engine will make TLS-secured outbound connections to Snyk domains (`app.snyk.io` and `broker.snyk.io`) to:

- Authenticate users of the CLI and IDE.
- Make analysis results available on the Snyk Web UI.

If outbound connections to the internet require a proxy, the following may be configured in the `values-customer-settings.yaml` file:

```yaml
global:
  ...
  proxy:
    configMapName: 'proxy-configmap' #default, do not change
    enabled: true
    url: <proxy-url>
    tlsRejectUnauthorized: [false|true]
    usePrivateCaCert: [false|true]
  ...
  privateCaCert:
    enabled: [false|true]
    cert: |
      -----BEGIN CERTIFICATE-----
      ...
      -----END CERTIFICATE-----
    certMountPath: "/etc/config"
```

Where:

- `global.proxy.enabled` is set to true to use an external proxy,
- `global.proxy.url` is the proxy URL, of form `http(s)://<user>:<password>@<uri>:<port>`,
- `global.proxy.tlsRejectUnauthorized` should be set to `true` _only if_ your proxy requires a certificate and one cannot be provided,
- `global.proxy.usePrivateCaCert` should be set to `true` if providing a certificate for the external proxy (see `global.privateCaCert`),
- `global.privateCaCert.cert` should be set to the full certificate chain required to trust your proxy. This should be a multi-line string in `PEM` format. This must be specified if `global.proxy.usePrivateCaCert` is `true`.

### Further Proxy Configuration

- If your proxy uses whitelisting, ensure the `app.snyk.io` and `broker.snyk.io` (if using broker) domains are added.
- If your proxy certificate and/or authentication information changes, update the value(s) above and re-apply Helm. Any running pods that use the proxy will be recreated with the new proxy configuration.

---

## Private Certificate Authority Support for SCM

If the SCM used with Snyk Code Local Engine requires clients to trust a private/self-signed certificate, the following may be configured in the `values-customer-settings.yaml` file:

```yaml
global:
  privateCaCert:
    enabled: true
    cert: |
      -----BEGIN CERTIFICATE-----
      ...
      -----END CERTIFICATE-----
```

Where:

- `global.privateCaCert.enabled` is set to true to provide a certificate to components that interact with an SCM,
- `global.privateCaCert.cert` should be set to the full certificate chain required to trust your SCM. This should be a multi-line string in `PEM` format.

## Private Certificate Authority Support for both SCM and Proxy

To enforce certificate trust towards both SCM and Proxy, provide one or more CA certificates to the `global.privateCaCert.cert` key:

```yaml
global:
  ...
  proxy:
    enabled: true
    url: <proxy-url>
    tlsRejectUnauthorized: false
    usePrivateCaCert: true
  ...
  privateCaCert:
    enabled: true
    cert: |
      -----BEGIN CERTIFICATE-----
      ...
      -----END CERTIFICATE-----
      -----BEGIN CERTIFICATE-----
      ...
      -----END CERTIFICATE-----
```

These certificates will be concatenated to a single file and mounted by all services that interact with the outbound proxy, the SCM, or both.
