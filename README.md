# Apache-Kafka

## _Kafdrop_
- **Kafdrop** is a web UI for viewing Kafka topics and browsing consumer groups. The tool displays information such as brokers, topics, partitions, consumers, and lets you view messages.
- You can run the Kafdrop JAR directly, via Docker, or in Kubernetes.
- Java 17 or newer
- Kafka (version 0.11.0 or newer) or Azure Event Hubs 

## Running from JAR

* First Build the JAR in the terminal:
```sh
mvn clean compile package
```
* Run the JAR file in the terminal:
```sh
java --add-opens=java.base/sun.nio.ch=ALL-UNNAMED \
    -jar target/kafdrop-<version>.jar \
    --kafka.brokerConnect=<host:port,host:port>,...
```
**Example:** `java --add-opens=java.base/sun.nio.ch=ALL-UNNAMED -jar absolute_path\kafdrop-master\target\kafdrop-4.0.1-SNAPSHOT.jar --kafka.brokerConnect=ip:9092`
* If unspecified, `kafka.brokerConnect` defaults to `localhost:9092`.
* Open a browser and navigate to [http://localhost:9000](http://localhost:9000). The port can be overridden by adding the following config:

```
--server.port=<port> --management.server.port=<port>
```

* Optionally, configure a schema registry connection with:
```
--schemaregistry.connect=http://localhost:8081
```
and if you also require basic auth for your schema registry connection you should add:
```
--schemaregistry.auth=username:password
```

**For More Details:** [**Refer Official Kafdrop documentation**](https://github.com/srvcode/Kafka-A2Z-Repo/blob/master/kafdrop-master/README.md) 

**Credit:** A Big Thumbs up👍😌  to **Obsidian Dynamics** for this Wonderful tool. [**Checkout official latest changes & upgradations Here**](https://github.com/obsidiandynamics/kafdrop)











