---
title: "Apache & Spring Boot 연동하기"
excerpt: "Git"

sidebar:
    nav: "Tools"
categories:
  - Tools
tags:
  - Tools 
classes: wide
last_modified_at: 2021-12-16T08:57:00-21:55:00
---

> 상황을 어떻게 보냐에 따라 그 사람이 처한 상황을 이해할 수 있다. 

***

##### Spring Boot 설정 

```properties

############################ [ START ] Tomcat Setting ############################

tomcat.accesslog=/logs/api/sample/78/
tomcat.ajp.protocol=AJP/1.3
tomcat.ajp.port=20300
tomcat.ajp.enabled=true
tomcat.port=10300
tomcat.ajp.redirectport=8443
tomcat.jvmroute=jvmKi1

############################ [ E N D ] Tomcat Setting ############################

```

```java

@Configuration
public class EmbeddedTomcatContainer {

	@Value("${tomcat.ajp.protocol}")
	String ajpProtocol;
	 
	@Value("${tomcat.ajp.port}")
	int ajpPort;
	
	@Value("${tomcat.port}")
	int tomcatPort;
	 
	@Value("${tomcat.ajp.enabled}")
	boolean isHttpsProtocol;
	
    @Value("${tomcat.jvmroute}")
    private String jvmRoute;
    
    @Value("${tomcat.ajp.redirectport}")
    int redirectport;
    
    @Value("${tomcat.accesslog}")
    private String accessLogPath;

    @PostConstruct
    public void setJvmRoute() {
        // embedded tomcat uses this property to set the jvmRoute
        System.setProperty("jvmRoute", jvmRoute);
    }
    
    @Bean
    public ServletWebServerFactory servletContainer() {
        TomcatServletWebServerFactory tomcat = new TomcatServletWebServerFactory() ;
        
        if(isHttpsProtocol) {
        	tomcat.addAdditionalTomcatConnectors(redirectConnectorForHttps());
        } else {
        	tomcat.setPort(tomcatPort);
        }
       
        return tomcat;
    }

    private Connector redirectConnectorForHttps() {
		Connector connector = new Connector(ajpProtocol);
        connector.setPort(ajpPort);
        connector.setSecure(false);
        connector.setAllowTrace(false);
        ((AbstractAjpProtocol) connector.getProtocolHandler()).setSecretRequired(false);
        connector.setScheme("https");
        connector.setRedirectPort(redirectport);
        return connector;
    }
}

```

```shell

...
2021-12-23 14:29:35 [main] INFO  s.d.s.w.p.DocumentationPluginsBootstrapper - Context refreshed
2021-12-23 14:29:35 [main] INFO  s.d.s.w.p.DocumentationPluginsBootstrapper - Found 5 custom documentation plugin(s)
2021-12-23 14:29:35 [main] INFO  s.d.s.w.s.ApiListingReferenceScanner - Scanning for api listing references
2021-12-23 14:29:35 [main] INFO  s.d.s.w.r.o.CachingOperationNameGenerator - Generating unique operation named: POST_samplePOST_1
2021-12-23 14:29:35 [main] INFO  s.d.s.w.s.ApiListingReferenceScanner - Scanning for api listing references
2021-12-23 14:29:35 [main] INFO  s.d.s.w.s.ApiListingReferenceScanner - Scanning for api listing references
2021-12-23 14:29:35 [main] INFO  s.d.s.w.s.ApiListingReferenceScanner - Scanning for api listing references
2021-12-23 14:29:35 [main] INFO  s.d.s.w.s.ApiListingReferenceScanner - Scanning for api listing references
2021-12-23 14:29:35 [main] INFO  o.a.coyote.http11.Http11NioProtocol - Starting ProtocolHandler ["http-nio-10303"]
2021-12-23 14:29:35 [main] INFO  org.apache.coyote.ajp.AjpNioProtocol - Starting ProtocolHandler ["ajp-nio-127.0.0.1-20303"]
2021-12-23 14:29:35 [main] INFO  o.s.b.w.e.tomcat.TomcatWebServer - Tomcat started on port(s): 10303 (http) 20303 (https) with context path ''
2021-12-23 14:50:17 [SpringContextShutdownHook] INFO  o.s.s.c.ThreadPoolTaskExecutor - Shutting down ExecutorService 'applicationTaskExecutor'

```