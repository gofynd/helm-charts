<p align="center">
  <img  src="../assets/images/elasticsearch.png">
</p>

## Elasticsearch

[Elasticsearch](https://www.elastic.co/elasticsearch/) is a search engine based on the Lucene library. It provides a distributed, multitenant-capable full-text search engine with an HTTP web interface and schema-free JSON documents. Elasticsearch is developed in Java.

Elasticsearch does not communicate with the Kubernetes API, hence no need for RBAC permissions. Since elasticsearch 7.x is much heavier than 6.x, we **highly advise** you to create separate dedicated instance with minimum two nodes having at least 16 GiB memory  and 4 vCPU. Taints and tolerations are applied on the nodes to control deployment of pods.

## Prerequisites

Kubernetes 1.10+

PV dynamic provisioning support on the underlying infrastructure

Dedicated elasticsearch nodes with appropriate taints and tolerations.

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

## Installing the charts

```bash
helm upgrade --recreate-pods --install elasticsearch-master elasticsearch/elasticsearch-master --namespace monitoring

helm upgrade --recreate-pods --install elasticsearch-data elasticsearch/elasticsearch-data --namespace monitoring
```

wait for both the pods to come up as this may take a while. After elasticsearch-master and elasticsearch-data, install elasticsearch-client

```bash
helm upgrade --recreate-pods --install   elasticsearch-client elasticsearch/elasticsearch-client --namespace monitoring
```

## Uninstalling the charts

```bash
helm del --purge elasticsearch-client
helm del --purge elasticsearch-data
kubectl delete pvc elasticsearch-data-persistent-storage-elasticsearch-data-0 -n monitoring
kubectl delete pvc elasticsearch-data-persistent-storage-elasticsearch-data-1 -n monitoring
helm del --purge elasticsearch-master
kubectl delete namespace monitoring
```

## Configuration

### [Elasticsearch Master](elasticsearch-master)

| Parameter                              | Description                                                  |                     Default                     |
| :------------------------------------- | ------------------------------------------------------------ | :---------------------------------------------: |
| `image.repository`                     | Container image repository                                   | `docker.elastic.co/elasticsearch/elasticsearch` |
| `image.tag`                            | Container image tag                                          |                     `7.3.0`                     |
| `image.pullPolicy`                     | Container image pull policy                                  |                 `IfNotPresent`                  |
| `nodeSelector`                         | Node selector                                                |             `used_for: monitoring`              |
| `tolerations`                          | Tolerations                                                  |        `dedicated=monitoring:NoExecute`         |
| `environmentVariables[0].ES_JAVA_OPTS` | Cluster parameters to be added to `ES_JAVA_OPTS` environment variable |              `-Xms1024m -Xmx1024m`              |
| `resources`                            | Pod resource requests & limits                               |                      `{}`                       |
| `xpack.security.enabled`               | Enabling xpack security                                      |                      false                      |
| `xpack.monitoring.collection.enabled`  | Monitoring collections for xpack                             |                      false                      |

### [Elasticsearch Data](elasticsearch-data)

| Parameter                              | Description                                                  | Default                                         |
| -------------------------------------- | ------------------------------------------------------------ | ----------------------------------------------- |
| `image.repository`                     | Container image repository                                   | `docker.elastic.co/elasticsearch/elasticsearch` |
| `image.tag`                            | Container image tag                                          | `7.3.0`                                         |
| `image.pullPolicy`                     | Container image pull policy                                  | `IfNotPresent`                                  |
| `persistence.enabled`                  | Data persistent enabled/disabled                             | true                                            |
| `persistence.size`                     | Data persistent volume size                                  | `50Gi`                                          |
| `data.persistence.accessMode`          | Data persistent Access Mode                                  | `ReadWriteOnce`                                 |
| `persistence.storageClass`             | Data persistent volume Class                                 | `gp2`                                           |
| `nodeSelector`                         | Node selector                                                | `used_for: monitoring`                          |
| `tolerations`                          | Tolerations                                                  | `dedicated=monitoring:NoExecute`                |
| `environmentVariables[0].ES_JAVA_OPTS` | Cluster parameters to be added to `ES_JAVA_OPTS` environment variable | `-Xms1024m -Xmx1024m`                           |
| `resources`                            | pod resource requests & limits                               | `{}`                                            |
| `xpack.security.enabled`               | Enabling xpack security                                      | false                                           |
| `xpack.monitoring.collection.enabled`  | Monitoring collections for xpack                             | false                                           |

### [Elasticsearch Client](elasticsearch-client)

| Parameter                              | Description                                                  |                     Default                     |
| :------------------------------------- | ------------------------------------------------------------ | :---------------------------------------------: |
| `image.repository`                     | Container image repository                                   | `docker.elastic.co/elasticsearch/elasticsearch` |
| `image.tag`                            | Container image tag                                          |                     `7.3.0`                     |
| `image.pullPolicy`                     | Container image pull policy                                  |                 `IfNotPresent`                  |
| `nodeSelector`                         | Node selector                                                |             `used_for: monitoring`              |
| `tolerations`                          | Tolerations                                                  |        `dedicated=monitoring:NoExecute`         |
| `environmentVariables[0].ES_JAVA_OPTS` | Cluster parameters to be added to `ES_JAVA_OPTS` environment variable |              `-Xms1024m -Xmx1024m`              |
| `resources`                            | pod resource requests & limits                               |                      `{}`                       |
| `xpack.security.enabled`               | Enabling xpack security                                      |                      false                      |
| `xpack.monitoring.collection.enabled`  | Monitoring collections for xpack                             |                      false                      |
