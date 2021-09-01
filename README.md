
# DropWizard Demo Application

Welcome to the DropWizard Demo Application !

Hello DIALLOb!


The goal of this repository is to show a tiny application example
to illustrate build pipelines and tools

## How to build it ?

### Requirements

To be built, this application needs:
* JDK 8 (OpenJDK is fine) as SDK
* **Maven 3.3** as build tool

### Get the code

Clone the repository wherever Maven + JDK is
available
* [Maven 3.3](https://maven.apache.org/) installation on "metal" with your JDK
* [A Maven + JDK Docker image](https://hub.docker.com/_/maven/)
* A [Virtual Machine with Maven and JDK](https://atlas.hashicorp.com/antapos/boxes/ubuntu-trusty64-jdk8-maven)

### Build it

From the root of the repository:

* Whole building process is as simple as
```bash
mvn clean install
```
* Just compiling classes:
```bash
mvn clean compile
```
* Running unit tests (implies compiling):
```bash
mvn test
```
* Running Integration tests (implies compiling but NO unit tests):
```bash
mvn verify
```

* Generating the standalone application:
```bash
mvn package
```

* "Installing" application in the local Maven repository:
```bash
mvn install
```

## How to run it ?

### Bare-metal

The [awesome DropWizard framework](http://www.dropwizard.io/1.0.0/docs/) makes the generated application a standalone one.

You can run it with just a Java JRE 8 installed:

```bash
java -jar ./target/demoapp.jar server ./hello-world.yml
```

The application will then be available on http://localhost:8080/ if everything went smoothly

### Docker run

They are a lot of reasons that would make you **NOT**
wanting to launch application in Bare Metal:

* Developer Syndrom (portability concerns): "It works on my machine" (**your** JDK and Maven installation)
* Security concerns: "What is this application doing ?"
* Ease of run: If any application is already running on
the `8080` port of your machine, you'll be required
to dig into the codebase + documentation to change
listening ports.


This is why you can run the application with your
own baked Docker image.

* A `Dockerfile` is provided at the root of the repository
* First step is to "build" the Docker image:
```bash
docker build -t dw-demo-app:latest ./
```
* Then run it in background,
letting Docker selecting a port:
```bash
CID=$(docker run -d -P dw-demo-app:latest)
```
* Fetch the allocated port by asking docker:
  - Note that a simple ```docker ps``` could be sufficient, but not very machine-readable
```bash
docker port ${CID} 8080
```
Example: 0.0.0.0:**37567**

* Browse to the application homepage, using the fetched port: http://{DOCKER SERVICE IP}:**37567**
  - {DOCKER SERVICE IP} is the IP address of your Docker service:
    * `localhost` on Docker4Mac or Docker4Windows
    * The result of ```boot2docker ip``` using Docker toolbox

* Stop and clean the application:
```bash
docker stop ${CID} && docker rm -v ${CID}
```
