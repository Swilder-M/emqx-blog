## Introduction to Apache Pulsar

Apache Pulsar is a cloud-native, distributed messaging and streaming platform. Originally developed by Yahoo, Pulsar is under the stewardship of the [Apache Software Foundation](https://www.apache.org/). It features multi-tenant, low latency, read-write separation, cross-regional replication, fast scalability, and flexible fault tolerance, regarded as the best solution for real-time message flow transmission, storage, and computing in the cloud-native era. 

## Advantages of Integrating MQTT with Apache Pulsar for IoT Data Processing

In IoT applications, data generated by devices is usually transmitted through the lightweight [MQTT protocol](https://www.emqx.com/en/blog/the-easiest-guide-to-getting-started-with-mqtt). Integrating MQTT with Apache Pulsar can facilitate real-time processing, storage, and analysis of IoT data.

[EMQX](https://www.emqx.com/en/products/emqx) is a highly scalable and feature-rich [MQTT broker](https://www.emqx.com/en/blog/the-ultimate-guide-to-mqtt-broker-comparison) designed for IoT and real-time messaging applications. With its robust built-in rule engine and data integration capabilities, EMQX can seamlessly integrate IoT data with various backend databases and analytics tools. This provides a broader range of application scenarios for IoT applications, enriching and diversifying interactions between devices and business systems.

Integrating EMQX with Apache Pulsar can bring the following advantages : 

1. **Partition Selection Capability**: Pulsar provides different partitioning strategies to write messages from the same [MQTT client](https://www.emqx.com/en/blog/mqtt-client-tools), topic, or user into the same partition, facilitating subsequent data tracking and processing. 
2. **High Efficiency with Batch Processing**: When batch mode is enabled, EMQX can write multiple pieces of data into Pulsar at the same time. With batch mode enabled, EMQX will temporarily store each piece of data and then write the entire batch of temporarily stored data into Pulsar after a certain amount of time has passed or after a certain number of data pieces have accumulated. This significantly improves write efficiency.
3. **MQTT Message Conversion**: The EMQX rule engine allows users to filter and process MQTT messages from the device side, reducing data redundancy and simplifying processing.
4. **Higher Throughput and Low Latency**: Pulsar is a distributed messaging system designed to achieve high throughput and low latency, making it highly practical for real-time data processing and analysis scenarios. 
5. **Memory Overload Protection**: EMQX offers various message caching modes. When the caching mode is set to memory or hybrid, it includes a system overload protection mechanism. Under high memory pressure, the system discards old messages from the queue to slow down memory growth. This helps prevent system instability due to excessive memory usage and ensures system reliability.

## Workflow of Transferring MQTT Data to Apache Pulsar

Apache Pulsar data integration is an out-of-box feature of EMQX that connects MQTT-based IoT data with Pulsar's data processing capabilities. It combines device access and message transmission with real-time storage and analysis. With simple configuration, MQTT data can be seamlessly integrated. The built-in rule engine simplifies data transmission and processing between the two platforms without complex encoding.

The diagram below illustrates a typical architecture of data integration between EMQX and Apache Pulsar: 

![diagram](https://assets.emqx.com/images/ef6adc06828a3980fb6f152bc54cdb1a.png)

EMQX forwards MQTT data to Apache Pulsar through the rule engine and Sink, and the complete process is as follows:

1. **Message Publication and Reception**: IoT devices establish successful connections through the MQTT protocol and subsequently publish telemetry and status data to specific topics. When EMQX receives these messages, it initiates the matching process within its rules engine.
2. **Message Processing with Rule Engine**: With the built-in rule engine, MQTT messages from specific sources can be processed based on topic matching. The rule engine matches corresponding rules and processes messages, which can include data format conversion, filtering specific information, or enriching messages with context information.
3. **Data Streaming into Apache Pulsar**: The rule triggers the action of forwarding messages to Pulsar, where data can be easily configured to Pulsar message key and value. [MQTT topics](https://www.emqx.com/en/blog/advanced-features-of-mqtt-topics) can also be mapped to Pulsar topics for better data organization and identification, facilitating subsequent data processing and analysis.

After MQTT message data is written to Apache Pulsar, you can engage in flexible application development, such as:

- **Write Pulsar consumer applications to subscribe and process these messages.** You can associate, aggregate, or transform MQTT data with other data sources according to business needs, achieving real-time data synchronization and integration.
- **Use Pulsar's rule engine component to trigger corresponding actions or events** upon receiving specific MQTT messages, enabling cross-system and application event-driven functionality.
- **Analyze MQTT data streams in real-time within Pulsar**, detect anomalies or specific event patterns, and trigger alert notifications or perform corresponding actions based on these conditions.
- **Centralize data from multiple MQTT topics into a unified data stream** and utilize Pulsar's computational capabilities for real-time aggregation, calculation, and analysis to gain more comprehensive data insights.

## Demo: Writing MQTT Data to Pulsar with EMQX

This section will guide you to create a Pulsar server and theme, as well as the Sink and corresponding rules for Pulsar producers in EMQX, to complete the data writing.

### Install Pulsar Server

You can choose Docker to install Pulsar.

```shell
docker run --rm -it -p 6650:6650 --name pulsar apachepulsar/pulsar:2.11.0 bin/pulsar standalone -nfw -nss
```

### Create a Topic for Pulsar

Create topic `testTopic1` with 5 partitions.

```shell
./bin/pulsar-admin topics create-partitioned-topic -p 5 testTopic1
```

### Create Pulsar Sink

Create connectors and actions through the EMQX dashboard. Configure some necessary parameters, such as Servers, Authentication, Pulsar Topic Name, and Partition Strategy.

![Create Pulsar Sink](https://assets.emqx.com/images/b583b1e3a6b408010f653934a182f9c8.png)

![Create Pulsar Sink 2](https://assets.emqx.com/images/36765ca3f51998b46c54c4db0da78060.png)

Please refer to the documentation for specific operating steps: [Stream MQTT Data into Apache Pulsar | EMQX Enterprise Docs](https://docs.emqx.com/en/enterprise/latest/data-integration/data-bridge-pulsar.html) 

### Forward MQTT Messages to Pulsar

#### Create Rules Corresponding to Process Messages

Create a rule in the Dashboard to process messages from the source MQTT topic `"t/#"`, and write the processed results to Pulsar's topic `"testTopic1"` through the configured Sink.

```sql
SELECT
  *
FROM
  "t/#"
```

#### Pulsar Reads MQTT Messages

Activate Pulsar consumers and wait for messages from the “testTopic1” topic to be consumed.

```shell
./bin/pulsar-client consume persistent://public/default/testTopic1  -s "first-subscription" -n 1000
```

#### Send Message

Send a message using [MQTT client](https://www.emqx.com/en/blog/mqtt-client-tools) with the payload `"hello"`.

```shell
lina@linadeMacBook-Pro ~ % mqttx pub -h 10.42.6.48  -t "t/1" -q 2 -m "hello"
[2024-5-27] [16:51:41] › …  Connecting...
[2024-5-27] [16:51:41] › ✔  Connected
[2024-5-27] [16:51:41] › …  Message publishing...
[2024-5-27] [16:51:41] › ✔  Message published
```

#### View the Data Received by Pulsar

You can see that the topic `testTopic1` of pulsar writes one piece of data, with `"event":"message.publish"` and `"payload":"hello"`.

```shell
----- got message -----
key:[mqttx_54d5e3db], properties:[], content:{"publish_received_at":1716799908792,"pub_props":{"User-Property":{}},"peerhost":"10.42.0.0","qos":2,"topic":"t/1","clientid":"mqttx_54d5e3db","payload":"hello","username":"undefined","event":"message.publish","metadata":{"rule_id":"rule_i2f8"},"timestamp":1716799908792,"node":"emqx@10.42.6.48","id":"0006196BA0B6C7A8CC0D0000C02D0002","flags":{"retain":false,"dup":false}}
```

#### View Sink Operation Statistics

We can see that both Matched and Success are displayed as 1

![Matched and Success](https://assets.emqx.com/images/57999e26f43af0554d688fb2e2e293d9.png)

You can also modify this rule later to add custom processing using EMQX's [built-in SQL functions](https://docs.emqx.com/en/enterprise/v5.4/data-integration/rule-sql-builtin-functions.html), like:

```sql
SELECT
  upper(payload),clientid
FROM
  "t/#"
```

Send a message using [MQTT client](https://www.emqx.com/en/blog/mqtt-client-tools) with the payload `"hello"`

```shell
lina@linadeMacBook-Pro ~ % mqttx pub -h 10.42.6.48  -t "t/1" -q 2 -m "hello"
[2024-5-27] [17:11:39] › …  Connecting...
[2024-5-27] [17:11:39] › ✔  Connected
[2024-5-27] [17:11:39] › …  Message publishing...
[2024-5-27] [17:11:39] › ✔  Message published
```

Upon reviewing the Pulsar data again, it was found that the payload was converted to uppercase `"HELLO"`.

```
key:[mqttx_de315a6d], properties:[], content:{"clientid":"mqttx_de315a6d","_v_fun_{var,<<\"upper\">>}_1716800277274093878":"HELLO"}
```

### Forward Client Online and Offline Events to Pulsar

#### Create Rules Corresponding to Client Online and Offline Events

Create rules using the following SQL:

```sql
SELECT
  *
FROM
  "$events/client_connected",
  "$events/client_disconnected"
```

#### Pulsar Reads MQTT Client Online and Offline Event Messages

Activate Pulsar consumers and wait for messages from the `testTopic1` topic to be consumed.

```shell
./bin/pulsar-client consume persistent://public/default/testTopic1  -s "first-subscription" -n 1000
```

#### Trigger Online and Offline Events

Send a message using [MQTT client](https://www.emqx.com/en/blog/mqtt-client-tools) to simulate the online and offline events.

```shell
lina@linadeMacBook-Pro ~ % mqttx pub -h 10.42.6.48  -t "t/1" -q 2 -m "hello"
[2024-5-27] [16:24:29] › …  Connecting...
[2024-5-27] [16:24:29] › ✔  Connected
[2024-5-27] [16:24:29] › …  Message publishing...
[2024-5-27] [16:24:29] › ✔  Message published
```

#### View the Data Received by Pulsar

You can see that the topic `testTopic1` of Pulsar writes two pieces of data, with event types of `"client.connected"` and `"client.disconnected"`.

```shell
----- got message -----
key:[mqttx_a90a8203], properties:[], content:{"receive_maximum":32,"conn_props":{"Request-Problem-Information":1,"User-Property":{}},"expiry_interval":0,"clean_start":true,"mountpoint":"undefined","is_bridge":false,"connected_at":1716798277258,"proto_ver":5,"proto_name":"MQTT","clientid":"mqttx_a90a8203","username":"undefined","event":"client.connected","metadata":{"rule_id":"rule_i2f8"},"keepalive":30,"sockname":"10.42.6.48:1883","peername":"10.42.0.0:37971","timestamp":1716798277258,"node":"emqx@10.42.6.48"}
----- got message -----
key:[mqttx_a90a8203], properties:[], content:{"disconnected_at":1716798277371,"disconn_props":{"User-Property":{}},"proto_ver":5,"proto_name":"MQTT","clientid":"mqttx_a90a8203","username":"undefined","event":"client.disconnected","metadata":{"rule_id":"rule_i2f8"},"sockname":"10.42.6.48:1883","peername":"10.42.0.0:37971","timestamp":1716798277372,"reason":"normal","node":"emqx@10.42.6.48"}
```

#### View Sink Operation Statistics

We can see that both Matched and Success are displayed as 2

![View Sink Operation Statistics](https://assets.emqx.com/images/884ba30c1673be87866128435be50b73.png)

## Conclusion

In this comprehensive guide, we delved into the intricacies of leveraging EMQX for seamless integration with Apache Pulsar for efficient data transmission and processing. By integrating EMQX and Apache Pulsar, users can harness robust capabilities to ensure reliable data handling in high-throughput environments, maximizing the potential of both.



<section class="promotion">
    <div>
        Talk to an Expert
    </div>
    <a href="https://www.emqx.com/en/contact?product=solutions" class="button is-gradient">Contact Us →</a>
</section>
