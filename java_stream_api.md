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

### 1) filter() Method
```java
    List<Integer> list = Arrays.asList(10,15,17,20,15,30);
    evenList= list.stream().filter(n->n%2==0)
              .collect(Collectors.toList());
    System.out.println(evenList);
 ```

### 2) filter() Method
```java
    List<String> list = Arrays.asList("Lipsa","Daisy","David","Daniel",null,"Sanya",null); 
    List<String> resultList = new ArrayList<String>();    
    resultList=list.stream().filter(name->name!=null && name.startsWith("D"))
              .collect(Collectors.toList());
    list.stream().filter(name->name!=null && name.startsWith("D")).forEach(System.out::println);
 ```
 
 ### 3) filter() Method
```java
List<Employee> empList=new ArrayList<Employee>();
empList.add(new Employee(1,"Marco","marco@gmail.com",25000));
empList.add(new Employee(2,"Daisy","daisy@gmail.com",30000));
empList.add(new Employee(3,"Michael","michael@gmail.com",40000));
empList.add(new Employee(4,"Sanya","sanya@gmail.com",28000));
empList.add(new Employee(5,"Robin","robin@gmail.com",50000));

empList.stream().filter(employee->employee.getSalary()>25000)
.forEach(employee -> System.out.println(employee.getName()));
```
### 1) map() Method
```java
List<Integer> list = Arrays.asList(2,3,5,7,9);
resultList= list.stream().map(n->n*n).collect(Collectors.toList();

System.out.println(resultList);

list.stream().map(n->n*n).forEach(System.out::println);
```
### 2) map() Method
```java
List<String> list = Arrays.asList("Marco","Daisy","Michael","Sanya","Robin");
                   list.stream()
                   .map(name->name.toUpperCase())
                  .forEach(System.out::println);

resultList= list.stream().map(name->name.toUpperCase())
            .collect(Collectors.toList());

System.out.println(resultList);
```

### 3) map() Method
```java
List<Employee> empList=new ArrayList<Employee>();
empList.add(new Employee(1,"Marco","marco@gmail.com",25000));
empList.add(new Employee(2,"Daisy","daisy@gmail.com",30000));
empList.add(new Employee(3,"Michael","michael@gmail.com",40000));
empList.add(new Employee(4,"Sanya","sanya@gmail.com",28000));
empList.add(new Employee(5,"Robin","robin@gmail.com",50000));

List<String> nameList = empList.stream().filter(emp->emp.getSalary()>25000)
                        .map(emp->emp.getName())
                        .collect(Collectors.toList());

System.out.println(nameList);
```
### 1) flatMap() Method
```java
List<String> list1 = Arrays.asList("Marco","Daisy");
List<String> list2 = Arrays.asList("Michael","Sanya");
List<String> list3 = Arrays.asList("Robin");

List<List<String>> finalList = Arrays.asList(list1, list2, list3);

System.out.println(finalList);

List<String> resultList = finalList.stream()
                        .flatMap(list -> list.stream())
                        .collect(Collectors.toList());

System.out.println(resultList);
```

### 2) flatMap() Method
```java
List<Integer> list1 = Arrays.asList(2,3);
List<Integer> list2 = Arrays.asList(5,7);
List<Integer> list3 = Arrays.asList(9,11);

List<List<Integer>> finalList = Arrays.asList(list1, list2, list3);

List<Integer> resultList = finalList.stream()
                          .flatMap(list->list.stream())
                          .filter(n->n%2==0)
                          .collect(Collectors.toList());

System.out.println(resultList);
```
### 3) flatMap() Method
```java
List<Author> authorList= Arrays.asList(new Author(1,"author1", Arrays.asList("book1","book2")),
new Author(2,"author2", Arrays.asList("book3","book4")),
new Author(3,"author3", Arrays.asList("book5","book6")));

System.out.println(authorList);

//map() // List<Author> -> List<String> // op : [author1,author2,author3]

List<String> nameList = authorList.stream()
                       .map(author->author.getName())
                       .collect(Collectors.toList());

System.out.println(nameList);

List<List<String>> bookList1 = authorList.stream()
                    .map(list -> list.getBooks())
                   .collect(Collectors.toList());

System.out.println(bookList1);

// flatMap() // List<Author> -> book list // op:[book1,book2,book3,book4,book5,book6]

List<String> bookList = authorList.stream()
                       .flatMap(list -> list.getBooks().stream())
                       .collect(Collectors.toList());

System.out.println(bookList);
```

### 1) sort() Method
```java
List<Integer> list = Arrays.asList(2,11,3,5,10,7,29);
list.stream().sorted(Comparator.reverseOrder()).forEach(System.out::println);
```
### 2) sort() Method
```java
List<String> list = Arrays.asList("Marco","Daisy","Michael","Sanya","Robin");
list.stream().sorted().forEach(System.out::println);
list.stream().sorted(Comparator.reverseOrder()).forEach(System.out::println);
```
### 3) sort() Method
```java
List<Employee> empList = new ArrayList<Employee>();
empList.add(new Employee(1, "Marco", "marco@gmail.com", 25000, "IT"));
empList.add(new Employee(2, "Daisy", "daisy@gmail.com", 30000, "Finance"));
empList.add(new Employee(3, "Michael", "michael@gmail.com", 40000, "HR"));
empList.add(new Employee(4, "Sanya", "sanya@gmail.com", 28000, "IT"));
empList.add(new Employee(5, "Robin", "robin@gmail.com", 50000, "Finance"));
empList.add(new Employee(6, "Albert", "Anel@gmail.com", 50000, "HR"));

empList.stream().sorted(Comparator.comparing(Employee::getSalary).reversed()) //descending order
.forEach(System.out::println);

empList.stream().sorted(Comparator.comparing(Employee::getDepartment).thenComparing(Employee::getName))
.forEach(System.out::println);
```

###  distinct() Method
```java
List<String> courseList= Arrays.asList("Java","Python","Ruby","Javascript","Java","Angular","React");

courseList.stream().distinct().forEach(System.out::println);
```
###  limit() Method
```java
List<Integer> numList = new ArrayList<>();

for (int i = 1; i <=20 ; i++) {
        numList.add(i);
}

System.out.println(numList);

List<Integer> resultList = numList.stream().limit(10)
                         .collect(Collectors.toList());

System.out.println(resultList);

}
```

### skip() Method
```java
List<Integer> numList = Arrays.asList(0,1,2,3,4,5,10,20,30,40,50);
List<Integer> resultList = numList.stream().skip(5)
                        .collect(Collectors.toList());

System.out.println(resultList);
```
### 1) reduce() Method
```java
List<Integer> list = Arrays.asList(10,15,20,25,30);

// before java 8
Integer sum = 0;
for (Integer no:list) {
      sum = sum+no;
}
System.out.println(sum);

// using reduce() method
Optional<Integer> result = list.stream().reduce((a, b) -> a + b);
System.out.println(result.get());

// using method reference
Optional<Integer> result1 = list.stream().reduce(Integer::sum);
System.out.println(result1.get());

//using identity accumalator
Integer result2 = list.stream().reduce(0, (a, b) -> a + b);
System.out.println(result2);
```
### 2) reduce() Method
```java
List<String> list = Arrays.asList("Marco","Daisy","Michael","Sanya","Robin");
Optional<String> result = list.stream().reduce((a, b) -> a.length() > b.length() ? a : b);

System.out.println(result.get()); //Michael
```

### 3) reduce() Method
```java
List<Employee> empList=new ArrayList<Employee>();
empList.add(new Employee(1,"Marco","marco@gmail.com",25000));
empList.add(new Employee(2,"Daisy","daisy@gmail.com",30000));
empList.add(new Employee(3,"Michael","michael@gmail.com",40000));
empList.add(new Employee(4,"Sanya","sanya@gmail.com",28000));
empList.add(new Employee(5,"Robin","robin@gmail.com",50000));

Optional<Float> result = empList.stream().filter(emp -> emp.getSalary() > 25000).map(emp -> emp.getSalary()).reduce((a, b) -> a > b ? a : b);
System.out.println(result.get());

Optional<Float> result1 = empList.stream().filter(emp -> emp.getSalary() > 25000).map(emp -> emp.getSalary()).reduce(Float::max);

System.out.println(result1.get());
```

### 1) min() and max() Method
```java
List<Integer> list = Arrays.asList(10,15,20,25,30);
//min()
Optional<Integer> minValue = list.stream()
                            .min(Comparator.comparing(Integer::intValue));
if(minValue.isPresent()){
System.out.println(minValue.get()); //10
}

list.stream().min(Comparator.comparing(Integer::intValue))
.ifPresent(no-> System.out.println("the min value is..." + no));

//max()
list.stream()
.max(Comparator.comparing(Integer::intValue))
.ifPresent(no-> System.out.println("the max value is ..." +no)); //30
```
### 2) min() and max()  Method
```java
List<String> list = Arrays.asList("Marco","Daisy","Michael","Sanya","Robin");

//min()
Optional<String> minValue = list.stream().min(Comparator.comparing(String::valueOf));
if(minValue.isPresent()){
       System.out.println(minValue.get()); // Daisy
}

list.stream().min(Comparator.comparing(String::valueOf))
.ifPresent(no-> System.out.println("min value is .." + no)); // Daisy

// max()
list.stream().max(Comparator.comparing(String::valueOf))
.ifPresent(no-> System.out.println("max value is .." + no)); // Sanya
```

### 3) min() and max() Method
```java
List<Employee> empList=new ArrayList<Employee>();

empList.add(new Employee(1,"Marco","marco@gmail.com",25000));
empList.add(new Employee(2,"Daisy","daisy@gmail.com",30000));
empList.add(new Employee(3,"Michael","michael@gmail.com",40000));
empList.add(new Employee(4,"Sanya","sanya@gmail.com",28000));
empList.add(new Employee(5,"Robin","robin@gmail.com",50000));

//min()

empList.stream().min(Comparator.comparing(Employee::getSalary))
.ifPresent(employee -> System.out.println("employee with min salary is " + employee.getName())); //Marco

//max()

empList.stream().max(Comparator.comparing(Employee::getSalary))
.ifPresent(employee -> System.out.println("employee with max salary is " + employee.getName())); //Robin
```

### 1) findAny() and findFirst() Method
```java
List<String> courseList= Arrays.asList("Java","Python","Ruby","Javascript","Java","Angular","React");
//findAny()
Optional<String> result = courseList.stream().findAny();
if(result.isPresent()) {
     System.out.println(result.get());
}

//findFirst()
Optional<String> result = courseList.stream().findFirst();
if(result.isPresent()) {
      System.out.println(result.get());
}
```


