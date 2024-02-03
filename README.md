## Table of Content

* [Confluent Cloud for Kafka](#confluentkafka)
* [Kafdrop Documentation](#kafdrop)

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











