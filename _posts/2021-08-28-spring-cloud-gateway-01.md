---
title: "Spring Cloud Gateway"
excerpt: ""

categories:
  - Spring
tags:
  - Spring 
classes: wide
---

> 남이 나를 알아주지 않음을 걱정하지말고, 자신의 능하지 못함을 걱정해야 한다. 

### Get Started With Cloud Gateway 

- 기본 pom.xml 설정 

```xml 

<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.5.4</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    <groupId>com.example</groupId>
    <artifactId>spring-cloud-gateway</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>spring-cloud-gateway</name>
    <description>spring-cloud-gateway</description>
    <properties>
        <java.version>11</java.version>
        <spring-cloud.version>2020.0.3</spring-cloud.version>
    </properties>
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-webflux</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-gateway</artifactId>
        </dependency>

        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>io.projectreactor</groupId>
            <artifactId>reactor-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>${spring-cloud.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <configuration>
                    <excludes>
                        <exclude>
                            <groupId>org.projectlombok</groupId>
                            <artifactId>lombok</artifactId>
                        </exclude>
                    </excludes>
                </configuration>
            </plugin>
        </plugins>
    </build>
</project>

```

- 기본 routes 예제 

```java

public static void main(String[] args) {
    SpringApplication.run(SpringCloudGatewayApplication.class, args);
}

@Bean
RouteLocator gateway ( RouteLocatorBuilder rlb ) {
    return rlb
            .routes()
            .route( routeSpec ->
                    routeSpec.path("/hello").and().host("*.spring.io")
                            .filters(gatewayFilterSpec ->
                                    gatewayFilterSpec.setPath("/guides"))
                            .uri("https://spring.io/")
                    )
            .route("twitter", routeSpec ->
                routeSpec.path("/twitter/**")
                        .filters( fs -> fs.rewritePath("/twitter/(?<handle>.*)",
                                "/${handle}"
                                )
                        ).uri("http://twitter.com/@"))

            .build();
}

```

### Spring Cloud Gateway 시작하기 

Spring Cloud Gateway는 Spring Boot 2.X, Spring WebFlux, 그리고 Project Reactor를 기반으로 구성되어 있다. 

- 기본 용어 
  - Route
  - Predicate
  - Filter 


### 어떻게 동작하는가?

![](https://keepinmindsh.github.io/lines/assets/img/spring_cloud_gateway_diagram.png){: .align-center} 

Client는 Spring Cloud Gateway로 요청을 전달하면, Gateway Handler가 만약 설정된 route 정보에 해당 하는 요청일 경우, Gateway Web Handler로 요청을 전달한다. 그리고 이 핸들러는 요청에 특화된 Filter Chain을 통해서 요청을 동작시킨다. 
필터가 점선에 의해서 구분되는 것은 전/후의 로직에 대해서 Filter를 통해서 처리할 수 있다. 모든 "pre" 필터 로직은 실행될 것이다. 
그리고 해당 우회 요청이 생기고, 그 우회 요청이 생깅 후에 "post" 필터의 로직이 동작할 것이다. 

### 간단 예제

- yaml 

```

spring:
  cloud:
    gateway:
      routes:
      - id: after_route
        uri: https://example.org
        predicates:
        - Cookie=mycookie,mycookievalue

```

> https://docs.spring.io/spring-cloud-gateway/docs/current/reference/html/#gateway-starter [2021-08-29]