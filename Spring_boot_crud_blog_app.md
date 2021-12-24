# Spring Boot Blog Rest Basic API Application Example 

## Spring boot main

```java
package com.springboot.blog;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class SpringbootBlogRestApiApplication {

	public static void main(String[] args) {
		SpringApplication.run(SpringbootBlogRestApiApplication.class, args);
	}

}
```

## Post Controller
```java
package com.springboot.blog.controller;

import java.util.List;

import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.DeleteMapping;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.PutMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import com.springboot.blog.payload.PostDto;
import com.springboot.blog.service.PostService;

@RestController
@RequestMapping("api/posts")
public class PostController {

	private PostService postService;

	public PostController(PostService postService) {
		this.postService = postService;
	}
	
	@PostMapping
	public ResponseEntity<PostDto> createPost(@RequestBody PostDto postDto){
		return new ResponseEntity<PostDto>(postService.createPost(postDto), HttpStatus.CREATED);	
	}
	
	@GetMapping
	public List<PostDto> getAllPosts(){
		return postService.getAllPosts();		
	}
	
	@GetMapping("/{id}")
	public ResponseEntity<PostDto> getPostById(@PathVariable(name = "id") Long id) {		
		return  ResponseEntity.ok(postService.getPostById(id));	
	}

	@PutMapping("/{id}")
	public ResponseEntity<PostDto> updatePost(@RequestBody PostDto postDto ,@PathVariable(name = "id") Long id) {		
		PostDto postResonse=postService.updatePost(postDto, id);
		return new ResponseEntity<>(postResonse,HttpStatus.OK);
	}
	
	@DeleteMapping("/{id}")
	public ResponseEntity<String> deletePost(@PathVariable(name = "id") Long id) {		
		postService.deletePostById(id);
		return new ResponseEntity<>("deleted post successfully ",HttpStatus.OK);
  }

}

```


## Post Entity
```java
package com.springboot.blog.entity;

import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import javax.persistence.Table;
import javax.persistence.UniqueConstraint;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@Data
@AllArgsConstructor
@NoArgsConstructor

@Entity
@Table(name = "post", uniqueConstraints = { @UniqueConstraint(columnNames = { "title" }) })
public class Post {

	@Id
	@GeneratedValue(strategy = GenerationType.IDENTITY)
	private Long id;
	
	@Column(name="title",nullable=false)
	private String title;
	
	@Column(name="description",nullable=false)
	private String description;
	
	@Column(name="content",nullable=false)
	private String content;

}

```


## Custom Exception
```java
package com.springboot.blog.exception;

import org.springframework.http.HttpStatus;
import org.springframework.web.bind.annotation.ResponseStatus;

@ResponseStatus(value = HttpStatus.NOT_FOUND)
public class ResourceNotFoundException extends RuntimeException {

	/**
	 * 
	 */
	private static final long serialVersionUID = 1L;
	private String resourceName;
	private String fieldName;
	private Long fieldValue;
	
	public ResourceNotFoundException(String resourceName, String fieldName, Long fieldValue) {
		super(String.format("%s Not found with %s : '%s' ",resourceName,fieldName,fieldValue));
		this.resourceName = resourceName;
		this.fieldName = fieldName;
		this.fieldValue = fieldValue;
	}

	public String getResourceName() {
		return resourceName;
	}

	public String getFieldName() {
		return fieldName;
	}

	
	public Long getFieldValue() {
		return fieldValue;
	}

}

```


## Dto object
```java
package com.springboot.blog.payload;

import lombok.Data;

@Data
public class PostDto {

	private Long id;
	private String title;
	private String description;
	private String content;
	

}


```


## Post Repository
```java
package com.springboot.blog.repository;

import org.springframework.data.jpa.repository.JpaRepository;

import com.springboot.blog.entity.Post;

public interface PostRepository extends JpaRepository<Post, Long>{

}

```


## Post Service

```java
package com.springboot.blog.service;

import java.util.List;

import com.springboot.blog.payload.PostDto;

public interface PostService {

	public PostDto createPost(PostDto postDto);

	public List<PostDto> getAllPosts();
	
	public PostDto getPostById(Long id);
	
	public PostDto updatePost(PostDto postDto,Long id);
	
	public void deletePostById(Long id);

	
}

```


## Post Service impl

```java
package com.springboot.blog.service.impl;

import java.util.List;
import java.util.stream.Collectors;

import org.springframework.stereotype.Service;

import com.springboot.blog.entity.Post;
import com.springboot.blog.exception.ResourceNotFoundException;
import com.springboot.blog.payload.PostDto;
import com.springboot.blog.repository.PostRepository;
import com.springboot.blog.service.PostService;

@Service
public class PostServiceImpl implements PostService {
	
	private PostRepository postRepo;

	public PostServiceImpl(PostRepository postRepo) {
		this.postRepo = postRepo;
	}

	@Override
	public PostDto createPost(PostDto postDto) {
	
		//convert entity to dto
		Post post = mapToEntity(postDto);		
		Post newpost=postRepo.save(post);
		//convert dto to entity
		PostDto postDtoResponse = mapToDto(newpost);
		
		return postDtoResponse;
	}
	

	@Override
	public List<PostDto> getAllPosts(){
		List<Post> posts=postRepo.findAll();
		return posts.stream().map(post ->mapToDto(post)).collect(Collectors.toList());		
	}
	
	@Override
	public PostDto getPostById(Long id) {
		Post post=postRepo.findById(id).orElseThrow( () ->new ResourceNotFoundException("post","id",id));
		return mapToDto(post);
	}
	

	@Override
	public PostDto updatePost(PostDto postDto ,Long id) {
		
		Post post=postRepo.findById(id).orElseThrow( () ->new ResourceNotFoundException("post","id",id));
		
		post.setTitle(postDto.getTitle());
		post.setDescription(postDto.getDescription());
		post.setContent(postDto.getContent());
		
		Post updatedPost=postRepo.save(post);
		return mapToDto(updatedPost);
	}


	@Override
	public void deletePostById(Long id) {
		
		Post post=postRepo.findById(id).orElseThrow( () ->new ResourceNotFoundException("post","id",id));
		postRepo.delete(post);
	}


	private PostDto mapToDto(Post newpost) {
		PostDto postDtoResponse=new PostDto();
		postDtoResponse.setId(newpost.getId());
		postDtoResponse.setTitle(newpost.getTitle());
		postDtoResponse.setDescription(newpost.getDescription());
		postDtoResponse.setContent(newpost.getContent());
		return postDtoResponse;
	}

	private Post mapToEntity(PostDto postDto) {
		Post post=new Post();
		post.setTitle(postDto.getTitle());
		post.setDescription(postDto.getDescription());
		post.setContent(postDto.getContent());
		return post;
	}

}	

```

## application.properties
```xml
#mysql
spring.datasource.url=jdbc:mysql://localhost:3306/myblog
spring.datasource.username=root
spring.datasource.password=12345

#hibernate
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL5InnoDBDialect
spring.jpa.hibernate.ddl-auto=update


```

## pom.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.6.1</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>
	<groupId>com.springboot.blog</groupId>
	<artifactId>springboot-blog-rest-api</artifactId>
	<version>0.1</version>
	<name>springboot-blog-rest-api</name>
	<description>springboot blog rest api</description>
	<properties>
		<java.version>1.8</java.version>
	</properties>
	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-data-jpa</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-devtools</artifactId>
			<scope>runtime</scope>
			<optional>true</optional>
		</dependency>
		<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
			<scope>runtime</scope>
		</dependency>
		<dependency>
			<groupId>org.projectlombok</groupId>
			<artifactId>lombok</artifactId>
			<optional>true</optional>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
	</dependencies>

	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
				<configuration>
					<excludes>
						<exclude>
							<groupId>org.projectlombok</groupId>
							<artifactId>lombok</artifactId>
						</exclude>
					</excludes>
				</configuration>
			</plugin>
		</plugins>
	</build>

</project>


```


