FROM java:openjdk-8-jre

ENV DEBIAN_FRONTEND=noninteractive \
    SCALA_VERSION=2.11 \
    KAFKA_VERSION=0.9.0.1 \
    PATH=/opt/kafka/bin:$PATH

ENV KAFKA_BIN_URL=http://apache.mirrors.spacedump.net/kafka/"$KAFKA_VERSION"/kafka_"$SCALA_VERSION"-"$KAFKA_VERSION".tgz \
    KAFKA_TMP_FILE=/tmp/kafka_"$SCALA_VERSION"-"$KAFKA_VERSION".tgz \
    KAFKA_MD5=/tmp/kafka.md5

RUN echo b71e5cbc78165c1ca483279c27402663 *kafka_"$SCALA_VERSION"-"$KAFKA_VERSION".tgz > "$KAFKA_MD5"

# Install Kafka, Zookeeper, and other needed things
RUN apt-get update
RUN apt-get install -y zookeeper wget supervisor dnsutils
RUN rm -rf /var/lib/apt/lists/*
RUN apt-get clean
RUN wget -q "$KAFKA_BIN_URL" -O "$KAFKA_TMP_FILE"
WORKDIR /tmp
RUN md5sum -c "$KAFKA_MD5"
WORKDIR /
RUN tar xfz "$KAFKA_TMP_FILE" -C /opt
RUN mv /opt/kafka_"$SCALA_VERSION"-"$KAFKA_VERSION" /opt/kafka
RUN rm "$KAFKA_TMP_FILE"
RUN mkdir -p /mnt/kafka-logs

ADD scripts/start-kafka.sh scripts/start-zookeeper.sh /usr/bin/

# Supervisor config
ADD supervisor/kafka.conf supervisor/zookeeper.conf /etc/supervisor/conf.d/

# 2181 is zookeeper, 9092 is kafka
EXPOSE 2181 9092

CMD ["supervisord", "-n", "-c", "/etc/supervisor/supervisord.conf"]
