# hot-day-offline

## Development machine

### Visual Studio Code
* https://code.visualstudio.com/Download
* You can use default settings during installation
### Git for Windows
* https://git-scm.com/download/win
### Golang 1.15.2
*	https://golang.org/dl/go1.15.2.windows-amd64.msi
*	Don’t install the most recent version of Golang. OneAgent is very picky when it comes to the currently supported version here.
* Dynatrace is usually a 2-3 updates behind, when it comes to supported Go versions, because compatibility needs to get ensured via integration tests first.
* You can use default settings during installation
### JDK 11
* The sample for Hazelcast is configured to build against Java 11. In general OneAgent doesn’t have a requirement regarding the Java version.
* https://www.oracle.com/java/technologies/javase-jdk11-downloads.html
* Specifically regarding OpenTelemetry / OpenTracing the minimum version of Java required depends on the libraries you would like to include
  * OpenTracing Hazelcast Instrumentation (https://github.com/opentracing-contrib/java-hazelcast) e.g. requires at the minimum Java 8
* You can use default settings during installation
### Apache Maven
Maven is being used in order to build the Java application the first part of the hands on focuses on
* https://mirror.klaus-uwe.me/apache/maven/maven-3/3.6.3/binaries/apache-maven-3.6.3-bin.zip

## External Services
### Kafka
* For Lessen 03 an external Kafka Broker is required
* There are a lot of ways to launch a Kafka Broker (AWS Image, Docker Image, …)
* You can also follow pretty much all the steps explained in this tutorial to set it up on a local Ubuntu machine
* https://idroot.us/install-apache-kafka-ubuntu-20-04/
* Just make sure that in the end you’re creating a topic “SomeTopic”, because that’s what the Kafka Sample sends data to (line 15 in kafka.go).
* bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic SomeTopic

### Hazelcast
* Like with Kafka, there are multiple ways to get a Hazelcast instance to run
* https://hazelcast.org/imdg/get-started/
* The easiest way is to install the Hazelcast cluster on a Kubernetes cluster
* You can either use Google Kubenetes Engine, AKS or EKS
* Please install only `ver 3.12.10-3` of the Hazelcast cluster
* Follow the commands below to install HazelCast using Helm Charts
  ```bash
  $ helm repo add hazelcast https://hazelcast-charts.s3.amazonaws.com/
  $ helm repo update
  $ helm install hazelcast --set image.tag="3.12.10-3",service.type=LoadBalancer,service.clusterIP="" hazelcast/hazelcast
  ```
* After a successful installation, extract the public facing IP address using this command
  ```bash
  $ kubectl get svc --namespace default hazelcast -o jsonpath='{.status.loadBalancer.ingress[0].ip}
  ```
* You can also follow these instructions to install Hazelcast manually on Linux
* https://riptutorial.com/hazelcast/example/26416/installation-or-setup

### Dynatrace Cluster version
•	The required OneAgent version for the samples is 1.207.xxx
o	If your Dynatrace Cluster doesn’t yet provide a 207 OneAgent installer you may want to use a free trial tenant (which are coincidentally at 207 at the moment)
o	Free Dynatrace Trial | Dynatrace
•	An earlier version (< 207) will unfortunately not yet be supporting OpenTelemetry
•	OneAgent >= 1.211 will not work without modifications
o	The current Golang project includes OpenTelemetry 0.13
o	OneAgent 1.211 has desupported OpenTelemetry 0.13 and switched to 0.16+
	Reason for that is a severe bug within the 0.13 OT API
o	In order to switch to 0.16 you’d have to change the go.mod file

Installing Kafka on Ubuntu 20.04
* sudo apt update
* sudo apt upgrade
* sudo apt install openjdk-11-jdk
* wget https://downloads.apache.org/kafka/2.6.1/kafka_2.13-2.6.1.tgz
* sudo tar xzf kafka_2.13-2.6.1.tgz
* sudo mv kafka_2.13-2.6.1 /opt/kafka
* sudo vi /etc/systemd/system/zookeeper.service
[Unit]
Description=Apache Zookeeper service
Documentation=http://zookeeper.apache.org
Requires=network.target remote-fs.target
After=network.target remote-fs.target

[Service]
Type=simple
ExecStart=/opt/kafka/bin/zookeeper-server-start.sh /opt/kafka/config/zookeeper.properties
ExecStop=/opt/kafka/bin/zookeeper-server-stop.sh
Restart=on-abnormal

[Install]
WantedBy=multi-user.target
