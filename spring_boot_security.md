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
## In Memoery Authentication multiple user with limited requests

```java

import org.springframework.context.annotation.Configuration;
import org.springframework.http.HttpMethod;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
import org.springframework.security.core.userdetails.User;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.provisioning.InMemoryUserDetailsManager;

@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {

	@Override
	public void configure(HttpSecurity http) throws Exception {
		http.csrf().disable()
		    .authorizeRequests()
		    .antMatchers(HttpMethod.GET,"/api/***").permitAll()
		    .anyRequest()
		    .authenticated()
		    .and()
		    .httpBasic();
	}
	
	@Override
	public UserDetailsService userDetailsService() {
		UserDetails omprakash=User.builder().username("om").password("123").roles("USER").build();
		UserDetails om123=User.builder().username("om123").password("12345").roles("ADMIN").build();
		
		return new InMemoryUserDetailsManager(omprakash,om123);			
	}
}

```

# In Memory Authontication multiple user with role based method level security 


```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.http.HttpMethod;
import org.springframework.security.config.annotation.method.configuration.EnableGlobalMethodSecurity;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
import org.springframework.security.core.userdetails.User;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.crypto.password.PasswordEncoder;
import org.springframework.security.provisioning.InMemoryUserDetailsManager;

@Configuration
@EnableWebSecurity
@EnableGlobalMethodSecurity(prePostEnabled = true)
public class SecurityConfig extends WebSecurityConfigurerAdapter {
	
	@Bean
	public PasswordEncoder passwordEncoder() {
		return new BCryptPasswordEncoder();
	}

	@Override
	public void configure(HttpSecurity http) throws Exception {
		http.csrf().disable()
		    .authorizeRequests()
		    .antMatchers(HttpMethod.GET,"/api/***").permitAll()
		    .anyRequest()
		    .authenticated()
		    .and()
		    .httpBasic();
	}
	
	@Override
	@Bean
	public UserDetailsService userDetailsService() {
		UserDetails omprakash=User.builder().username("om").password(passwordEncoder().encode("123")).roles("USER").build();
		UserDetails om123=User.builder().username("om123").password(passwordEncoder().encode("12345")).roles("ADMIN").build();
		
		return new InMemoryUserDetailsManager(omprakash,om123);			
	}
}

```

add this annotation on controller to implement method level security 

```java
@PreAuthorize("hasRole('ADMIN')")
```
