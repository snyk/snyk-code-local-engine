# Prerequisites

Before proceeding to install Snyk Code Local Engine, ensure you have:

- Image Pull Secrets (these are provided to you by Snyk), and
- Snyk Broker Settings (to connect to your private Source Code Management system)
- An installation of [Helm](https://helm.sh/) `3.8.0` or higher (`NOTE: we only support install and upgrade via helm`).
- Access to your target Kubernetes Cluster to deploy the Snyk Code Local Engine Helm Chart

## Deployment Requirements

### Default Deployment (Dedicated Cluster)

Refer to [Snyk Code Local Engine System Requirements](https://docs.snyk.io/products/snyk-code/deployment-options/snyk-code-local-engine/introduction#system-requirements) for the most up-to-date requirements.

### Advanced Deployments

Snyk Code Local Engine can be installed on a shared Kubernetes Cluster. This does not change resource requirements, and comes with additional concerns. Review [Advanced Deployment Models - Shared Cluster](deployment-advanced.md#shared-cluster) to understand the impact.

## Network Requirements

Snyk Code Local Engine will communicate with Snyk to:

- Trigger Code Analyses
- Check Snyk Code Local Engine settings for Snyk CLI
- Manage inbound webhooks from SCM

The domains Snyk Code Local Engine requires outbound connectivity (supporting WebSockets) to are `*.snyk.io`. All outbound connections are secured with HTTPS/TLS encryption.

### Outbound Proxy

Connections to Snyk domains from Snyk Code Local Engine may be routed through a proxy. Review [Outbound Proxy](networking.md#outbound-proxy-support) for further information.

---