## Fluent-bit Helm Chart

This helm chart is for fluentibit 7.3.0. 

## Chart details

This chart will do the following:

- Install a configmap for Fluent Bit
- Install a daemonset that provisions Fluent Bit [per-host architecture]

## Installing the Chart

To install the chart with the release name fluent-bit

```
helm upgrade --recreate-pods --install fluent-bit . --namespace monitoring
```

## Uninstalling the charts

```
helm del --purge fluent-bit
```



## Configuration

| Parameter                    | Description                                                  | Default                                                      |
| ---------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| `image.repository`           | Container image repository                                   | `fluent/fluent-bit`                                          |
| `image.tag`                  | Container image tag                                          | `1.2.1`                                                      |
| `image.pullPolicy`           | Container image pull policy                                  | `IfNotPresent`                                               |
| ```backend.type```           | Set the backend to which Fluent-Bit should flush the information it gathers | `es`                                                         |
| `backend.es.host`            | Target host where Fluent-Bit or Fluentd are listening for Forward messages | `{$ELASTICSEARCH_HOST}`                                      |
| `backend.es.port`            | TCP Port of the target service                               | `9200`                                                       |
| `backend.es.index`           | Elastic Index name                                           | `kubernetes_cluster-0001`                                    |
| `backend.es.logstash_prefix` | Index Prefix. If Logstash_Prefix is equals to 'mydata' your index will become 'mydata-YYYY.MM.DD'. | `kubernetes_cluster`                                         |
| `nodeSelector`               | Node selector                                                | `used_for: monitoring`                                       |
| `tolerations`                | Tolerations                                                  | `dedicated=monitoring:NoExecute`                             |
| `data.parsers.conf`          | Parsers for fluent-bit                                       | parsers for apache, apache-error, nginx, json, docker, syslog, k8s-nginx-ingress |
| `resources`                  | pod resource requests & limits                               | `{}`                                                         |

