# Spring Boot Security
## Add Dependecy 
```xml
  <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-security</artifactId>
  </dependency>
```

## Basic Auth (random password)
   Enbale debug from application.properties
   ```xml
   logging.level.org.springframework.security=debug
   ```
   Just run and enter username user
   copy password from consle paste it on password
   
## Basic Auth Single user
 ```xml
spring.security.user.name=omprakash
spring.security.user.password=1234
```
## Basic Http Auth Single user Authenticate all Requests

```java
package com.springboot.blog.config;

import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;

@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {

	@Override
	public void configure(HttpSecurity http) throws Exception {
		http.csrf().disable()
		    .authorizeRequests()
		    .anyRequest()
		    .authenticated()
		    .and()
		    .httpBasic();
	}
}

```
