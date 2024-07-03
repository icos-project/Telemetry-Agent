# Installation

## Kubernetes

Create a `my-values.yaml` file that will specify the configuration of the Telemetry Agent. At minimum, the following three values should be specified:

```yaml
# the Telemetry Controller (or another Telemetry Agent) endpoint where to send data to
telemetryControllerEndpoint: 10.160.3.20:32103

# the ICOS Agent ID for this instance
agentId: icos-agent-1
```
The depoyment can be started with:

```bash
helm install --namespace icos-system my-release-name oci://harbor.res.eng.it/icos/helm/telemetry-agent --values my-values.yaml
```

??? note "Development Versions"

    In order to install versions not yet released, the url of the chart needs to be change to `oci://harbor.res.eng.it/icos-private/helm/telemetry-agent`.

    In addition, since unreleased versions are private, you need to login to the repository before launching the install command:

    ```bash
    helm registry login harbor.res.eng.it/icos-private/helm
    ```

    and provide your credentials.

### Enabling cluster-level collector

By default the Telemetry Agent deploys an agent (`telemetry-node-agent`) on each node of the Kubernetes cluster (using a `Daemonset` resource). Each agent directly sends telemetry to the upstream telemetry component (e.g. a Telemetry Controller). This requires internet connectivity from each node to the upstream component.

In case in which not all the nodes of the cluster have connectivity to internet and/or visibility of the upstream telemetry node (e.g., in case that a VPN connection is needed and only one node of the cluster is connected to the VPN) it is possible to deploy an addtional component that will collect all the metrics produced in the cluster and will send them.

For instance, if the only node in the cluster that has network visibility of the Telemetry Controller is the node named `jetson-1`, it is possible to deploy the telemetry agent with the cluster-level collector and force it to be deployed on the node with connectivity:

```yaml
otel-cluster-collector:
  enabled: true
  nodeSelector:
    kubernetes.io/hostname: jetson1
```


## Docker

1. Get the docker compose packaged
```bash
git clone https://production.eng.it/gitlab/icos/meta-kernel/observability/telemetry-agent.git
```  

2. Create a file named `telemetry-agent.env`
```ini
ICOS_AGENT_ID=nuvla-ag-9
ICOS_TELEMETRY_CONTROLLER_ENDPOINT=10.160.3.20:32103
ICOS_TELEMETRY_AGENT_CONFIG=full
```

3. Deploy
```bash
docker compose --env-file telemetry-agent.env --file telemetry-agent/deploy/docker/docker-compose.yml up --detach
```