## Setup

- Install Kafka service, into Kubernetes cluster for this example by using Helm:

```sh
helm repo add confluentinc https://confluentinc.github.io/cp-helm-charts/
helm repo update
kubectl create ns kafka
helm install kafka confluentinc/cp-helm-charts -n kafka \
		--set cp-schema-registry.enabled=false \
		--set cp-kafka-rest.enabled=false \
		--set cp-kafka-connect.enabled=false
```

- Deploy the Kafka client and wait until it’s ready.

```sh
kubectl apply -n kafka -f manifests/kafka-client.yaml
kubectl wait -n kafka --for=condition=ready pod kafka-client --timeout=120s
```

- Create a Kafka topic - `metric` which is used in this example.

```sh
kubectl -n kafka exec -it kafka-client -- kafka-topics \
	--zookeeper kafka-cp-zookeeper-headless:2181 \
	--topic metric \
	--create \
	--partitions 10 \
	--replication-factor 3 \
	--if-not-exists
```

## Deploy

## Troubleshooting

## Cleanup

- Delete Kafka topic.

```sh
kubectl -n kafka exec -it kafka-client -- kafka-topics \
	--zookeeper kafka-cp-zookeeper-headless:2181 \
	--delete \
	--topic metric
```