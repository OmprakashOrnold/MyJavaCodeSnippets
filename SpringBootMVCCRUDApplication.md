#  Spring Boot MVC CRUD Application
If we develop any software following the specific steps, we minimize the chances of getting **bugs.** Furthermore, in case we face any issue after the implementation, we also reduce the time to fix it. Here are the common steps to develop a *‘Spring Boot MVC CRUD Example’.*

### Step#1: Create a starter Project using an IDE

Create a Spring Boot starter project using any IDE like STS, Netbeans, Intellij Idea etc. While creating Starter Project select ***‘Spring Web’***, ***‘Spring Data JPA’***, ***‘MySQL Driver’***, ***‘ Thymeleaf’***, ***‘Lombok’*** and ***‘Spring Boot DevTools’*** as starter project dependencies. Here ‘Lombok’ and ‘Spring Boot Dev Tools’ are optional to add.

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

```java
package com.dev.boot.exception;

public class InvoiceNotFoundException extends RuntimeException {

    private static final long serialVersionUID = 1L;

    public InvoiceNotFoundException() {
        super();
    }

    public InvoiceNotFoundException(String customMessage) {
        super(customMessage);
    }
}
```

### Step#5: Create Service Interface & Service Impl classes

As part of service layer, we need to create an interface and its implementation. Don’t forget to include ***@Service*** at the top of the service implementation class. Additionally, inject the dependency of the Repository interface via ***@Autowired.*** For example, below code demonstrates the concept behind the service layer. As a convention, the service interface should start with the letter ‘I’ to be recognized as an interface. Subsequently, the service implementation class should have a suffix ‘Impl’ in its name as shown below.

```java
package com.dev.boot.service;

import java.util.List;
import com.dev.boot.model.Invoice;

public interface IInvoiceService {

    public Invoice saveInvice(Invoice invoice);
    public List<Invoice> getAllInvoices();
    public Invoice getInvoiceById(Long id);
    public void deleteInvoiceById(Long id);
    public void updateInvoice(Invoice invoice);

}
```
```java
package com.dev.boot.service.impl;


import java.util.List;
import java.util.Optional;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import com.dev.boot.exception.InvoiceNotFoundException;
import com.dev.boot.model.Invoice;
import com.dev.boot.repo.InvoiceRepository;
import com.dev.boot.service.IInvoiceService;

@Service
public class InvoiceServiceImpl implements IInvoiceService{

    @Autowired
    private InvoiceRepository repo;

    @Override
    public Invoice saveInvice(Invoice invoice) {
       return repo.save(invoice);
    }

    @Override
    public List<Invoice> getAllInvoices() {
       return repo.findAll();
    }

    @Override
    public Invoice getInvoiceById(Long id) {
       Optional<Invoice> opt = repo.findById(id);
       if(opt.isPresent()) {
           return opt.get();
       } else {
           throw new InvoiceNotFoundException("Invoice with Id : "+id+" Not Found");
       }
    }

    @Override
    public void deleteInvoiceById(Long id) {
       repo.delete(getInvoiceById(id)); 
    }

    @Override
    public void updateInvoice(Invoice invoice) {
       repo.save(invoice);
    }
}
```

### Step#6: Create Controller class

In order to handle various requests of a client, we need to create a controller as InvoiceController.java. Each handler method will return a UI page based on the criteria it implemented for. For example, below code snippet of InvoiceController will demonstrate the relation between UI page and the controller.

```java
package com.dev.boot.controller;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.ModelAttribute;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.servlet.mvc.support.RedirectAttributes;

import com.dev.boot.exception.InvoiceNotFoundException;
import com.dev.boot.model.Invoice;
import com.dev.boot.service.IInvoiceService;

@Controller
@RequestMapping("/invoice")
public class InvoiceController {

	@Autowired   
	private IInvoiceService service;

	@GetMapping("/")
	public String showHomePage() {
		return "homePage";
	}

	@GetMapping("/register")
	public String showRegistration() {
		return "registerInvoicePage";
	}

	@PostMapping("/save")
	public String saveInvoice( @ModelAttribute Invoice invoice,Model model) {
		service.saveInvice(invoice);
		Long id = service.saveInvice(invoice).getId();
		String message = "Record with id : '"+id+"' is saved successfully !";
		model.addAttribute("message", message);
		return "registerInvoicePage";
	}

	@GetMapping("/getAllInvoices")
	public String getAllInvoices(@RequestParam(value = "message", required = false) String message,Model model) {
		List<Invoice> invoices= service.getAllInvoices();
		model.addAttribute("list", invoices);
		model.addAttribute("message", message);
		return "allInvoicesPage";
	}

	@GetMapping("/edit")
	public String getEditPage(Model model,RedirectAttributes attributes,@RequestParam Long id) {
		String page = null; 
		try {
			Invoice invoice = service.getInvoiceById(id);
			model.addAttribute("invoice", invoice);
			page="editInvoicePage";
		} catch (InvoiceNotFoundException e) {
			e.printStackTrace();
			attributes.addAttribute("message", e.getMessage());
			page="redirect:getAllInvoices";
		}
		return page; 
	}

	@PostMapping("/update")
	public String updateInvoice(@ModelAttribute Invoice invoice,RedirectAttributes attributes) {
		service.updateInvoice(invoice);
		Long id = invoice.getId();
		attributes.addAttribute("message", "Invoice with id: '"+id+"' is updated successfully !");
		return "redirect:getAllInvoices";
	}

	@GetMapping("/delete")
	public String deleteInvoice(@RequestParam Long id,RedirectAttributes attributes) {
		try {
			service.deleteInvoiceById(id);
			attributes.addAttribute("message", "Invoice with Id : '"+id+"' is removed successfully!");
		} catch (InvoiceNotFoundException e) {
			e.printStackTrace();
			attributes.addAttribute("message", e.getMessage());
		}
		return "redirect:getAllInvoices";
	}
}
```

You are here
Home > java >
How to export data into PDF in a Spring Boot MVC Application?

    javapdfSpring Boot

by devs5003 - November 12, 20210

How to export data into PDF in a Spring Boot MVC Application?Nobody can deny from the fact that data is everything for an application. If there is no data, no need of an application. Since data is everything for an application, we need to know each & every way where & how we can import/export data. Many a time, almost every client needs the data in the form of text reports. When we talk about reports in a web application from the front-end, two popular reports come into mind: Excel Report, and the PDF report. Spring Boot has an integration with Excel & PDF and offers us an API to create such type of reports easily. In this article, we are going to learn about ‘How to export data into PDF in a Spring Boot MVC Application?’.

Moreover, we will also provide you all the steps to build a MVC pattern based web App starting from user interface till the data layer. Obviously, our final goal is to learn ‘How to export data into PDF in a Spring Boot MVC Application?’. As a development part, we will start with building an MVC based web App and end with creating an PDF report using the API provided by Spring Boot. Let’s discuss our topic ‘How to export data into PDF in a Spring Boot MVC Application?’ and the related concepts.

Table of Contents (Click on links below to navigate) [hide]

    1 What all Technology & Software we used?
        1.1 iText Dependency
    2 What is PDF Export functionality in a Web Application?
    3 What will be the output of the PDF Export functionality?
    4 How does Spring Boot internally work with iText API?
    5 What all functionalities can you expect from this Spring Boot MVC example?
        5.1 Home Page
        5.2 Invoice Registration Page
        5.3 List Of Invoices Page
    6 Steps to implement the pdf export functionality
    7 What are the steps to develop a Spring Boot MVC CRUD Application?
        7.1 Step#1: Create a starter Project using an IDE
        7.2 Step#2: Update application.properties or application.yml
        7.3 Step#3: Create Entity (model) class
        7.4 Step#4: Create Repository Interface for DB access
        7.5 Step#5: Create Service Interface & Service Impl classes
        7.6 Step#6: Create Controller class
        7.7 Step#7: Create pages as part of view
    8 Spring Boot MVC CRUD Application Example
        8.1 Use-case Details
        8.2 Step#1: Create a Spring Boot Starter Project using STS
        8.3 Step#2: Update application.properties
            8.3.1 application.properties
        8.4 Step#3: Create Entity (model) class
            8.4.1 Invoice.java
        8.5 Step#4: Create Repository Interface for DB access
            8.5.1 InvoiceRepository.java
            8.5.2 Step#4A: Create a custom Exception class
            8.5.3 InvoiceNotFoundException.java
        8.6 Step#5: Create Service Interface & Service Impl classes
            8.6.1 IInvoiceService.java
            8.6.2 InvoiceServiceImpl.java
        8.7 Step#6: Create Controller class
            8.7.1 InvoiceController.java
        8.8 Step#7: Create pages for view
            8.8.1 homePage.html
            8.8.2 registerInvoicePage.html
            8.8.3 allInvoicesPage.html
            8.8.4 editInvoicePage.html
    9 What are the steps to implement pdf export functionality in a Spring Boot MVC Application?
        9.1 Step#1: Add iText dependency to pom.xml
        9.2 Step#2: Create a class to implement pdf export functionality
        9.3 Step#3: Create a method in Controller to accept pdf export requests
        9.4 Step#4: Modify UI page to send pdf export requests
    10 How to test the Application?
    11 Conclusion

What all Technology & Software we used?

♦ STS (Spring Tool Suite) : Version-> 4.7.1.RELEASE
⇒ Dependent Starters : ‘Spring Web’, ‘Spring Data JPA’, ‘MySQL Driver’, ‘Thymeleaf’, ‘Lombok’ and ‘Spring Boot DevTools’
♥ User Interface : Thymeleaf, Bootstrap, Font-Awesome
♦ MySQL Database : Version ->8.0.19 MySQL Community Server
♦ JDK8 or later versions (Extremely tested on JDK8, JDK9 and JDK14)
iText Dependency

We require below two dependencies to be added at pom.xml.

<!-- https://mvnrepository.com/artifact/com.lowagie/itext -->
<dependency>
    <groupId>com.lowagie</groupId>
    <artifactId>itext</artifactId>
    <version>2.1.7</version>
</dependency>

What is PDF Export functionality in a Web Application?

In a web application, we generally have some pages where we show data directly or indirectly mapped to the database. We store these data into the database once the user fills some data in the UI and submits the same. Sometimes we have a requirement to get all data from the UI and save it into a PDF file. In short, we generally call this functionality as an PDF Export. Now-a-days, having this functionality in an application becomes more popular.
What will be the output of the PDF Export functionality?

When we click on the Export PDF icon given in the ‘List Of Invoices’ page, we will get the PDF downloaded. Below is the output that we can expect after implementing this functionality in the form of PDF as a whole.

PDF Export using Spring Boot
How does Spring Boot internally work with iText API?

No doubt, When we use the inbuilt functionality of Spring Boot to export PDF, the whole process becomes very easy to implement and coding steps also reduces. In fact, Spring Boot has integration with iText API to make the PDF generation process easy. We only need to create PDF specific components such as header, footer and title by using iText  API. Rest of the process is handled by Spring Boot itself.

As a part of MVC pattern, Spring Boot classifies PDF generation as a View. There is an interface ‘org.springframework.web.servlet.View’ in Spring Boot API. An abstract class ‘org.springframework.web.servlet.view.AbstractView’ implements ‘org.springframework.web.servlet.View’. Furthermore, ‘org.springframework.web.servlet.view.document.AbstractPdfView’ extends ‘AbstractView’.  Here, ‘AbstractPdfView‘ is an abstract class that we will be using to generate our Pdf. It has below abstract method in its hierarchy that we need to implement in our pdf generation class.

protected abstract void buildPdfDocument(
         Map<String, Object> model, Document document, PdfWriter writer, HttpServletRequest request, HttpServletResponse response)
         throws Exception;

If we open this method of the API, the comment above the method clearly states “Application-provided subclasses must implement this method to populate the Pdf document, given the model.”.
What all functionalities can you expect from this Spring Boot MVC example?

Below is the summary of page-wise functionalities that you can expect from this Spring Boot MVC CRUD Example.
Home Page

1) When the user hits the application URL into the browser, he/she will see the home page that is the entry point of the application. From Here, users will be able to visit the ‘Invoice registration page’ and the ‘list of All invoices’ Page by clicking on the given links.
Invoice Registration Page

2) If user clicks on the ‘Add Invoice’ link available on the home page, he/she will redirect to the invoice registration page.
3) In the Invoice registration page, the user can fill the form and save it after clicking on the ‘Save Invoice’ button. After the successful addition of the record, a message “Invoice with id: ‘XXX’ is added successfully !” will be displayed at the bottom. From this page user can also visit to list of Invoice Pages after clicking on the ‘List Of Invoices’ link.
4) If user clicks on the ‘Show All Invoices’ link available on the home page, he/she will enter to the list of invoice Pages and can see all pre-existing invoices.
List Of Invoices Page

5) In the List Of Invoices page, we will have a link in the form of Icon that we can use to export all the data of the page in an excel sheet.

6) Apart from that, in the List Of Invoices page, user can perform ‘edit’ or ‘delete’ operation on the invoices. Additionally, user can also enter into the Invoice registration page after clicking on the ‘Add Invoice’ link.
7) If user clicks on the ‘Edit’ link available at List Of Invoices page, a new form will open. User can modify the value and update it into the DB after clicking on the ‘Update’ button. After a successful update, a message “Invoice with id: ‘XXX’ is updated successfully !” will be displayed at the bottom.
8) If user clicks on the ‘Delete’ link available at List Of Invoices page, the record will be deleted. After successful removal of the record, a message “Invoice with id: ‘XXX’ is deleted successfully !” will be displayed at the bottom.

9) From the list of Invoices page, user can go back to the home page after clicking on the ‘Go to Home’ link.
Steps to implement the pdf export functionality

In order to implement the pdf export functionality, we need to have some data into a page on UI side. To achieve all this, we have divided the whole implementation into two parts as shown below.

1) Create an MVC web application using Spring Boot that supports CRUD operation and save some data from UI.
2) Implement pdf export functionality in this MVC web application.

However, if you already have a web application and want to implement pdf export functionality, you can directly go the the section ‘What are the steps to implement pdf export functionality in a Spring Boot MVC Application?’.
What are the steps to develop a Spring Boot MVC CRUD Application?

If we develop any software following the specific steps, we minimize the chances of getting bugs. Furthermore, in case we face any issue after the implementation, we also reduce the time to fix it. Here are the common steps to develop a ‘Spring Boot MVC CRUD Example’.
Step#1: Create a starter Project using an IDE

Create a Spring Boot starter project using any IDE like STS, Netbeans, Intellij Idea etc. While creating Starter Project select ‘Spring Web’, ‘Spring Data JPA’, ‘MySQL Driver’, ‘ Thymeleaf’, ‘Lombok’ and ‘Spring Boot DevTools’ as starter project dependencies. Here ‘Lombok’ and ‘Spring Boot Dev Tools’ are optional to add.
Step#2: Update application.properties or application.yml

The next step to update either application.properties or application.yml whichever is applicable. Here we need to provide datasource details like DB driver class name, DB url, DB username and DB password. Additionally, we can provide other configuration properties such as dialect, ddl-auto, show-sql etc. However, as we are talking about a basic MVC application, our purpose to update this file is just to connect through the database.
Step#3: Create Entity (model) class

Now, it’s the first step to start coding. It’s a recommendation that we should start with Entity class.
Step#4: Create Repository Interface for DB access

In order to access the database programmatically, we need to create one Repository Interface that will extend any Repository Interface provided by Spring Data JPA such as JpaRepository, CrudRepository, PagingAndSortingRepository etc. as per our requirement. However, for a basic CRUD operation, we don’t need to write any custom method in this interface. Spring Data JPA will provide all the methods by default that we require in a standard CRUD operation.
Step#5: Create Service Interface & Service Impl classes

Now we are in the Service Layer of the application. Here, we need to create one Service Interface and its Implementation class. Annotate this class with @Service to inform Spring Container that this class will act as a service. In order to connect to Data Layer, we need to provide dependency of the Repository interface (created in Step#4) in our service implementation class via auto-wiring.
Step#6: Create Controller class

Having followed the above steps, once we complete flow of data access, at the last, we need to create a Controller class to handle requests coming from the browser. Annotate this class with @Controller to inform Spring Container that this is a controller. The controller class will have handler methods to serve the requests for performing various operations involved in CRUD with the help of other layers. Please note that the Controller class will connect to the service layer via auto-wiring of Service Interface.
Step#7: Create pages as part of view

In this step, we need to develop user interface part from where a user interacts with the application to perform various operations included in the CRUD. Moreover, any request made by the user from UI page will be handed over to the Controller. Subsequently, based on the request nature, the controller will connect to service layer to serve the request accordingly. However, we can develop Step#6 and Step#7 interchangeably as both are dependent on each other.
Spring Boot MVC CRUD Application Example
Use-case Details

Let’s assume that we have to develop an Invoice Processing Application. As the application name suggests, we must have an Invoice entity in this application. In this example, we will develop CRUD operations for Invoice as an entity accordingly.

Let’s develop Spring Boot MVC CRUD Example step by step as below:
Step#1: Create a Spring Boot Starter Project using STS

Here, we are using STS (Spring tool Suite) as an IDE to develop the example. While creating Starter Project select ‘Spring Web’, ‘Spring Data JPA’, ‘MySQL Driver’, ‘Thymeleaf’, ‘Lombok’ and ‘Spring Boot DevTools’ as starter project dependencies. Here ‘Lombok’ and ‘Spring Boot Dev Tools’ are optional to add. Even If you don’t know how to create Spring Boot Starter Project, Kindly visit Internal Link.  Also, if you want to know more about Lombok, then visit Internal Link.
Step#2: Update application.properties

Let’s update application.properties that we already have after creating the starter project in step#1. For example, we need to include below entries.
application.properties

server.port=8888

spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/invoice-mvc
spring.datasource.username=root
spring.datasource.password=****

spring.jpa.show-sql=true
spring.jpa.database-platform=org.hibernate.dialect.MySQL8Dialect
spring.jpa.hibernate.ddl-auto=update

Step#3: Create Entity (model) class

Since our use-case for this example is Invoice Processing, we will create an entity class as Invoice.java as below.
Invoice.java

package com.dev.springboot.model;

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

Step#4: Create Repository Interface for DB access

Next step is to create one Repository Interface. Please note that we should create a separate repository interface for each Entity in the application. This is also applicable for classes taking part in other layers accordingly. Since we have only one Entity, we will create one Repository Interface for now. As a naming convention, we will create a repository interface as InviceRepository.java as below.
InvoiceRepository.java

package com.dev.springboot.repo;

import org.springframework.data.jpa.repository.JpaRepository;
import com.dev.springboot.model.Invoice;

public interface InvoiceRepository extends JpaRepository<Invoice, Long> {

}

Step#4A: Create a custom Exception class

However, this is an additional step to handle exceptions if any user search for an Invoice by Invoice Id and the same doesn’t exist at all. Let’s assume that a user search from the browser using direct URL, then there is a possibility that the invoice may not exist. In order to handle this scenario, we need to provide the user a readable message. It is possible with the help of creating a custom exception class. For example, below code demonstrates the concept of creating a custom exception.
InvoiceNotFoundException.java

package com.dev.springboot.exception;

public class InvoiceNotFoundException extends RuntimeException {

    private static final long serialVersionUID = 1L;

    public InvoiceNotFoundException() {
        super();
    }

    public InvoiceNotFoundException(String customMessage) {
        super(customMessage);
    }
}

Step#5: Create Service Interface & Service Impl classes

As part of service layer, we need to create an interface and its implementation. Don’t forget to include @Service at the top of the service implementation class. Additionally, inject the dependency of the Repository interface via @Autowired. For example, below code demonstrates the concept behind the service layer. As a convention, the service interface should start with the letter ‘I’ to be recognized as an interface. Subsequently, the service implementation class should have a suffix ‘Impl’ in its name as shown below.
IInvoiceService.java

package com.dev.springboot.service;

import java.util.List;
import com.dev.springboot.model.Invoice;

public interface IInvoiceService {

    public Invoice saveInvice(Invoice invoice);
    public List<Invoice> getAllInvoices();
    public Invoice getInvoiceById(Long id);
    public void deleteInvoiceById(Long id);
    public void updateInvoice(Invoice invoice);

}

InvoiceServiceImpl.java

package com.dev.springboot.service.impl;

import java.util.List;
import java.util.Optional;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import com.dev.springboot.exception.InvoiceNotFoundException;
import com.dev.springboot.model.Invoice;
import com.dev.springboot.repo.InvoiceRepository;
import com.dev.springboot.service.IInvoiceService;

@Service
public class InvoiceServiceImpl implements IInvoiceService{

    @Autowired
    private InvoiceRepository repo;

    @Override
    public Invoice saveInvice(Invoice invoice) {
       return repo.save(invoice);
    }

    @Override
    public List<Invoice> getAllInvoices() {
       return repo.findAll();
    }

    @Override
    public Invoice getInvoiceById(Long id) {
       Optional<Invoice> opt = repo.findById(id);
       if(opt.isPresent()) {
           return opt.get();
       } else {
           throw new InvoiceNotFoundException("Invoice with Id : "+id+" Not Found");
       }
    }

    @Override
    public void deleteInvoiceById(Long id) {
       repo.delete(getInvoiceById(id)); 
    }

    @Override
    public void updateInvoice(Invoice invoice) {
       repo.save(invoice);
    }
}

Step#6: Create Controller class

In order to handle various requests of a client, we need to create a controller as InvoiceController.java. Each handler method will return a UI page based on the criteria it implemented for. For example, below code snippet of InvoiceController will demonstrate the relation between UI page and the controller.
InvoiceController.java

package com.dev.springboot.controller;

import java.util.List;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.ModelAttribute;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.servlet.mvc.support.RedirectAttributes;
import com.dev.springboot.exception.InvoiceNotFoundException;
import com.dev.springboot.model.Invoice;
import com.dev.springboot.service.IInvoiceService;

@Controller
@RequestMapping("/invoice")
public class InvoiceController {

    @Autowired   
    private IInvoiceService service;

    @GetMapping("/")
    public String showHomePage() {
       return "homePage";
    }

    @GetMapping("/register")
    public String showRegistration() {
       return "registerInvoicePage";
    }

    @PostMapping("/save")
    public String saveInvoice(
            @ModelAttribute Invoice invoice,
            Model model
            ) {
        service.saveInvice(invoice);
        Long id = service.saveInvice(invoice).getId();
        String message = "Record with id : '"+id+"' is saved successfully !";
        model.addAttribute("message", message);
        return "registerInvoicePage";
    }

    @GetMapping("/getAllInvoices")
    public String getAllInvoices(
            @RequestParam(value = "message", required = false) String message,
            Model model
            ) {
       List<Invoice> invoices= service.getAllInvoices();
       model.addAttribute("list", invoices);
       model.addAttribute("message", message);
       return "allInvoicesPage";
    }

    @GetMapping("/edit")
    public String getEditPage(
            Model model,
            RedirectAttributes attributes,
            @RequestParam Long id
            ) {
       String page = null; 
       try {
       Invoice invoice = service.getInvoiceById(id);
       model.addAttribute("invoice", invoice);
       page="editInvoicePage";
       } catch (InvoiceNotFoundException e) {
           e.printStackTrace();
           attributes.addAttribute("message", e.getMessage());
           page="redirect:getAllInvoices";
       }
       return page; 
    }

    @PostMapping("/update")
    public String updateInvoice(
            @ModelAttribute Invoice invoice,
            RedirectAttributes attributes
            ) {
       service.updateInvoice(invoice);
       Long id = invoice.getId();
       attributes.addAttribute("message", "Invoice with id: '"+id+"' is updated successfully !");
       return "redirect:getAllInvoices";
    }

    @GetMapping("/delete")
    public String deleteInvoice(
            @RequestParam Long id,
            RedirectAttributes attributes
            ) {
        try {
        service.deleteInvoiceById(id);
        attributes.addAttribute("message", "Invoice with Id : '"+id+"' is removed successfully!");
        } catch (InvoiceNotFoundException e) {
            e.printStackTrace();
            attributes.addAttribute("message", e.getMessage());
        }
        return "redirect:getAllInvoices";
    }
}

### Step#7: Create pages for view

Last part of our example is to create UI pages that will help users to interact with the application. Here, we have four pages : ***homePage.html*** as an entry point of the application, ***registerInvoicePage.html*** to fill the form and register an Invoice, ***allInvoicesPages.html*** to see the list of registered invoices, and ***editInvoicePage.html*** to update the data of any invoice accordingly.

> homePage.html
```html
<html xmlns:th="https://www.thymeleaf.org/">
<head>
<link rel="stylesheet"
	href="https://cdn.jsdelivr.net/npm/bootstrap@4.5.3/dist/css/bootstrap.min.css" />
<script
	src="https://cdn.jsdelivr.net/npm/bootstrap@4.5.3/dist/js/bootstrap.bundle.min.js"></script>
<link rel="stylesheet"
	href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.css" />
</head>
<body>
	<div class="col-12">
		<div class="container">

			<div class="card">
				<div class="card-header bg-info text-center text-white">
					<h3>Welcome to Invoice MVC App</h3>
				</div>
				<div class="card-body">
					<div align="center">
						<form>
							<a th:href="@{/invoice/register}" class="btn btn-success ">Add
								Invoice <i class="fa fa-plus-square" aria-hidden="true"></i>
							</a> <a th:href="@{/invoice/getAllInvoices}" class="btn btn-primary">Show
								All Invoices</a>
						</form>
					</div>
				</div>
			</div>
		</div>
	</div>
</body>
</html>
```

> registerInvoicePage.html
```html
<html xmlns:th="https://www.thymeleaf.org/">
<head>
<link rel="stylesheet"
	href="https://cdn.jsdelivr.net/npm/bootstrap@4.5.3/dist/css/bootstrap.min.css" />
<script
	src="https://cdn.jsdelivr.net/npm/bootstrap@4.5.3/dist/js/bootstrap.bundle.min.js"></script>
<link rel="stylesheet"
	href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.css" />
</head>
<body>
	<div class="col-12">
		<div class="container">

			<div class="card">
				<div class="card-header bg-info text-center text-white">
					<h3>Register Invoices</h3>
				</div>
				<div class="card-body">
					<div align="center">
						<form th:action="@{/invoice/save}" method="POST" id="invoiceForm">
							<div class="row">
								<div class="col-2">
									<label for="name"><b>NAME</b></label>
								</div>
								<div class="col-4">
									<input type="text" name="name" id="name" class="form-control" />
								</div>
							</div>
							<br />
							<div class="row">
								<div class="col-2">
									<label for="location"><b>LOCATION</b></label>
								</div>
								<div class="col-4">
									<input type="text" name="location" id="location"
										class="form-control" />
								</div>
							</div>
							<br />
							<div class="row">
								<div class="col-2">
									<label for="amount"><b>AMOUNT</b></label>
								</div>
								<div class="col-4">
									<input type="text" name="amount" id="amount"
										class="form-control" />
								</div>
							</div>
							<br />
							<button type="submit" class="btn btn-success">
								Save Invoice <i class="fa fa-plus-square" aria-hidden="true"></i>
							</button>
							<a th:href="@{/invoice/getAllInvoices}" class="btn btn-primary">Show
								All Invoices</a>
						</form>
					</div>
				</div>
				<div th:if="${message!=null}" class="card-footer bg-white text-info">
					<span th:text="${message}"></span>
				</div>
			</div>
		</div>
	</div>
</body>
</html>
```

> allInvoicesPages.html
```html
<html xmlns:th="https://www.thymeleaf.org/">
<head>
<link rel="stylesheet"
	href="https://cdn.jsdelivr.net/npm/bootstrap@4.5.3/dist/css/bootstrap.min.css" />
<script
	src="https://cdn.jsdelivr.net/npm/bootstrap@4.5.3/dist/js/bootstrap.bundle.min.js"></script>
<link rel="stylesheet"
	href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.css" />
</head>

<body>
  <div class="col-12">
	<div class="container">
		<div class="card">
			<div class="card-header bg-info text-center text-white">
				<h3>List Of Invoices</h3>
			</div>
			<div class="card-body">
				<table class="table table-hover">
					<tr class="bg-dark text-white">
						<th>ID</th>
						<th>Name</th>
						<th>Location</th>
						<th>Amount</th>
						<th>Edit/Delete</th>
					</tr>
					<tr th:each="ob:${list}">
						<td th:text=${ob.id}></td>
						<td th:text=${ob.name}></td>
						<td th:text=${ob.location}></td>
						<td th:text=${ob.amount}></td>
						<td><a th:href="@{/invoice/delete(id=${ob.id})}" class= "btn btn-danger">DELETE <i class="fa fa-trash-o" aria-hidden="true"></i></a> |
						    <a th:href="@{/invoice/edit(id=${ob.id})}" class="btn btn-warning">EDIT <i class="fa fa-pencil-square-o" aria-hidden="true"></i></a></td>
					</tr>
				</table>
				<a th:href="@{/invoice/register}" class= "btn btn-success ">Add Invoice <i class="fa fa-plus-square" aria-hidden="true"></i></a> 
				<a th:href="@{/invoice/}" class= "btn btn-primary">Go to Home</a>
			</div>
			<div class="card-footer bg-white text-success" th:if="${message!=null}">
				<span th:text="${message}"></span>
			</div>
		</div>
	</div></div>
</body>
</html>
```

> editInvoicePage.html
```html
<html xmlns:th="https://www.thymeleaf.org/">
  <head>
    <link
      rel="stylesheet"
      href="https://cdn.jsdelivr.net/npm/bootstrap@4.5.3/dist/css/bootstrap.min.css"
    />
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@4.5.3/dist/js/bootstrap.bundle.min.js"></script>
    <link
      rel="stylesheet"
      href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.css"
    />
  </head>
  <body>
  <div class="col-12">
    <div class="container">
      <div class="card">
        <div class="card-header bg-primary text-white text-center">
            <h3>Edit Invoice</h3>
        </div>
        <div class="card-body">
            <form th:action="@{/invoice/update}" method="POST" id="invoiceForm" th:object="${invoice}">
                <div class="row">
                    <div class="col-2">
                        <label for="id">ID</label>
                    </div>
                    <div class="col-4">
                        <input type="text" th:field="*{id}" class="form-control" readonly/>
                    </div>
                </div><br/>
                <div class="row">
                    <div class="col-2">
                        <label for="name">NAME</label>
                    </div>
                    <div class="col-4">
                        <input type="text" th:field="*{name}" class="form-control"/>
                    </div>
                </div><br/>
                <div class="row">
                    <div class="col-2">
                        <label for="location">LOCATION</label>
                    </div>
                    <div class="col-4">
                        <input type="text" th:field="*{location}" class="form-control"/>
                    </div>
                </div><br/>
                <div class="row">
                    <div class="col-2">
                        <label for="amount">AMOUNT</label>
                    </div>
                    <div class="col-4">
                        <input type="text" th:field="*{amount}" class="form-control" />
                    </div>
                </div> <br/>
                <button type="submit" class="btn btn-success">Update <i class="fa fa-wrench" aria-hidden="true"></i></button>             
				<a th:href="@{/invoice/getAllInvoices}" class= "btn btn-primary">Show All Invoices</a>
            </form>
        </div>
        <div class="card-footer"></div>
      </div>
    </div></div>
  </body>
</html>
```
