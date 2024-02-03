## Table of Content

* [**Confluent Cloud for Kafka**](#confluentkafka)
* [**Kafdrop Documentation**](#kafdrop)
* [**Steps to run Kafka Connect locally**](#runningkafkalocally)
*******************************************************************************************************************************************************************
## _ConfluentKafka_

Steps to configure Confluent Kafka using Confluent CLI

1. Signup/Login to https://confluent.cloud/login
2. You can navigate & use all Kafka functionality either using the Web UI or using Confluent CLI
3. [Download official Confluent Kafka & follow the steps](https://docs.confluent.io/platform/current/installation/installing_cp/zip-tar.html)
4. If Confluent CLI is not present in Confluent kafka, [Then Download Confluent CLI Here & follow the steps.](https://docs.confluent.io/platform/current/installation/installing_cp/zip-tar.html)
5. [You can configure User Account Here](https://docs.confluent.io/cloud/current/access-management/identity/user-accounts.html)
6. If Required, [You can configure Service Account & ACLs Here](https://docs.confluent.io/cloud/current/access-management/identity/service-accounts.html)
7. [You can refer Confluent CLI commands here.](https://docs.confluent.io/confluent-cli/current/command-reference/overview.html)
8. [You can refer Schema Registry here](https://docs.confluent.io/platform/current/schema-registry/index.html)

#### Environment Setup
|Name                        |Description
|-------------------------------------------------|---------------------------------------------------------------------
|`export CONFLUENT_HOME=/opt/testConfluent`       | Setting this is required to run kafka locally.
|`export PATH=$PATH:$CONFLUENT_HOME/bin`          | Setting this is required to run all bin commands from anywhere.

#### Confluent CLI Imp. Commands
|Name                        |Description
|-------------------------------------------------------------|---------------------------------------------------------------
|`confluent env list`                                         |Lists down all the env available.
|`confluent env use <env_name>`                               |Select the environment you want to use.
|`confluent kafka cluster list`                               |Lists down all the cluster available in the env.
|`confluent kafka cluster use <clusterId>`                    |Select the cluster you want to use.
|`confluent shell`                                            |You will enter the shell where you all commands will be prefixed with confluent. It gives command hints as well.
|`confluent api-key list`                                     |Lists down all the api-keys available.
|`confluent api-key store <apiKey> <apiSecret>`               |Storing the key values locally.
|`confluent api-key use <apiKey>`                             |Select the api key you want to use.
|`confluent iam user list`                                    |Lists down all the users.
|`confluent iam service-account list`                         |Lists down all the service account. (service-account/sa)
|`confluent kafka acl list --cluster <clusterId>`             |List down all the ACLs assigned to the particular user.
|`confluent iam sa use <sa-id>`                               |Select the service account to use.
|`confluent iam service-account create <userName> --description <desc>`       |Create a new service account with username & description.

* Confluent Command to run producer from CLI:
```sh
confluent kafka topic produce <topicName> --schema <fileName.avsc> --value-format avro \
	--schema-registry-endpoint <schema-registry-endpoint> --schema-registry-api-key <schema-registry-api-key> \
	--schema-registry-api-secret <schema-registry-api-secret>
```
* Confluent Command to run consumer from CLI:
```sh
confluent kafka topic consume <topicName> -b --value-format avro --schema-registry-endpoint <schema-registry-endpoint> \
	--schema-registry-api-key <schema-registry-api-key> --schema-registry-api-secret <schema-registry-api-secret>
```
*******************************************************************************************************************************************************************
## _Kafdrop_
- **Kafdrop** is a web UI for viewing Kafka topics and browsing consumer groups. The tool displays information such as brokers, topics, partitions, consumers, and lets you view messages.
- You can run the Kafdrop JAR directly, via Docker, or in Kubernetes.
- Java 17 or newer
- Kafka (version 0.11.0 or newer) or Azure Event Hubs 

## Running from JAR

* First Build the JAR in the terminal:
```sh
mvn clean compile package -DskipTest=true
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

*******************************************************************************************************************************************************************
## _RunningKafkaLocally_

Steps to configure Kafka Connect locally
1. Download Kafka connect from the [official confluent Repo Github](https://github.com/confluentinc)
2. [kafka-connect-http official](https://github.com/castorm/kafka-connect-http/tree/master)
3. Remove not required dependencies to resolve all errors
4. Create a config file & add connect-standalone.properties with below configuration. Refer [config/kafka-connect-http connect-standalone.properties](https://github.com/srvcode/Kafka-A2Z-Repo/blob/master/kafka-connect-http/config/connect-standalone.properties)
```sh
bootstrap.servers=ip:9092

group.id=connect-cluster

key.converter=org.apache.kafka.connect.json.JsonConverter
key.converter.schema.registry.url=http://localhost:8081
value.converter=org.apache.kafka.connect.json.JsonConverter
value.converter.schema.registry.url=http://localhost:8081
key.converter.schemas.enable=false
value.converter.schemas.enable=false

config.storage.topic=_connect-configs
offset.storage.topic=_connect-offsets
status.storage.topic=_connect-statuses

config.storage.replication.factor=1
offset.storage.replication.factor=1
status.storage.replication.factor=1

internal.key.converter=org.apache.kafka.connect.json.JsonConverter
internal.value.converter=org.apache.kafka.connect.json.JsonConverter
internal.key.converter.schemas.enable=false
internal.value.converter.schemas.enable=false

plugin.path=/absolute_path or relative/kafka-connect-http/target
```

6. To run the kafka connect locally. Give the connect-standalone.properties file path to the arguments & main class as
```
org.apache.kafka.connect.cli.ConnectDistributed
```
![image](https://github.com/srvcode/Kafka-A2Z-Repo/assets/74100226/f09a026e-bd94-47dd-af45-0b6e7a93eda5)

7. [Refer Source Configuration to create the connector](https://github.com/srvcode/Kafka-A2Z-Repo/tree/master/kafka-connect-http/example)
* POST `localhost:8083/connectors`
```
{
  "name": "SourceConnector1",
  "config": {
    "connector.class": "com.github.castorm.kafka.connect.http.HttpSourceConnector",
    "http.request.url": "https://5577d429-5de3-4242-a281-9a141cb55f34.mock.pstmn.io/rest/api",
    "http.request.params": "updated=${offset.timestamp}",
    "http.request.headers": "Accept: application/json",
    "http.response.parser": "com.github.castorm.kafka.connect.http.response.KvHttpResponseParser",
    "http.auth.type": "Basic",
    "http.auth.user": "user",
    "http.auth.password": "password",
    "http.offset.initial": "key=TICKT-0001, timestamp=2020-01-01T00:00:01Z",
    "http.response.list.pointer": "/issues",
    "http.response.record.offset.pointer": "key=/key, timestamp=/fields/updated",
    "kafka.topic": "topic-name",
    "http.timer.interval.millis": "0",
    "http.timer.catchup.interval.millis": "0",
    "http.client.read.timeout.millis": "0"
  }
}
```
* Connection Plugins (GET) `localhost:8083/connector-plugins`
* Get Active Connectors (GET) `localhost:8083/connectors`
* Delete Connector (DELETE) `localhost:8083/connectors/<Connector_Name>`





