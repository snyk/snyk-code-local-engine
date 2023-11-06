# Advanced Deployments

Our current recommended deployment model requires a dedicated Kubernetes cluster. For alternative approaches, follow the guide below most relevant to your requirements.

## Shared Cluster

Using a shared Kubernetes cluster allows Snyk Code Local Engine to be installed alongside pre-existing workloads. The following considerations are important:

- Snyk Code Local Engine has significant resource requirements and may negatively impact other workloads already running on the cluster if capacity is low. Snyk Code Local Engine may be unable to fully schedule. See [Snyk Code Local Engine Requirements](prerequisites.md#prerequisites) to ensure your cluster has adequate resources available.

- By default, Kubernetes schedules pods for Snyk Code Local Engine on any available node within the cluster. See [Using `tolerations`, `affinity` or `nodeSelector`](#using-tolerations-affinity-or-nodeselector) for advanced resource targeting.

- By default, Helm installs to the `default` namespace. To change this, provide a namespace with `-n <namespace>` to Helm when installing. If the namespace does not exist, provide the `--create-namespace` flag as well (assuming your user is privleged to create namespaces).

### Using `tolerations`, `affinity` or `nodeSelector`

_This section will build on information given in [Kubernetes documentation](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/)_

To target some/all of the workloads to particular nodes, specify the some/all of the mechanisms as follows (example values are provided, adjust these to match your own node labels/other running workloads):

```yaml
service_name:
    nodeSelector: #matches a node with label `NodeType` and value `large`
        NodeType: large 
    tolerations: #matches if a node has taint `bigNode` with value `"true"`. The taint blocks other workloads that do not have this toleration.
        - key: "bigNode"
          operator: "Equal"
          value: "true"
          effect: "NoSchedule"
    affinity: #schedules only on nodes with `kubernetes.io/os:linux`
        nodeAffinity:
        requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
            - matchExpressions:
            - key: kubernetes.io/os
                operator: In
                values:
                - linux
```

This approach is recommended for the heaviest workloads in Snyk Code Local Engine (`bundle`, `suggest`) to avoid the considerations mentioned in [Shared Cluster](#shared-cluster).

Making these changes in `values-customer-settings.yaml` could look like:

```yaml
...
suggest:
    ...
    nodeSelector:
        NodeType: large
    ...
```

---