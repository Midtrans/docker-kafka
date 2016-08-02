# Kafka in Docker

This repository provides everything you need to run Kafka in Docker.

## Why?

The main hurdle of running Kafka in Docker is that it depends on Zookeeper.
Compared to other Kafka Docker images, this one runs both Zookeeper and Kafka
in the same container. This means:

* No dependency on an external Zookeeper host, or linking to another container
* Zookeeper and Kafka are configured to work together out of the box

## Run

First, launch the broker:

```bash
docker run -p 2181:2181 -p 9092:9092 --env ADVERTISED_HOST=HOST_IP_GOES_HERE --env ADVERTISED_PORT=9092 kchristidis/kafka
```

Where you substitute `HOST_IP_GOES_HERE` with [the IP of your host
machine](http://superuser.com/a/1080211), e.g. `192.168.0.5`.

Then, from your localhost, launch a producer and a consumer:

```bash
kafka-console-producer.sh --broker-list localhost:9092 --topic test
```

```bash
kafka-console-consumer.sh --zookeeper localhost:2181 --topic test
```

## Get

* Public build: https://registry.hub.docker.com/u/kchristidis/kafka/
* Build from source: `docker build -t kchristidis/kafka kafka/`

## TODO

* Not particularly optimized for startup time
* Better docs
