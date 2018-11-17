# java-SpringRestWithoutSpringBoot-GET -> Spring security xml

![screenshot](https://github.com/ArnaudBaley/java-SpringRestWithoutSpringBoot-GET/blob/Spring_security_basic_auth/Capture.PNG)

## LANCEMENT

[http://localhost:8080/SpringRestWithoutSpringBoot/](http://localhost:8080/SpringRestWithoutSpringBoot/)

[http://localhost:8080/SpringRestWithoutSpringBoot/hello/arnaud](http://localhost:8080/SpringRestWithoutSpringBoot/hello/arnaud)

## CONFIGURATION

-**Pom.xml** : 
```xml
<properties>
	........
	<springsecurity.version>4.2.9.RELEASE</springsecurity.version>
	........
</properties>

........

<!-- Spring Security -->
<dependency>
	<groupId>org.springframework.security</groupId>
	<artifactId>spring-security-web</artifactId>
	<version>${springsecurity.version}</version>
</dependency>
<dependency>
	<groupId>org.springframework.security</groupId>
	<artifactId>spring-security-config</artifactId>
	<version>${springsecurity.version}</version>
</dependency>

........
```

Classe Message.java dans package com.spring.rest.domain :
```java
package com.spring.rest.domain;

public class Message {

	String name;
	String text;

	public Message(String name, String text) {
		this.name = name;
		this.text = text;
	}

	public String getName() {
		return name;
	}

	public String getText() {
		return text;
	}

}
```