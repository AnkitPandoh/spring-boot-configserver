# Sample Spring Boot Config Server

## config-server

**application.properties**

```
spring.cloud.config.server.git.uri={HOME}/config 
```
 
 Go to your local system directory ```{HOME}/config ``` and run following commands:

```
 git init
 echo message: Default Hello World > application.yml
 echo message: Hello World from Dev > config-client-dev.yml
 echo message: Hello World from Prod > config-client-prod.yml
 git add .
 git commit -m 'adding properties'
```

**Run**

```
curl http://localhost:8888
```

## config-client

**bootstrap.properties**

```
spring.application.name=config-client
spring.profiles.active=dev
spring.cloud.config.uri=http://localhost:8888

server.port=9999
```

**Run**

```
curl http://localhost:9999/message
```

In order to reflect properties change dynamically without restarting the client use ``` @RefreshScope ```. Following changes are required:

**pom.xml**

```
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

**application-properties**

```
management.endpoints.web.exposure.include=*
```
After updating the properties, use below command :

```
curl http://localhost:9999/actuator/refresh -d {} -H "Content-Type: application/json"
```

**Test**

```
curl http://localhost:9999/message
```




 
 
