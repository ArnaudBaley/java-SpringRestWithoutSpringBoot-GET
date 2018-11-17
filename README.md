# java-SpringRestWithoutSpringBoot-GET -> Spring security xml

![screenshot](https://github.com/ArnaudBaley/java-SpringRestWithoutSpringBoot-GET/blob/Spring_security_basic_auth_xml/Capture.PNG)

## LANCEMENT

[http://localhost:8080/SpringRestWithoutSpringBoot/](http://localhost:8080/SpringRestWithoutSpringBoot/)

[http://localhost:8080/SpringRestWithoutSpringBoot/hello/arnaud](http://localhost:8080/SpringRestWithoutSpringBoot/hello/arnaud)

## CONFIGURATION XML

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

-**web.xml** : 
```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns="http://java.sun.com/xml/ns/javaee"
	xmlns:web="http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"
	xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"
	id="WebApp_ID" version="3.0">

	<display-name>Archetype Created Web Application</display-name>

	<servlet>
		<servlet-name>spring</servlet-name>
		<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
		<load-on-startup>1</load-on-startup>
	</servlet>

	<servlet-mapping>
		<servlet-name>spring</servlet-name>
		<url-pattern>/</url-pattern>
	</servlet-mapping>

	<!-- Spring context files to be loaded -->
	<context-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>
			/WEB-INF/spring-security.xml
		</param-value>
	</context-param>

	<listener>
		<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
	</listener>

	<!-- Spring Security -->
	<filter>
		<filter-name>springSecurityFilterChain</filter-name>
		<filter-class>org.springframework.web.filter.DelegatingFilterProxy</filter-class>
	</filter>
	<filter-mapping>
		<filter-name>springSecurityFilterChain</filter-name>
		<url-pattern>/*</url-pattern>
	</filter-mapping>

</web-app>
```

-**spring-security.xml** : 
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans:beans xmlns="http://www.springframework.org/schema/security"
             xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xmlns:beans="http://www.springframework.org/schema/beans"
             xsi:schemaLocation="http://www.springframework.org/schema/security
                                 http://www.springframework.org/schema/security/spring-security.xsd
                                 http://www.springframework.org/schema/beans
                                 http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!-- <beans:bean id="basicAuthEntryPoint" class="com.memorynotfound.spring.security.config.CustomAuthenticationEntryPoint"/> -->

    <http realm="API">
        <intercept-url pattern="/**" access="isAuthenticated()"/>
        <http-basic/>
        <csrf disabled="true"/>
    </http>

    <authentication-manager>
        <authentication-provider>
            <user-service>
                <user name="user" password="password" authorities="ROLE_USER" />
            </user-service>
        </authentication-provider>
    </authentication-manager>

</beans:beans>
```