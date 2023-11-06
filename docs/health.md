# Snyk Code Local Engine Health

Snyk Code Local Engine exposes useful endpoints to interrogate the status of your installation.

## `/status`

The `/status` endpoint allows the `service-health-aggregator` pod to report on the status of your installation.

```bash
curl http[s]://<LOCAL_ENGINE_URL>/status
```
_NOTE: The response will be cached by default for 60 seconds._

### Healthy Response

The following message is returned if all pods and services are running:
```
{
    "message": "Snyk Code is healthy! 🐶",
    "ok": true,
    "version": "<version_of_snyk_code_local_engine>"
}

```

### Unhealthy Response

A JSON object is returned with details of the unhealthy workloads. If a `503` is returned, the `service-health-aggregator` pod is unable to start correctly.

## `/broker/healthcheck`

The `/broker/healthcheck` endpoint ensures the `broker-client` pod is able to communicate with Snyk correctly.

```bash
curl http(s)://<LOCAL_ENGINE_URL>/broker/healthcheck
```

### Healthy Response

```json
{
    "ok": true,
    "websocketConnectionOpen": true,
    "brokerServerUrl": "https://broker.snyk.io",
    "version": "<version_of_broker>",
    "transport": "websocket"
}
```

## `/api/healthcheck`

The `/api/healthcheck` endpoint tests the health of the `sast-analysis-api` pod.

```bash
curl http[s]://<LOCAL_ENGINE_URL>/api/healthcheck
```

### Healthy Response
```json
{"gitSha":"sha","ok":true}
```

---