## Kibana Helm Chart

This helm chart is for Kibana 7.3.0. Dedicated nodes for elasticsearch and kibana are advised for installing kibana 7.3.0. It is configured according to the elasticsearch charts provided before. You can change configurations if you need.

## Chart details

Here, we assume that the nodes are having taints -

`dedicated=monitoring:NoExecute`

and labels -

`used_for=monitoring`

To apply these on your nodes, run

```
kubectl taint nodes <node_name> dedicated=monitoring:NoExecute
kubectl label nodes <node_name> used_for=monitoring
```

## Installing the Chart

To install the chart with the release name kibana

```
helm upgrade --recreate-pods --install kibana . --namespace monitoring
```

## Uninstalling the charts

```
helm del --purge kibana
```



## Configurations

| Parameter                                   | Description                        | Default                                         |
| ------------------------------------------- | ---------------------------------- | ----------------------------------------------- |
| `image.repository`                          | Container image repository         | `docker.elastic.co/kibana/kibana`               |
| `image.tag`                                 | Container image tag                | `7.3.0`                                         |
| `image.pullPolicy`                          | Container image pull policy        | `IfNotPresent`                                  |
| `ingress.enabled`                           | Enables Ingress                    | `false`                                         |
| `elasticsearch.hosts`                       | elasticsearch hosts                | `"http://elasticsearch-client.monitoring:9200"` |
| `nodeSelector`                              | Node selector                      | `used_for: monitoring`                          |
| `tolerations`                               | Tolerations                        | `dedicated=monitoring:NoExecute`                |
| `service.type`                              | Type of service                    | `ClusterIP`                                     |
| `service.ports.port`                        | External port for the service      | `80`                                            |
| `service.ports.targetPort`                  | Internal port for the service      | `5601`                                          |
| `service.ports.protocol`                    | Protocol for service               | `TCP`                                           |
| `service.ports.name`                        | Service port name                  | `http`                                          |
| `resources`                                 | pod resource requests & limits     | `{}`                                            |
| `environmentVariable[0].ELASTICSEARCH_HOST` | elasticsearch host along with port | `http://elasticsearch-client.monitoring:9200`   |

