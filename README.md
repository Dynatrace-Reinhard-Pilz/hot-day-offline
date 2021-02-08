# Perform 2021 - OpenTelemetry - Requirements to set up an environment for the Hands On Parts

## Hands On Instructions
The Instructions for the Hands ons are available [here|https://github.com/Dynatrace/perform-2021-hotday/tree/main/Opentelemetry_With_Dynatrace]. Just check out the various `index.md` files. The instructions in there are 100% the same than the ones you have seen within Dynatrace University.
The Source Code for the Hands ons is available (like it was during the sessions) [here|https://github.com/Dynatrace-APAC/vhot2021].

## Development machine

### Git for Windows
* Git is being used to download the source code of the examples to development machine.
* You may, of course also choose to download manually and skip this step
* https://git-scm.com/download/win
### Golang 1.15.2
*	https://golang.org/dl/go1.15.2.windows-amd64.msi
*	Don’t install the most recent version of Golang. OneAgent is very picky when it comes to the currently supported version here.
* Dynatrace is usually a 2-3 updates behind, when it comes to supported Go versions, because compatibility needs to get ensured via integration tests first.
* You can use default settings during installation
### JDK 11
* The sample for Hazelcast is configured to build against Java 11. In general OneAgent doesn’t have a requirement regarding the Java version.
* https://www.oracle.com/java/technologies/javase-jdk11-downloads.html
* You will have to create an Oracle Account in order to be able to download
* Specifically regarding OpenTelemetry / OpenTracing the minimum version of Java required depends on the libraries you would like to include
  * OpenTracing Hazelcast Instrumentation (https://github.com/opentracing-contrib/java-hazelcast) e.g. requires at the minimum Java 8
* You can use default settings during installation
### Apache Maven
Maven is being used in order to build the Java application the first part of the hands on focuses on
* https://mirror.klaus-uwe.me/apache/maven/maven-3/3.6.3/binaries/apache-maven-3.6.3-bin.zip
* Don't forget to add the folder `C:\<maven-installation-folder>\bin` to your `PATH` variable. Otherwise the command `mvn` will not get recognized when entering it within a Terminal.
### Visual Studio Code
* https://code.visualstudio.com/Download
* You can use default settings during installation
* When opening the first `.go` files Visual Studio Code will show a pop up `Do you want to install the recommended extensions for Go?`. It is a good idea to choose `Install` here.
* When opening the first `.go` files Visual Studio Code will show a pop up `The "go-pls" command is not available. Run "go get -v golang.org/x/tools/gopls" to install".`. It is a good idea to choose `Install All` here. Visual Studio Code will then run that command for you.
### Additional utilities
* During the Hands On Training you're required to execute `curl` in order to send requests to the application(s)
* `curl` is getting installed as a side effect when installing `Git for Windows`
* You can also choose to use a Web Browser instead and not to rely on `curl`

## External Services

### Kafka
* For Lessen 03 an external Kafka Broker is required
* There are a lot of ways to launch a Kafka Broker (AWS Image, Docker Image, …)
* You can also follow pretty much all the steps explained in this tutorial to set it up on a local Ubuntu machine
* https://idroot.us/install-apache-kafka-ubuntu-20-04/
* It's also possible to install Kafka on the same Windows machine where Visual Studio is running. In this case the IP Address for the Kafka Broker is simply `127.0.0.1`.
* Download link for the Kafka Binaries: https://downloads.apache.org/kafka/2.6.1/kafka_2.13-2.6.1.tgz
* Just make sure that in the end you’re creating a topic “SomeTopic”, because that’s what the Kafka Sample sends data to (line 15 in `kafka.go`).
  ```bash
  bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic SomeTopic`
  ```
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

* Finally it's also possible to install Hazelcast on the same Windows machine where Visual Studio is running. In this case the IP Address for the Hazelcast Cluster is simply `127.0.0.1`.

### Dynatrace Cluster version
* The required OneAgent version for the samples is 1.207.xxx
  * If your Dynatrace Cluster doesn’t yet provide a 207 OneAgent installer you may want to use a free trial tenant (which are coincidentally at 207 at the moment)
* An earlier version (< 207) will unfortunately not yet be supporting OpenTelemetry
* OneAgent >= 1.211 will not work without modifications. If there is a need to use these samples with a later OneAgent >= 1.211 please reach out to us for assistance.
  * The current Golang project includes OpenTelemetry 0.13
  * OneAgent 1.211 has desupported OpenTelemetry 0.13 and switched to 0.16+
    * Reason for that is a severe bug within the 0.13 OT API
* In order to switch to 0.16 you’d have to change the go.mod file

