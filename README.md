# kafk8s-replicator-helm-chart
Replicates kafkas using k8s


## Overview
This Helm Chart uses [strimzi kafka operator](https://github.com/strimzi/strimzi-kafka-operator) to deploy a [MirrorMaker2](https://strimzi.io/docs/operators/latest/using.html#proc-mirrormaker-replication-deployment-configuration-kafka-mirror-maker) instance on Kubernetes.

| Helm v3 compatible only | 
| ---| 

| target topics will be replicated with `source.` prefix | 
| ---| 

## Usage
Create a values.yaml file:
```yaml
sourceBootstrapServers:
  - my-source-kafka:9092
targetBootstrapServers:
  - my-target-kafka:9092

syncTopicAcls: false
```

and use it in helm install:
```bash
git clone https://github.com/Cobliteam/kafk8s-replicator-helm-chart
cd kafk8s-replicator-helm-chart
git submodule update --init --recursive
helm install -f path-to-you-values.yaml kafk8s-replicator .
```

## Helm possible values
| Variable |Required |Type |Restrictions |Default Value |Description |
| -------- |-------- |---- |------------ |------------- |----------- |
| sourceBootstrapServers[ ] |true |array | | |Souce kafka servers |
| targetBootstrapServers[ ] |true |array | | |Target kafka servers |
| mirrorMakerEnv |false |object |keys: ^[A-Za-z_][A-Za-z0-9_.-]*$  | |Environment variables to be send to MirrorMaker2 image |
| replicaCount |false |integer | |1 |Number of MirrorMaker2 replicas |
| customImage.repository |false |string | | |Image repository |
| customImage.tag |false |string | | |Image tag |
| syncTopicAcls |false |boolean | |true |Sync topics ACLs |
| kafkaConnectVersion |false |string | |2.5.0 |Kafka Connect version to use |
| resources.limits.cpu |false |string | | |CPU limits |
| resources.limits.memory |false |string | | |Memory limits |
| resources.requests.cpu |false |string | | |CPU requests |
| resources.requests.memory |false |string | | |Memory requests |
| enablePrometheusJMXExporter |false |string | | |Enable/disable [JMX to Prometheus exporter](https://github.com/prometheus/jmx_exporter). Container generate default openmetrics at http://localhost:9404 |
| replicationPolicyClass |false |string | | |Policy class to be used to generate target topic name |
| strimzi-kafka-operator.enabled |false |boolean | |true |Enable/disable operator installation |

## Caveats
Helm 3 version of strimzi is in release condidate. In order to use is as a dependency, we are linking projects through git submodules.

We are waiting for https://github.com/strimzi/strimzi-kafka-operator/issues/2546 resolution to provide a way to replicate topics without prefixes.
