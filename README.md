# java-SpringRestWithoutSpringBoot-GET

![screenshot](https://github.com/ArnaudBaley/java-SpringRestWithoutSpringBoot-GET/blob/master/Capture.PNG)


## SOURCES

[Hello world](http://websystique.com/springmvc/spring-4-mvc-rest-service-example-using-restcontroller/)


## IMPORT DANS ECLIPSE

File -> Import -> Maven -> Existing Maven Projects


## LANCEMENT

[http://localhost:8080/SpringRestWithoutSpringBoot/](http://localhost:8080/SpringRestWithoutSpringBoot/)

[http://localhost:8080/SpringRestWithoutSpringBoot/hello/arnaud](http://localhost:8080/SpringRestWithoutSpringBoot/hello/arnaud)


## FROM SCRATCH


### Installation 

-Maven -> New "maven-archetype-webap"
	
	-Group Id = com.spring.rest
	
	-Artefact Id = SpringRestWithoutSpringBoot

-Créer nouveaux dossiers suivants : 

	-"java" à la base de src/main/

		-"domain" à la base de src/main/java

-Pom.xml : 
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<groupId>com.spring.rest</groupId>
	<artifactId>SpringRestWithoutSpringBoot</artifactId>
	<packaging>war</packaging>
	<version>0.0.1-SNAPSHOT</version>
	<name>SpringRestWithoutSpringBoot Maven Webapp</name>
	<url>http://maven.apache.org</url>

	<properties>
		<springframework.version>4.3.0.RELEASE</springframework.version>
		<jackson.library>2.7.5</jackson.library>
	</properties>

	<dependencies>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-core</artifactId>
			<version>${springframework.version}</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-web</artifactId>
			<version>${springframework.version}</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-webmvc</artifactId>
			<version>${springframework.version}</version>
		</dependency>

		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>javax.servlet-api</artifactId>
			<version>3.1.0</version>
		</dependency>
		<dependency>
			<groupId>com.fasterxml.jackson.core</groupId>
			<artifactId>jackson-databind</artifactId>
			<version>${jackson.library}</version>
		</dependency>
	</dependencies>

	<build>
		<pluginManagement>
			<plugins>
				<plugin>
					<groupId>org.apache.maven.plugins</groupId>
					<artifactId>maven-compiler-plugin</artifactId>
					<version>3.2</version>
					<configuration>
						<!-- Java version -->
						<source>1.8</source>
						<target>1.8</target>
					</configuration>
				</plugin>
				<plugin>
					<groupId>org.apache.maven.plugins</groupId>
					<artifactId>maven-war-plugin</artifactId>
					<version>2.4</version>
					<configuration>
						<warSourceDirectory>src/main/webapp</warSourceDirectory>
						<warName>SpringRestWithoutSpringBoot</warName>
						<failOnMissingWebXml>false</failOnMissingWebXml>
					</configuration>
				</plugin>
			</plugins>
		</pluginManagement>

		<finalName>SpringRestWithoutSpringBoot</finalName>
	</build>

</project>
```


### Dev

#### Pojo/domain

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

#### Controller

Classe HelloWorldRestController.java dans package com.spring.rest.controller :
```java
package com.spring.rest.controller;

import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import com.spring.rest.domain.Message;

@RestController
public class HelloWorldRestController {

	@RequestMapping("/")
	public String welcome() {// Welcome page, non-rest
		return "Welcome to RestTemplate Example.";
	}

	@RequestMapping("/hello/{player}")
	public Message message(@PathVariable String player) {// REST Endpoint.

		Message msg = new Message(player, "Hello " + player);
		return msg;
	}
}
```


#### Java Configuration

Classe HelloWorldConfiguration pour : 

-component scanning

-annotation support

Classe HelloWorldConfiguration.java dans package com.spring.rest.configuration :
```java
package com.spring.rest.configuration;

import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.EnableWebMvc;

@Configuration
@EnableWebMvc
@ComponentScan(basePackages = "com.spring.rest")
public class HelloWorldConfiguration {

}
```


Classe HelloWorldInitializer pour :

-remplace config spring dans web.xml

-chargée au démarrage du container Srpring

Classe HelloWorldInitializer.java dans package com.spring.rest.configuration :
```java
package com.spring.rest.configuration;

import org.springframework.web.servlet.support.AbstractAnnotationConfigDispatcherServletInitializer;

public class HelloWorldInitializer extends AbstractAnnotationConfigDispatcherServletInitializer {

	@Override
	protected Class<?>[] getRootConfigClasses() {
		return new Class[] { HelloWorldConfiguration.class };
	}

	@Override
	protected Class<?>[] getServletConfigClasses() {
		return null;
	}

	@Override
	protected String[] getServletMappings() {
		return new String[] { "/" };
	}

}
```

#### XML Configuration (instead of Java. branche Config_xml)

src/main/webapp/WEB-INF/**web.xml** référence le DispatcherServlet.
```xml
<!DOCTYPE web-app PUBLIC
 "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
 "http://java.sun.com/dtd/web-app_2_3.dtd" >
 
<web-app>
  <display-name>Archetype Created Web Application</display-name>
   
  <servlet>
        <servlet-name>spring</servlet-name>
            <servlet-class>
                org.springframework.web.servlet.DispatcherServlet
            </servlet-class>
        <load-on-startup>1</load-on-startup>
    </servlet>
 
    <servlet-mapping>
        <servlet-name>spring</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
     
</web-app>
```

src/main/webapp/WEB-INF/**spring-servlet.xml** :
-Référence les librairies Sring.
-Spécifie où les composants sont présents
-Active les annotations MVC
```xml
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"
    xmlns:mvc="http://www.springframework.org/schema/mvc"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd
        http://www.springframework.org/schema/mvc
        http://www.springframework.org/schema/mvc/spring-mvc.xsd">
 
    <context:component-scan base-package="com.spring.rest" />
    <mvc:annotation-driven />
 
</beans>
```