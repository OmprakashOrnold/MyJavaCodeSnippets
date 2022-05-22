# Java Stream API 
## 1) What is Stream ?
Stream is a sequence of object that support various methods which can be pipelined to produce 
the desired result.
## 2) Stream Opearations
### Intermediate operations
 - This methods are used to transfer one stream  to    another stream
 -  It only return other stream
 -  *filter()*, *map()*,*flatMap()*,*sorted()*
 - *distinct()*,*limit()*,*skip()*
 
### Terminal operations
 - This methods  are used to produce results 
 - It return final result
 - *collect(), forEach(),count(),toArray()*
 - *reduce(),min(),max(),findAny()*
 - *findFirst(),anyMatch(),allMatch(),noneMatch()*
 
## 3) Different ways to create stream
### Using Stream.of()
```java
     Stream<Integer> stream=Stream.of(10,20,30,40);
     stream.forEach(n->System.out.println(n));
````
### Using Stream.of(array)
```java
   Stream<String> stream=Stream.of(new String[] {"Rahul","Viney,"Kuyya"}
		 stream.forEach(System.out::println);
````
### Using list.stream()
```java
    List<String> litst=new ArrayList<String>("Rahul","Viney,"Kuyya");
		 Stream<String>stream=list.stream();
		 stream.forEach(System.out::println);		 
```		 
### Using Stream.generate()
```java
         Stream<String> stream=Stream.generate(()->"stream").limit(5);
		 stream.forEach(System.out::println);
         //it will print 5 times
````		 
### Using Stream.iterate()
```java
           Stream<String> stream=Stram.iterate(10,n->n*2).limit(5);	
		   stream.forEach(System.out::println);
```  
### Using Stram.bulider()
```java
            Stream.Builder<String> builder=Stream.builder();
			Stream<String> stream=bulider.add("ruhal")
			                      .add("Scachin")
								  .add("Sourav")
								  .add("Laxman").build();
``` 
 
## 3) map()  vs  flatMap()	
| map() |  flatMap()	 |
|--|--|
| map() produces one output value for each input value |flatMap() produces one or more values for each input value  |
|one-to-one mapping occurs here  | one-to-many mapping occurs here  |
| it produces a stream value |it produces stream of stream value	  |
| it is for transformations |it is a combination of transformations 	and flattering |

## 4)Example Code Snippets

### a) filter() Method
```java
    List<Integer> list = Arrays.asList(10,15,17,20,15,30);
    evenList= list.stream().filter(n->n%2==0).collect(Collectors.toList());
    System.out.println(evenList);
 ```

### b) filter() Method
```java
    List<String> list = Arrays.asList("Lipsa","Daisy","David","Daniel",null,"Sanya",null); 
    List<String> resultList = new ArrayList<String>();    
    resultList=list.stream().filter(name->name!=null && name.startsWith("D")).collect(Collectors.toList());
    list.stream().filter(name->name!=null && name.startsWith("D"))
    .forEach(System.out::println);
 ```
