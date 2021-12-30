# Mapping Tables 
## @OneToMany 
```java
package com.springboot.blog.entity;

import java.util.HashSet;
import java.util.Set;

import javax.persistence.CascadeType;
import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import javax.persistence.OneToMany;
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

  //CascadeType meaning When we perform some action on the target entity, the same action will be applied to the associated entity.
  @OneToMany(mappedBy = "post",cascade = CascadeType.ALL)
	private Set<Comment> comments=new HashSet<Comment>();

}

```

## @ManyToOne
```java
package com.springboot.blog.entity;

import javax.persistence.Entity;
import javax.persistence.FetchType;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import javax.persistence.JoinColumn;
import javax.persistence.ManyToOne;
import javax.persistence.Table;

import org.hibernate.annotations.ManyToAny;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@Data
@AllArgsConstructor
@NoArgsConstructor

@Entity
@Table(name = "comments")
public class Comment {

	@Id
	@GeneratedValue(strategy = GenerationType.IDENTITY)
	private Long id;
	private String name;
	private String email;
	private String body;
	
  //FetchType.LAZY it tell to hibernate do only fetch the related entities from database
	@ManyToOne(fetch = FetchType.LAZY)
	@JoinColumn(name = "post_id",nullable = false)
	private Post post;

}

```
