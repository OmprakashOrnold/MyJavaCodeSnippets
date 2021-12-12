# Spring MVC CRUD Application with Hibernate + JSP + MySQL + Maven + Eclipse

## Development Steps

1.   Create a Maven Web Application
2.   Add Dependencies JSTL, MySQL,JSP Spring web in pom.xml File
3.   Project Structure
4.   AppInitializer - Register a DispatcherServlet using Java-based Spring configuration
5.   AppContext - Spring and Hibernate Integration using Java-based Spring configuration
6.   WebMvcConfig - Spring MVC Bean Configuration using Java-based Spring configuration
7.   JPA Entity - Customer.java
8.   Spring MVC Controller Class - CustomerController.java
9.   Service Layer - CustomerService.java and CustomerServiceImpl.java
10.  DAO Layer - CustomerDAO.java and CustomerDAOImpl.java 
11.  JSP Views - customer-form.jsp and list-customers.jspServe Static Resources - CSS and JS
12.  Build and Run an application

 ## 1. Create a Maven Web Application
   Let's create a Maven-based web application either using a command line or from Eclipse IDE.
   Once you created a maven web application, refer below pom.xml file jar dependencies.  
   
 ## 2. Add Dependencies - pom.xml File
 
```xml
<project
    xmlns="http://maven.apache.org/POM/4.0.0"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
 xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>net.javaguides.springmvc</groupId>
    <artifactId>springmvc5-hibernate5-jsp-mysql-example</artifactId>
    <packaging>war</packaging>
    <version>0.0.1-SNAPSHOT</version>
    <name>springmvc5-hibernate5-jsp-mysql-example Maven Webapp</name>
    <url>http://maven.apache.org</url>
    <properties>
        <failOnMissingWebXml>false</failOnMissingWebXml>
        <spring.version>5.1.0.RELEASE</spring.version>
        <hibernate.version>5.3.5.Final</hibernate.version>
        <hibernate.validator>5.4.1.Final</hibernate.validator>
        <c3p0.version>0.9.5.2</c3p0.version>
        <jstl.version>1.2.1</jstl.version>
        <tld.version>1.1.2</tld.version>
        <servlets.version>3.1.0</servlets.version>
        <jsp.version>2.3.1</jsp.version>
        <hsqldb.version>1.8.0.10</hsqldb.version>
    </properties>
    <dependencies>
        <!-- Spring MVC Dependency -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <!-- Spring ORM -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-orm</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <!-- Hibernate Core -->
        <dependency>
            <groupId>org.hibernate</groupId>
            <artifactId>hibernate-core</artifactId>
            <version>5.2.17.Final</version>
        </dependency>
        <!-- Hibernate Validator -->
        <dependency>
            <groupId>org.hibernate</groupId>
            <artifactId>hibernate-validator</artifactId>
            <version>${hibernate.validator}</version>
        </dependency>
        <!-- JSTL Dependency -->
        <dependency>
            <groupId>javax.servlet.jsp.jstl</groupId>
            <artifactId>javax.servlet.jsp.jstl-api</artifactId>
            <version>${jstl.version}</version>
        </dependency>
        <dependency>
            <groupId>taglibs</groupId>
            <artifactId>standard</artifactId>
            <version>${tld.version}</version>
        </dependency>
        <!-- Servlet Dependency -->
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>${servlets.version}</version>
            <scope>provided</scope>
        </dependency>
        <!-- JSP Dependency -->
        <dependency>
            <groupId>javax.servlet.jsp</groupId>
            <artifactId>javax.servlet.jsp-api</artifactId>
            <version>${jsp.version}</version>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.47</version>
        </dependency>
    </dependencies>
    <build>
        <plugins>
            <plugin>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.5.1</version>
                <configuration>
                    <source>1.8</source>
                    <target>1.8</target>
                </configuration>
            </plugin>
        </plugins>
    </build>
</project>

```

The spring-orm module provides the Spring integration with Hibernate:

```xml
       <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-orm</artifactId>
            <version>${spring.version}</version>
        </dependency>

```
## 3. Project Structure
   Our final project structure looks like below format, we will look into each of the components one by one.
   
         packages
                config
                controller
                dao
                entity
                service
                             under web-inf
                                        views
			                                          customer-form.jsp
			                                          list-customers.jsp
						
## 4. AppInitializer - Register a DispatcherServlet using Java-based Spring configuration

  In Spring MVC, The DispatcherServlet needs to be declared and mapped for processing all requests either using java or web.xmlconfiguration.
	
  In a Servlet 3.0+ environment, you can use the AbstractAnnotationConfigDispatcherServletInitializer class to register and initialize the DispatcherServlet programmatically as follows.
  
  
  ```java
  package com.spring.mvc.config;

    import org.springframework.web.servlet.support.AbstractAnnotationConfigDispatcherServletInitializer;

    /**
     * @author Om Prakash
    */
    public class AppInitializer extends AbstractAnnotationConfigDispatcherServletInitializer {

	@Override
	protected Class<?>[] getRootConfigClasses() {
		return new Class[] { AppContext.class };
	}

	@Override
	protected Class<?>[] getServletConfigClasses() {

		return new Class[] { WebMvcConfig.class };
	}

	@Override
	protected String[] getServletMappings() {

		return new String[] { "/" };
	}

   
   }
```

## 5. AppContext - Spring and Hibernate Integration using Java-based Spring configuration

   Hibernate configuration used in the example is based on hibernate Java-based configuration.
   
   ```java
   package com.spring.mvc.config;
import java.util.Properties;

import javax.sql.DataSource;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.PropertySource;
import org.springframework.core.env.Environment;
import org.springframework.jdbc.datasource.DriverManagerDataSource;
import org.springframework.orm.hibernate5.HibernateTransactionManager;
import org.springframework.orm.hibernate5.LocalSessionFactoryBean;
import org.springframework.transaction.annotation.EnableTransactionManagement;

@Configuration
@PropertySource("classpath:database.properties")
@EnableTransactionManagement
public class AppContext {

    @Autowired
    private Environment environment;

    @Bean
    public LocalSessionFactoryBean sessionFactory() {
        LocalSessionFactoryBean sessionFactory = new LocalSessionFactoryBean();
        sessionFactory.setDataSource(dataSource());
        sessionFactory.setPackagesToScan(new String[] {"com.spring.mvc.entity"});
        sessionFactory.setHibernateProperties(hibernateProperties());
        return sessionFactory;
    }

    @Bean
    public DataSource dataSource() {
        DriverManagerDataSource dataSource = new DriverManagerDataSource();
        dataSource.setDriverClassName(environment.getRequiredProperty("jdbc.driverClassName"));
        dataSource.setUrl(environment.getRequiredProperty("jdbc.url"));
        dataSource.setUsername(environment.getRequiredProperty("jdbc.username"));
        dataSource.setPassword(environment.getRequiredProperty("jdbc.password"));
        return dataSource;
    }

    private Properties hibernateProperties() {
        Properties properties = new Properties();
        properties.put("hibernate.dialect", environment.getRequiredProperty("hibernate.dialect"));
        properties.put("hibernate.show_sql", environment.getRequiredProperty("hibernate.show_sql"));
        properties.put("hibernate.format_sql", environment.getRequiredProperty("hibernate.format_sql"));
        properties.put("hibernate.hbm2ddl.auto", environment.getRequiredProperty("hibernate.hbm2ddl.auto"));
        return properties;
    }

    @Bean
    public HibernateTransactionManager getTransactionManager() {
        HibernateTransactionManager transactionManager = new HibernateTransactionManager();
        transactionManager.setSessionFactory(sessionFactory().getObject());
        return transactionManager;
    }
}
   ```
   
- LocalSessionFactoryBean creates a Hibernate SessionFactory. This is the usual way to set up a shared Hibernate SessionFactory in a Spring application context.
- EnableTransactionManagement enables Spring’s annotation-driven transaction management capability.
- HibernateTransactionManager binds a Hibernate Session from the specified factory to the thread, potentially allowing for one thread-bound Session per factory.
- This transaction manager is appropriate for applications that use a single Hibernate SessionFactory for transactional data access, but it also supports direct DataSource access within a transaction i.e. plain JDBC.

### database.properties
Create database.properties file under the resources folder and put the following database configuration in it.
   
   ```xml
jdbc.driverClassName = com.mysql.jdbc.Driver
jdbc.url = jdbc:mysql://localhost:3306/demo?useSSL=false
jdbc.username = root
jdbc.password = 12345
hibernate.dialect = org.hibernate.dialect.MySQL5InnoDBDialect
hibernate.show_sql = true
hibernate.format_sql = true
hibernate.hbm2ddl.auto = update
   ```
## 6. WebMvcConfig - Spring MVC Bean Configuration using Java-based Spring configuration
 Create an MVCConfig class and annotated with @Configuration, @EnableWebMvc, and @ComponentScan annotations.

```java
  package com.spring.mvc.config;
import org.springframework.context.annotation.Bean;

import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.EnableWebMvc;
import org.springframework.web.servlet.config.annotation.ResourceHandlerRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;
import org.springframework.web.servlet.view.InternalResourceViewResolver;
import org.springframework.web.servlet.view.JstlView;

/**
 * @author Om Prakash
 */

@Configuration
@EnableWebMvc
@ComponentScan(basePackages = { "com.spring.mvc"})
public class WebMvcConfig implements WebMvcConfigurer {

    @Bean
    public InternalResourceViewResolver resolver() {
        InternalResourceViewResolver resolver = new InternalResourceViewResolver();
        resolver.setViewClass(JstlView.class);
        resolver.setPrefix("/WEB-INF/views/");
        resolver.setSuffix(".jsp");
        return resolver;
    }

    @Override
    public void addResourceHandlers(ResourceHandlerRegistry registry) {
        registry
            .addResourceHandler("/resources/**")
            .addResourceLocations("/resources/");
    }
}
```

## 7. JPA Entity - Customer.java

Create a @Entity class, whose field names are annotated JPA annotations. We will use this entity class for mapping the database table with Customer. It will also be used for binding from data to the model using @ModelAttribute annotation in controller's handler method.

```java
package com.spring.mvc.entity;
import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import javax.persistence.Table;

@Entity
@Table(name = "customer")
public class Customer {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "id")
    private int id;

    @Column(name = "first_name")
    private String firstName;

    @Column(name = "last_name")
    private String lastName;

    @Column(name = "email")
    private String email;

    public Customer() {

    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getFirstName() {
        return firstName;
    }

    public void setFirstName(String firstName) {
        this.firstName = firstName;
    }

    public String getLastName() {
        return lastName;
    }

    public void setLastName(String lastName) {
        this.lastName = lastName;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    @Override
    public String toString() {
        return "Customer [id=" + id + ", firstName=" + firstName + ", lastName=" + lastName + ", email=" + email + "]";
    }
}
```

## 8. Spring MVC Controller Class - CustomerController.java

Create a controller class to handle the Customer form data as follows.

```java
package com.spring.mvc.controller;
import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.ModelAttribute;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;

import com.spring.mvc.entity.Customer;
import com.spring.mvc.service.CustomerService;

@Controller
@RequestMapping("/customer")
public class CustomerController {

    @Autowired
    private CustomerService customerService;

    @GetMapping("/list")
    public String listCustomers(Model theModel) {
        List < Customer > theCustomers = customerService.getCustomers();
        theModel.addAttribute("customers", theCustomers);
        return "list-customers";
    }

    @GetMapping("/showForm")
    public String showFormForAdd(Model theModel) {
        Customer theCustomer = new Customer();
        theModel.addAttribute("customer", theCustomer);
        return "customer-form";
    }

    @PostMapping("/saveCustomer")
    public String saveCustomer(@ModelAttribute("customer") Customer theCustomer) {
        customerService.saveCustomer(theCustomer);
        return "redirect:/customer/list";
    }

    @GetMapping("/updateForm")
    public String showFormForUpdate(@RequestParam("customerId") int theId,
        Model theModel) {
        Customer theCustomer = customerService.getCustomer(theId);
        theModel.addAttribute("customer", theCustomer);
        return "customer-form";
    }

    @GetMapping("/delete")
    public String deleteCustomer(@RequestParam("customerId") int theId) {
        customerService.deleteCustomer(theId);
        return "redirect:/customer/list";
    }
}
```

## 9. Service Layer - CustomerService.java and CustomerServiceImpl.java

Let's create a CustomerService interface and it's implementation under net.javaguides.springmvc.service package as follows.

```java
package com.spring.mvc.service;
import java.util.List;

import com.spring.mvc.entity.Customer;

public interface CustomerService {

    public List < Customer > getCustomers();

    public void saveCustomer(Customer theCustomer);

    public Customer getCustomer(int theId);

    public void deleteCustomer(int theId);

}
```

### CustomerServiceImpl.java
We are using @Transactional annotation which is applied to the service layer for transaction support. @Service annotation used to annotate service layer implementation classes as in the below file.

```java
package com.spring.mvc.service;
import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

import com.spring.mvc.dao.CustomerDAO;
import com.spring.mvc.entity.Customer;

@Service
public class CustomerServiceImpl implements CustomerService {

    @Autowired
    private CustomerDAO customerDAO;

    @Override
    @Transactional
    public List < Customer > getCustomers() {
        return customerDAO.getCustomers();
    }

    @Override
    @Transactional
    public void saveCustomer(Customer theCustomer) {
        customerDAO.saveCustomer(theCustomer);
    }

    @Override
    @Transactional
    public Customer getCustomer(int theId) {
        return customerDAO.getCustomer(theId);
    }

    @Override
    @Transactional
    public void deleteCustomer(int theId) {
        customerDAO.deleteCustomer(theId);
    }
}
```

