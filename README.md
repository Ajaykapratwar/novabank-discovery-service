# NovaBank Discovery Service

The NovaBank Discovery Service is the service registry for the NovaBank microservices platform. It runs a [Spring Cloud Netflix Eureka](https://docs.spring.io/spring-cloud-netflix/reference/spring-cloud-netflix.html#spring-cloud-eureka-server) server so that application services can register themselves and discover the locations of other services at runtime.

## Features

- Eureka service registry
- Eureka dashboard for viewing registered services
- Spring Boot Actuator health and monitoring endpoints
- Maven and Docker-based execution
- Standalone registry configuration; this service does not register with another Eureka server

## Technology stack

- Java 21
- Spring Boot 4.1.1
- Spring Cloud 2025.1.3
- Spring MVC
- Spring Cloud Netflix Eureka Server
- Spring Boot Actuator
- Maven

## Prerequisites

- JDK 21 or later
- Maven 3.9 or later, or the included Maven Wrapper
- Docker (optional, for containerized execution)

## Running locally

Clone the repository and start the application with the Maven Wrapper:

```bash
./mvnw spring-boot:run
```

On Windows, use:

```powershell
.\mvnw.cmd spring-boot:run
```

The server starts at [http://localhost:8761](http://localhost:8761).

### Building and running the JAR

```bash
./mvnw clean package
java -jar target/discoveryservice-0.0.1-SNAPSHOT.jar
```

Use `.\mvnw.cmd` instead of `./mvnw` on Windows.

## Docker

Build the image:

```bash
docker build -t novabank-discovery-service .
```

Run the container:

```bash
docker run --rm -p 8761:8761 novabank-discovery-service
```

The Docker image builds the application with Maven and runs it on Amazon Corretto. Port `8761` must be published to access the registry from the host.

## Endpoints

| Endpoint | Description |
| --- | --- |
| `http://localhost:8761/` | Eureka dashboard |
| `http://localhost:8761/eureka/` | Eureka client registration and discovery endpoint |
| `http://localhost:8761/actuator/health` | Application health status |
| `http://localhost:8761/actuator` | Actuator endpoint index |

## Connecting a service

A Spring Cloud service can use this registry by adding the Eureka Client dependency and configuring the registry URL:

```properties
spring.application.name=orders-service
server.port=8081
eureka.client.service-url.defaultZone=http://localhost:8761/eureka/
```

For a containerized deployment, replace `localhost` with the hostname or service name that resolves to the discovery-service container.

## Configuration

The default configuration is in `src/main/resources/application.properties`:

```properties
spring.application.name=discoveryservice
server.port=8761
eureka.client.register-with-eureka=false
eureka.client.fetch-registry=false
management.endpoints.web.exposure.include=*
```

The server port can be overridden without changing the source:

```bash
./mvnw spring-boot:run -Dspring-boot.run.arguments="--server.port=8762"
```

For production deployments, expose only the Actuator endpoints required by your monitoring and security policies rather than using `*`.

## Testing

Run the test suite with:

```bash
./mvnw test
```

The current test suite verifies that the Spring application context loads successfully.

## Project structure

```text
src/
  main/
    java/com/example/discoveryservice/
      DiscoveryserviceApplication.java
    resources/
      application.properties
  test/
    java/com/example/discoveryservice/
      DiscoveryserviceApplicationTests.java
Dockerfile
pom.xml
```

## License

See [LICENSE](LICENSE) for licensing information.
