<p align="center">
  <img height="400px" src="../assets/images/kibana.png">
</p>



## Kibana

[Kibana](https://www.elastic.co/kibana) is an open source data visualization dashboard for Elasticsearch. It provides visualization capabilities on top of the content indexed on an Elasticsearch cluster. Users can create bar, line and scatter plots, or pie charts and maps on top of large volumes of data.

This helm chart is for Kibana 7.3.0. Dedicated nodes for Elasticsearch and Kibana are advised for installing Kibana 7.3.0. It is configured according to the Elasticsearch charts provided before. You can change configurations if you need.

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

To install the chart with the release name Kibana

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

