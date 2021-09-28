# Spring ORM  Main Method

```java
package com.spring.orm;

import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.List;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

import com.spring.orm.dao.StudentDAO;
import com.spring.orm.entities.Student;

/**
 * Hello world!
 *
 */
public class App 
{
	public static void main( String[] args )
	{
		System.out.println( "Programm Started..............." );
		ApplicationContext context = new ClassPathXmlApplicationContext("config.xml");

		StudentDAO studentTemplate = context.getBean("studentDao",StudentDAO.class);

		InputStreamReader r=new InputStreamReader(System.in);    
		BufferedReader br=new BufferedReader(r);    

		boolean go=true;
		while(go){

			System.out.println("Press 1 Add New Student");
			System.out.println("Press 2 Get Student");
			System.out.println("Press 3 Get All Students");
			System.out.println("Press 4 Delete Student");
			System.out.println("Press 5 Update Student");
			System.out.println("Press 6 exit");
			System.out.println();
			System.out.println("****************************************");
			try{
				System.out.println("Press any key from above");    
				Integer input=Integer.parseInt(br.readLine());   
				switch (input) {
				case 1:
					Student student=new Student();
					System.out.println("Enter student id");
					Integer id=Integer.parseInt(br.readLine());
					System.out.println("Enter student Name");
					String sName=br.readLine();
					System.out.println("Enter student City");
					String sCity=br.readLine();
					
					student.setStudentId(id);
					student.setStudentName(sName);
					student.setStudentCity(sCity);
					
					Integer result= studentTemplate.insert(student);
					System.out.println("Student Added Succussfully......");
					System.out.println("****************************************");
					
					break;

				case 2:
					System.out.println("Enter Student id");
					Integer sId=Integer.parseInt(br.readLine());
					Student singleStudent=studentTemplate.getStudent(sId);
					System.out.println(singleStudent.getStudentName());
					System.out.println(singleStudent.getStudentCity());
					System.out.println("****************************************");
					break;

				case 3:
					System.out.println("***********List Of Students*************");
					List<Student> allStudent = studentTemplate.getAllStudent();
					for (Student allStudents : allStudent) {
						System.out.println(allStudents.getStudentId()+" : "+allStudents.getStudentName()+" : "+allStudents.getStudentCity());				
					}
					System.out.println("****************************************");
					break;

				case 4:
					System.out.println("Enter Student id");
					Integer ssId=Integer.parseInt(br.readLine());
					studentTemplate.delete(ssId);
					System.out.println("Student Deleted Succussfully......");
					System.out.println("****************************************");
					break;

				case 5:
					Student studenT=new Student();
					System.out.println("Enter student id");
					Integer sid=Integer.parseInt(br.readLine());
					System.out.println("Enter student Name");
					String ssName=br.readLine();
					System.out.println("Enter student City");
					String ssCity=br.readLine();
					
					studenT.setStudentId(sid);
					studenT.setStudentName(ssName);
					studenT.setStudentCity(ssCity);
					
					studentTemplate.update(studenT);
					System.out.println("Student updated Succussfully......");
					System.out.println("****************************************");
					break;
					
				case 6:
					go=false;

					break;
				}
			}catch(Exception e){
				System.out.println("Please Enter Above keys only");
			}

		}

	}

}


```
# Spring ORM Student Entity

```java
package com.spring.orm.entities;

import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.Id;
import javax.persistence.Table;

@Entity
@Table(name="student_details")
public class Student {
	
	@Id
	@Column(name="student_id")
	private Integer studentId;
	@Column(name="student_name")
	private String studentName;
	@Column(name="student_city")
	private String studentCity;
	
	public Student(Integer studentId, String studentName, String studentCity) {
		super();
		this.studentId = studentId;
		this.studentName = studentName;
		this.studentCity = studentCity;
	}

	public Student() {
		
	}

	public Integer getStudentId() {
		return studentId;
	}

	public void setStudentId(Integer studentId) {
		this.studentId = studentId;
	}

	public String getStudentName() {
		return studentName;
	}

	public void setStudentName(String studentName) {
		this.studentName = studentName;
	}

	public String getStudentCity() {
		return studentCity;
	}

	public void setStudentCity(String studentCity) {
		this.studentCity = studentCity;
	}
	

}

```


# Spring ORM StudentDAO Interface

```java
package com.spring.orm.dao;

import java.util.List;

import com.spring.orm.entities.Student;

public interface StudentDAO {

	
	public int insert(Student student);
	
	public List<Student> getAllStudent();
	
	public Student getStudent(Integer studentId);
	
	public void delete(Integer studentId);
	
	public void update(Student student);


}


```

# Spring ORM StudentDAOImpl class

```java

package com.spring.orm.dao.impl;

import java.util.List;

import javax.transaction.Transactional;

import org.springframework.orm.hibernate5.HibernateTemplate;

import com.spring.orm.dao.StudentDAO;
import com.spring.orm.entities.Student;

public class StudentDAOImpl implements StudentDAO{

	private HibernateTemplate hibernateTemplate;

	public HibernateTemplate getHibernateTemplate() {
		return hibernateTemplate;
	}

	public void setHibernateTemplate(HibernateTemplate hibernateTemplate) {
		this.hibernateTemplate = hibernateTemplate;
	}

	@Transactional
	public int insert(Student student) {
		Integer r=(Integer) this.hibernateTemplate.save(student);
		return r
;	}

	public List<Student> getAllStudent() {
		
		List<Student> students=this.hibernateTemplate.loadAll(Student.class);
		return students;
	}

	public Student getStudent(Integer studentId) {
		Student student = this.hibernateTemplate.get(Student.class, studentId);
		return student;
		
	}

	@Transactional
	public void delete(Integer studentId) {
		Student student=this.hibernateTemplate.get(Student.class, studentId);
		this.hibernateTemplate.delete(student);
	}

	@Transactional
	public void update(Student student) {
		
		this.hibernateTemplate.update(student);
	}

}

```

# Spring ORM config.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:context="http://www.springframework.org/schema/context"
	xmlns:tx="http://www.springframework.org/schema/tx" xmlns:aop="http://www.springframework.org/schema/aop"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
    http://www.springframework.org/schema/context
    http://www.springframework.org/schema/context/spring-context-3.0.xsd
    http://www.springframework.org/schema/aop 
    http://www.springframework.org/schema/aop/spring-aop-3.0.xsd
    http://www.springframework.org/schema/tx
    http://www.springframework.org/schema/tx/spring-tx-3.0.xsd
    ">

	<tx:annotation-driven />

	<bean id="ds"
		class="org.springframework.jdbc.datasource.DriverManagerDataSource">
		<property name="driverClassName" value="com.mysql.cj.jdbc.Driver" />
		<property name="url" value="jdbc:mysql://localhost:3306/springjdbc" />
		<property name="username" value="root" />
		<property name="password" value="12345" />
	</bean>

	<bean id="hibernateTemplate" class="org.springframework.orm.hibernate5.HibernateTemplate">
		<property name="sessionFactory" ref="localFactory">
		</property>
	</bean>


	<bean id="transactionManager"
		class="org.springframework.orm.hibernate5.HibernateTransactionManager">
		<property name="sessionFactory" ref="localFactory">
		</property>
	</bean>

	<bean id="localFactory"
		class="org.springframework.orm.hibernate5.LocalSessionFactoryBean">

		<!-- data base properties -->
		<property name="dataSource" ref="ds" />

		<!-- hibernate properties -->
		<property name="hibernateProperties">
			<props>
				<prop key="hibernate.dialect">org.hibernate.dialect.MySQL8Dialect</prop>
				<prop key="hibernate.show_sql">true</prop>
				<prop key="hibernate.hbm2ddl.auto">update</prop>

			</props>
		</property>

		<!-- annoted class -->

		<property name="annotatedClasses">
			<list>
				<value>com.spring.orm.entities.Student</value>
			</list>
		</property>


	</bean>

	<bean id="studentDao" class="com.spring.orm.dao.impl.StudentDAOImpl">
		<property name="hibernateTemplate" ref="hibernateTemplate">
		</property>
	</bean>
</beans>
 



```

# Spring ORM pom.xml

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>

	<groupId>com.spring.orm</groupId>
	<artifactId>springorm</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<packaging>jar</packaging>

	<name>springorm</name>
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

		<!-- https://mvnrepository.com/artifact/org.springframework/spring-orm -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-orm</artifactId>
			<version>5.3.8</version>
		</dependency>



		<!-- https://mvnrepository.com/artifact/org.hibernate/hibernate-core -->
		<dependency>
			<groupId>org.hibernate</groupId>
			<artifactId>hibernate-core</artifactId>
			<version>5.3.8.Final</version>
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




