# Validation in Spring Boot
## java bean validation Basics

 1. ***@NotNull*** validates that the annotated property value is ***not null***.	  
 2. ***@Size*** validates that the annotated property value has a size between the attributes min and max; can be applied to ***String, Collection, Map, and array*** properties.
 3. ***@Min*** validates that the annotated property has a value ***not smaller than the value*** attribute.
 4. ***@Max*** validates that the annotated property has a value ***no larger than the value*** attribute.
 5. ***@Email*** validates that the annotated property is a ***valid email address***.
 6. ***@NotEmpty*** validates that the property is not null or empty;can be applied to ***String, Collection, Map, or Array*** values.
 7.*** @NotBlank*** can be applied only to text values and validates that the property is ***not null or whitespace***.
 
# To implement validation follow this steps

## Step :1 Add Dependency

```xml
<dependency> 
    <groupId>org.springframework.boot</groupId> 
    <artifactId>spring-boot-starter-validation</artifactId> 
</dependency>
```
 
 
 ## Step :2 Add validations annotation where ever reqiured Exmple below
 
 ```java
import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import javax.persistence.Table;
import javax.validation.constraints.Email;
import javax.validation.constraints.NotEmpty;
import javax.validation.constraints.Size;

@Table(name = "users")
@Entity
public class User {
	
	@Id
	@GeneratedValue(strategy = GenerationType.IDENTITY)
	private long id;
	
	@Column(name = "name", nullable = false)
	
	import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import javax.persistence.Table;
import javax.validation.constraints.Email;
import javax.validation.constraints.NotEmpty;
import javax.validation.constraints.Size;

@Table(name = "users")
@Entity
public class User {
	
	@Id
	@GeneratedValue(strategy = GenerationType.IDENTITY)
	private long id;
	
	@Column(name = "name", nullable = false)
	
	// user name should not be null or empty
	// user name should have at least 2 characters
	@NotEmpty
	@Size(min = 2, message = "user name should have at least 2 characters")
	private String name;
	
	// email should be a valid email format
	// email should not be null or empty
	@NotEmpty
	@Email
	private String email;
	
	// password should not be null or empty
	// password should have at least 8 characters
	@NotEmpty
	@Size(min = 8, message = "password should have at least 8 characters")
	private String password;
	
	public User() {
		
	}
	
	public User(String name, String email, String password) {
		super();
		this.name = name;
		this.email = email;
		this.password = password;
	}
	public long getId() {
		return id;
	}
	public void setId(long id) {
		this.id = id;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public String getEmail() {
		return email;
	}
	public void setEmail(String email) {
		this.email = email;
	}
	public String getPassword() {
		return password;
	}
	public void setPassword(String password) {
		this.password = password;
	}
}
	private String name;
	
	// email should be a valid email format
	// email should not be null or empty
	@NotEmpty
	@Email
	private String email;
	
	// password should not be null or empty
	// password should have at least 8 characters
	@NotEmpty
	@Size(min = 8, message = "password should have at least 8 characters")
	private String password;
	
	public User() {
		
	}
	
	public User(String name, String email, String password) {
		super();
		this.name = name;
		this.email = email;
		this.password = password;
	}
	public long getId() {
		return id;
	}
	public void setId(long id) {
		this.id = id;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public String getEmail() {
		return email;
	}
	public void setEmail(String email) {
		this.email = email;
	}
	public String getPassword() {
		return password;
	}
	public void setPassword(String password) {
		this.password = password;
	}
}
 ```
## Step :3 Enable validation on spring rest controller by adding @valid annotation to @RequestBody

```java
  @PostMapping
	public ResponseEntity<PostDto> createPost(@Valid @RequestBody PostDto postDto){
		return new ResponseEntity<PostDto>(postService.createPost(postDto), HttpStatus.CREATED);	
	}

## Step :4 To customize respnse validation we need  to extend ResponseEntityExceptionHandler

   ResponseEntityExceptionHandler class and override  handleMethodArgumentNotValid method
   
   ```java
   	@ExceptionHandler(Exception.class)
	@Override
	public ResponseEntity<Object> handleMethodArgumentNotValid(MethodArgumentNotValidException ex,
		                                                	HttpHeaders headers, 
		                                                  	HttpStatus status,
			                                                WebRequest request) {
		
		 Map<String, String> errors = new HashMap<>();
	        ex.getBindingResult().getAllErrors().forEach((error) ->{
	            String fieldName = ((FieldError)error).getField();
	            String message = error.getDefaultMessage();
	            errors.put(fieldName, message);
	        });

		return  new ResponseEntity<>(errors, HttpStatus.BAD_REQUEST);
	}
	
	any one we can use 
	
	@ExceptionHandler(MethodArgumentNotValidException.class)
		public ResponseEntity<Object> handleMethodArgumentNotValid(MethodArgumentNotValidException exception, WebRequest webrequest) {
		
			Map<String, String> errors = new HashMap<>();
			exception.getBindingResult().getAllErrors().forEach((error) ->{
	            String fieldName = ((FieldError)error).getField();
	            String message = error.getDefaultMessage();
	            errors.put(fieldName, message);
	        });

			return new ResponseEntity<>(errors, HttpStatus.BAD_REQUEST);

		}

   ```
	 
