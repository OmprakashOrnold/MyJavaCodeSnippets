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
``
