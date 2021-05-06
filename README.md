# Strimzi Office Hours demo from KubeCon Europe 2021

This repository contains the simple demo used during Strimzi Office Hours @ KubeCon Europe 2021.

## Create namespace `myproject`

```
kubectl create namespace myproject
```

## Deploy Strimzi Cluster Operator (0.22.1)

```
kubectl create -f 01-install/ -n myproject
```

## Deploy the Kafka cluster and wait for it to be ready

```
kubectl apply -f 02-kafka.yaml -n myproject
kubectl wait kafka/my-cluster --for=condition=Ready --timeout=300s -n myproject
```

## Deploy the example consumer and producer

```
kubectl apply -f 03-clients.yaml -n myproject
```

You can check that the consumer starts receiving messages using:

```
kubectl logs deployment/kafka-consumer -f -n myproject
```

## Deploy Kafka Connect

Edit the `04-kafka-connect-build.yaml` file and:
* Update the `kafkaconnectbuild-pull-secret` secret to contain your `.dockerconfigjson` for pushing the image (line 18)
* Update the image address in `KafkaConnect.spec.build.output.image` to match your own container registry (line 64)

```
kubectl apply -f 04-kafka-connect-build.yaml -n myproject
kubectl wait kafkaconnect/my-connect --for=condition=Ready --timeout=300s -n myproject
```

You can check that the consumer starts receiving messages using:

```
kubectl logs deployment/my-connect-connect -f -n myproject
```

## Issues? Questions?

Join the #strimzi channel on CNCF Slack if you have any questions or issues 😉.
