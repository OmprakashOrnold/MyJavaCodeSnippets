#  Spring Boot MVC CRUD Application
If we develop any software following the specific steps, we minimize the chances of getting **bugs.** Furthermore, in case we face any issue after the implementation, we also reduce the time to fix it. Here are the common steps to develop a *‘Spring Boot MVC CRUD Example’.*

### Step#1: Create a starter Project using an IDE

Create a Spring Boot starter project using any IDE like STS, Netbeans, Intellij Idea etc. While creating Starter Project select ‘Spring Web’, ‘Spring Data JPA’, ‘MySQL Driver’, ‘ Thymeleaf’, ‘Lombok’ and ‘Spring Boot DevTools’ as starter project dependencies. Here ‘Lombok’ and ‘Spring Boot Dev Tools’ are optional to add.

### Step#2: Update application.properties or application.yml
 The next step to update either application.properties or application.yml whichever is applicable. Here we need to provide datasource details like DB driver class name, DB url, DB username and DB password. Additionally, we can provide other configuration properties such as dialect, ddl-auto, show-sql etc. However, as we are talking about a basic MVC application, our purpose to update this file is just to connect through the database.

                       server.port=8888
                       spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
                       spring.datasource.url=jdbc:mysql://localhost:3306/invoice_mvc
                       spring.datasource.username=root
                       spring.datasource.password=12345
    
                       spring.jpa.show-sql=true
                       spring.jpa.database-platform=org.hibernate.dialect.MySQL8Dialect
                       spring.jpa.hibernate.ddl-auto=update

### Step#3: Create Entity (model) class
  
 Now, it’s the first step to start coding. It’s a recommendation that we should start with Entity class.

```java
    package com.dev.boot.model;
    
    import javax.persistence.Entity;
    import javax.persistence.GeneratedValue;
    import javax.persistence.Id;
    
    import lombok.AllArgsConstructor;
    import lombok.Data;
    import lombok.NoArgsConstructor;
    
    @Entity
    @Data
    @NoArgsConstructor
    @AllArgsConstructor
    public class Invoice {
    
        @Id
        @GeneratedValue
        private Long id;
        private String name;
        private String location;
        private Double amount;
    }
```

### Step#4: Create Repository Interface for DB access
 In order to access the database programmatically, we need to create one Repository Interface that will extend any Repository Interface provided by Spring Data JPA such as JpaRepository, CrudRepository, PagingAndSortingRepository etc. as per our requirement. However, for a basic CRUD operation, we don’t need to write any custom method in this interface. Spring Data JPA will provide all the methods by default that we require in a standard CRUD operation.

```java
    package com.dev.boot.repo;
    
    import org.springframework.data.jpa.repository.JpaRepository;
    
    import com.dev.boot.model.Invoice;
    
    public interface InvoiceRepository extends JpaRepository<Invoice, Long> {
    
    }
 ```
  
### Step#4A: Create a custom Exception class

However, this is an additional step to handle exceptions if any user search for an Invoice by Invoice Id and the same doesn’t exist at all. Let’s assume that a user search from the browser using direct URL, then there is a possibility that the invoice may not exist. In order to handle this scenario, we need to provide the user a readable message. It is possible with the help of creating a custom exception class. For example, below code demonstrates the concept of creating a custom exception
