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
