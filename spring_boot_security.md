# Spring Boot Security
## Add Dependecy 
```xml
  <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-security</artifactId>
  </dependency>
```

## Basic Authentication (random password)
   Enbale debug from application.properties
   ```xml
   logging.level.org.springframework.security=debug
   ```
   Just run and enter username user
   copy password from consle paste it on password
   
## Basic Authentication Single user
 ```xml
spring.security.user.name=omprakash
spring.security.user.password=1234
```
## Basic Http Authentication Single user Authenticate all Requests

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

## In Memory Authentication multiple user with role based method level security 


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
# Steps to Implement JDBC Authentication using mysql
## Create user amd role classes

```java
import java.util.Set;

import javax.persistence.CascadeType;
import javax.persistence.Entity;
import javax.persistence.FetchType;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import javax.persistence.JoinColumn;
import javax.persistence.JoinTable;
import javax.persistence.ManyToMany;
import javax.persistence.Table;
import javax.persistence.UniqueConstraint;

import lombok.Data;

@Data
@Entity
@Table(name="user" ,uniqueConstraints = {
		@UniqueConstraint(columnNames= {"username"}),
		@UniqueConstraint(columnNames= {"email"})
})
public class User {

	@Id
	@GeneratedValue(strategy = GenerationType.IDENTITY)
	private Long id;

	private String name;
	private String username;
	private String email;
	private String password;

	@ManyToMany(fetch = FetchType.EAGER,cascade = CascadeType.ALL)
	@JoinTable(name = "user_roles",
	     joinColumns=@JoinColumn(name ="user_id",referencedColumnName ="id" ),
	     inverseJoinColumns=@JoinColumn(name ="role_id",referencedColumnName ="id" ))
	private Set<Role> roles;	

}

```

```java
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import javax.persistence.Table;

import lombok.Getter;
import lombok.Setter;

@Getter
@Setter

@Entity
@Table(name ="roles")
public class Role {
	
	@Id
	@GeneratedValue(strategy = GenerationType.IDENTITY)	
	private Long id;
	
	private String name;

}

```

## create UserRepository and RoleRepository

```java
import java.util.Optional;

import org.springframework.data.jpa.repository.JpaRepository;

import com.springboot.blog.entity.User;

public interface UserRepository extends JpaRepository<User, Long> {

	Optional<User> findByEmail(String email);
	Optional<User> findByUsername(String username);
	Optional<User> findByUsernameOrEmail(String email,String username);
	boolean existEmail(String email);
	boolean existUsername(String username);
}

```

```java

import java.util.Optional;

import org.springframework.data.jpa.repository.JpaRepository;

import com.springboot.blog.entity.Role;

public interface RoleRepository extends JpaRepository<Role, Long> {

	Optional<Role> findByName(String name);
}

```
