# Applications configuration 


## Metrics


### Kuberentes Metrics Endpoints Discovery

In Kubernetes, the Telemtry Agent is able to discover endpoints that exposes metrics in two ways: using [annotations](#annotations) or using [CRDs](#crds).

#### Annotations

To configure scraping of metrics from an endpoint exposed by a service or a pod, the following annotations are supported when added to a service or a pod resource:

```yaml
annotations:

  # enable/disable the scraping of metrics from this service/pod
  # Default: false
  telemetry.icos.eu/scrape: true

  # the interval of time at which the endponint will be scraped
  # Default: 60s
  telemetry.icos.eu/interval: 120s

  # the timeout for the scraping call
  # Default: 2s
  telemetry.icos.eu/timeout: 10s

  # the protocol to use (e.g, http, https)
  # Default: http
  telemetry.icos.eu/scheme: http

  # the path at which scrape
  # Default: /
  telemetry.icos.eu/path: /metrics

  # the port at which scrape
  # Default: 80
  telemetry.icos.eu/path: 9100
```
These annotations are very similar to the `prometheus.io/*` annotations defined by the Prometheus Kube Stack

#### CRDs

The Telemetry agent watches [Service Monitor](https://github.com/prometheus-operator/prometheus-operator/blob/main/Documentation/api.md#monitoring.coreos.com/v1.ServiceMonitor) and [PodMonitor](https://github.com/prometheus-operator/prometheus-operator/blob/main/Documentation/api.md#monitoring.coreos.com/v1.PodMonitor) CRDs and configure itself to scrape metrics from the endpoints pointed by those resources.

==TODO: add an example==
## Logs

### Kuberentes Pod Logs Discovery

By default, logs from pods are not collected. To enable them, just add an annotation on the pod.

```yaml
annotations:

  # enable/disable the collection of logs from this pod
  # Default: false
  logs.icos.eu/scrape: true
```

==TODO: multi-container pods configuration==


