# zabbix-kafka
Simple Zabbix template to monitor kafka clusters.
This template is strongly based on @helli0n [kafka-monitoring](https://github.com/helli0n/kafka-monitoring), but instead of using a custom discovery script it uses the built-in JMX discovery.
The [JMX item protocol has changed](https://www.zabbix.com/documentation/3.4/manual/installation/upgrade_notes_340#jmx_item_protocol_changes) since version 3.4, this template uses the new syntax.

## Requisite

### Zabbix Java Gateway

Before using this template, you'll need to install and configure [Zabbix Java Gateway](https://www.zabbix.com/documentation/3.4/manual/concepts/java).
You can quickly install it through most packet managers:

```
yum install zabbix-java-gataway
```

or

```
apt-get update && apt-get install zabbix-java-gateway
```

You also need to configure [Zabbix Server](https://www.zabbix.com/documentation/3.4/manual/concepts/java#configuring_server_for_use_with_java_gateway) to communicate with the Java Gateway

### Kafka

You need to enable the [JMX port](https://kafka.apache.org/documentation/#monitoring) on every Kafka node.
You can do so by adding the following string to the `$KAFKA_JMX_OPTS` env variable:

`-Dcom.sun.management.jmxremote.port=$PORT`

Change `$PORT` with the chosen JMX port.

### Zabbix

On Zabbix you simply need to add a JMX interface to each of the Kafka cluster's node.
The JMX port is the same used to bootstrap Kafka on every node.

## Discovery Rules

There are some discovery rules in order to automatically discover some informations.
By default, the metrics related to the *__consumer_offset* topic won't be picked up. You can enable it by removing the `{#JMXTOPIC}` Macro filter in the related discovery rules.

## Consumers, Producers, ...

At the moment, this template is for broker metrics only. 
If you want to include metrics from consumers, procuders, etc etc, you're free to put up a merge request.