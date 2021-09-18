# MyJavaCodeSnippets
# Spring JDBC Main Method

```java
package com.spring.jdbc;
import java.util.List;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import com.spring.jdbc.dao.StudentDAO;
import com.spring.jdbc.entities.Student;

public class App {
  public static void main(String[] args) {
    System.out.println("Program started...................");

    ApplicationContext context = new AnnotationConfigApplicationContext(JdbcConfig.class);

    ApplicationContext context = new ClassPathXmlApplicationContext("com/spring/jdbc/config.xml");

    //insert
    StudentDAO template = context.getBean("studentDao", StudentDAO.class);
    Student student = new Student();
    student.setId(238);
    student.setName("raja sigh");
    student.setCity("United States");
    template.insert(student);
    System.out.println("inserted........");

    //update 
    StudentDAO template = context.getBean("studentDAO", StudentDAO.class);
    Student student = new Student();
    student.setId(153);
    student.setName("Peddamadthala Om Prakash");
    student.setCity("Hyderabad");

    template.change(student);
    System.out.println("data changed........");*/

    //delete
    StudentDAO template = context.getBean("studentDAO", StudentDAO.class);
    template.delete(1543);
    System.out.println("deleted........");*/

    //get single student
    StudentDAO template = context.getBean("studentDAO", StudentDAO.class);
    Student student = template.getStudent(131);
    System.out.println(student);

    //getall students

    //StudentDAO template = context.getBean("studentDao",StudentDAO.class);
    List < Student > student22 = template.getAllStudent();
    for (Student student2: student22) {
      System.out.println(student2);
    }
  }
}

```

# Spring JDBC Entity 

```java
package com.spring.jdbc.entities;
public class Student {

	private Integer id;
	private String name;
	private String city;
	
	public Student() {
		
	}
	public Student(Integer id, String name, String city) {
		super();
		this.id = id;
		this.name = name;
		this.city = city;
	}
	public Integer getId() {
		return id;
	}
	public void setId(Integer id) {
		this.id = id;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public String getCity() {
		return city;
	}
	public void setCity(String city) {
		this.city = city;
	}
	@Override
	public String toString() {
		return "Student [id=" + id + ", name=" + name + ", city=" + city + "]";
	}
}

```

# Spring JDBC DAO Interface 

```java
package com.spring.jdbc.dao;
import java.util.List;
import com.spring.jdbc.entities.Student;
public interface StudentDAO {
	
	public int insert(Student student);
	
	public int change(Student student);
	
	public int delete(int id);
	
	public Student getStudent(int studentId);
	
	public List<Student> getAllStudent();

}


```

# Spring JDBC DAO Implementation

```java

package com.spring.jdbc.dao.impl;
import java.util.List;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.stereotype.Component;
import com.spring.jdbc.dao.StudentDAO;
import com.spring.jdbc.entities.Student;

@Component("studentDao")
public class StudentDAOImpl implements StudentDAO {

	@Autowired
	private JdbcTemplate jdbcTemplate;


	public int insert(Student student) {

		String sql="insert into student(id,name,city) values(?,?,?)";      
		int result = jdbcTemplate.update(sql,student.getId(),student.getName(),student.getCity() );       
		return result;	
	}
	
	public int change(Student student) {
		String sql="update student set name=?,city=? where id=?";      
		int result = jdbcTemplate.update(sql,student.getName(),student.getCity(),student.getId() );       
		return result;	
	}


	public int delete(int id) {		
		String sql="delete from student where id=?"; 		
		int result = jdbcTemplate.update(sql,id);       		
		return result;
	}

	
	public Student getStudent(int studentId) {		
		String sql="select * from student where id=?";
		RowMapperImpl rowMapper=new RowMapperImpl();	
		Student student=jdbcTemplate.queryForObject(sql, rowMapper,studentId);		
		return student;
	}
	
	public List<Student> getAllStudent() {		
     	String sql="select * from student";
		RowMapperImpl rowMapper=new RowMapperImpl();	
		List<Student> student=jdbcTemplate.query(sql, rowMapper);		
		return student;
	
	}

	public JdbcTemplate getJdbcTemplate() {
		return jdbcTemplate;
	}


	public void setJdbcTemplate(JdbcTemplate jdbcTemplate) {
		this.jdbcTemplate = jdbcTemplate;
	}





}


```

# Spring JDBC DAO RowMapper Implemention

```java
package com.spring.jdbc.dao.impl;
import java.sql.ResultSet;
import org.springframework.jdbc.core.RowMapper;
import com.spring.jdbc.entities.Student;

public class RowMapperImpl implements RowMapper<Student> {
	
	public Student mapRow(ResultSet rs, int rowNum) {
		Student student=new Student();
		try{
			student.setId(rs.getInt(1));
			student.setName(rs.getString(2));
			student.setCity(rs.getString(3));
		}catch(Exception e){
			e.printStackTrace();
		}
		return student;
	}

}
```

# Spring JDBC XML Config

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:context="http://www.springframework.org/schema/context"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
    http://www.springframework.org/schema/context
    http://www.springframework.org/schema/context/spring-context-3.0.xsd
    http://www.springframework.org/schema/aop 
    http://www.springframework.org/schema/aop/spring-aop-3.0.xsd
    ">

	<bean id="ds"
		class="org.springframework.jdbc.datasource.DriverManagerDataSource">
		<property name="driverClassName" value="com.mysql.cj.jdbc.Driver" />
		<property name="url" value="jdbc:mysql://localhost:3306/springjdbc" />
		<property name="username" value="root" />
		<property name="password" value="12345" />
	</bean>
	
	<bean id="studentDAO" class="com.spring.jdbc.dao.impl.StudentDAOImpl">
		<property name="jdbcTemplate" ref="jdbcTemplate">		
		</property>

	</bean>
	

	<bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
		<property name="dataSource" ref="ds">		
		</property>

	</bean>
</beans>
 

```

# Spring JDBC java Config

```java
package com.spring.jdbc;
import javax.sql.DataSource;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.jdbc.datasource.DriverManagerDataSource;
import com.spring.jdbc.dao.StudentDAO;
import com.spring.jdbc.dao.impl.StudentDAOImpl;

@Configuration
@ComponentScan(basePackages ="com.spring.jdbc.dao")
public class JdbcConfig {
	
	@Bean("ds")
	public DataSource getDataSource(){		
		DriverManagerDataSource driver=new DriverManagerDataSource();
		driver.setDriverClassName("com.mysql.cj.jdbc.Driver");
		driver.setUrl("jdbc:mysql://localhost:3306/springjdbc");
		driver.setUsername("root");
		driver.setPassword("12345");
		return driver;	
	}
	
	@Bean("jdbcTemplate")
	public JdbcTemplate getJdbcTemplate(){	
		JdbcTemplate jdbcTemplate =new JdbcTemplate();
		jdbcTemplate.setDataSource(getDataSource());
		return jdbcTemplate;	
	}
	
	@Bean("studentDao")
	public StudentDAO getStudentDao(){	
		StudentDAOImpl studentDAO =new StudentDAOImpl();
		studentDAO.setJdbcTemplate(getJdbcTemplate());
		return studentDAO;	
	}

}

```


# Spring JDBC pom.xml

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>

	<groupId>com.spring.jdbc</groupId>
	<artifactId>springjdbc</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<packaging>jar</packaging>

	<name>springjdbc</name>
	<url>http://maven.apache.org</url>

	<properties>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
	</properties>

	<dependencies>

		<!-- https://mvnrepository.com/artifact/org.springframework/spring-core -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-core</artifactId>
			<version>5.3.8</version>
		</dependency>

		<!-- https://mvnrepository.com/artifact/org.springframework/spring-context -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-context</artifactId>
			<version>5.3.8</version>
		</dependency>


		<!-- https://mvnrepository.com/artifact/org.springframework/spring-jdbc -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-jdbc</artifactId>
			<version>5.3.8</version>
		</dependency>

		<!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
		<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
			<version>8.0.25</version>
		</dependency>




		<dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
			<version>3.8.1</version>
			<scope>test</scope>
		</dependency>
	</dependencies>
</project>

```
