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
