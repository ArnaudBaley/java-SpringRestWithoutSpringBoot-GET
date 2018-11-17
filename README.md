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

Classe **SecurityWebApplicationInitializer.java** dans package com.spring.rest.security :
```java
package com.spring.rest.security;

import org.springframework.security.web.context.AbstractSecurityWebApplicationInitializer;

public class SecurityWebApplicationInitializer
extends AbstractSecurityWebApplicationInitializer {
	
}
```

Classe **SpringSecurityConfig.java** dans package com.spring.rest.security :
```java
package com.spring.rest.security;

import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;

@Configuration
@EnableWebSecurity
public class SpringSecurityConfig 
  extends WebSecurityConfigurerAdapter {
 
    @Override
    protected void configure(AuthenticationManagerBuilder auth)
      throws Exception {
        auth.inMemoryAuthentication()
          .withUser("user").password("password").roles("USER");
    }
    
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
          .csrf().disable()
          .authorizeRequests()
          .antMatchers("/**")
          .authenticated()
          .and()
          .formLogin();
    }  
}
```