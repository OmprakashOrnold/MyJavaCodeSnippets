#### Difference Between @RequestParam and @PathVariable
| @RequestParam   |@PathVariable   |
| ------------ | ------------ |
|   @RequestParam used for accessing the values of the **query parameters**  | @PathVariable used for accessing the values from the **URI template**.  |
http://localhost:8080/springmvc/hello/101?param1=10&param2=20|http://localhost:8080/springmvc/hello/101?param1=10&amp;param2=20|
RequestParam annotation used for accessing the query parameter **values** from the request|@PathVariable identifies the pattern that is used in the URI for the **incoming request**. 



------------




####  Difference between openSession() and getCurrentSession()
|  openSession()  | getCurrentSession()  |
| ------------ | ------------ |
| It always create **new Session object**|It creates a new Session **if not exists** , else use same session which is in current hibernate context
You need to **explicitly** flush and close session objects|You do **not need to flush and close session** objects, it will be automatically taken care by Hibernate internally|
In single threaded environment , It is **slower** than getCurrentSession|In single threaded environment , It is **faster** than getOpenSession
You **do not need to configure any property to call** this method|You need to configure additional property **“hibernate.current_session_context_class”** to call getCurrentSession method, otherwise it will throw exceptions.


------------


####  Difference between load() and get()
| load()  | get()  |
| ------------ | ------------ |
|  Only use load() method if **you are sure that the object exists**. |If **you are not sure that the object exist**, then use one of get() methods.   |
load() method will **throw an exception** if the unique id is not found in the database.|get() method will **return null** if the unique id is not found in the database.
load() just **returns a proxy by default** and database won't be hit until the proxy is first invoked.|get() will hit the **database immediately.**


------------


####  Difference between FetchType.LAZY and FetchType.EAGER
| FetchType.LAZY  | FetchType.EAGER  |
| ------------ | ------------ |
|   is on **demand** (i.e. when we required the data). |  is **immediate** (i.e. before our requirement comes we are unnecessarily fetching the record)   |
 does not load the relationships **unless you invoke** it via the getter method. |This **loads all** the relationships.
improves performance by avoiding unnecessary **computation** and **reduce memory** requirements.| takes **more memory consumption** and **processing speed is slow**. 
The best practice is to use **FetchType.LAZY** with a mapper (like MapStruct) to transfer data from Entity to another data object DTO and then send it back to the controller, so there is **no exception if the session closed**.|The easiest one is to use **FetchType.EAGER** or any other **Anti-patterns solutions**, So that the session will still be alive at the controller method, but these **methods will impact the performance**.


------------



####  Difference between abstract classes and interfaces
| Abstract Class  |  Interfaces |
| ------------ | ------------ |
|Abstract class declared using **abstract** keyword. |Interface is declared using **interface** keyword.
Abstract class can have both an **abstract** as well as **concrete** methods. |  Interface can have **only abstract methods**. Java 8 onwards, it can have **default** as well as **static** methods. |
Multiple Inheritance is **not supported**.|Interface **supports** Multiple Inheritance.
final, non-final, static and non-static **variables supported**.|Only static and final **variables are permitted**
Abstract class can **implement** an interface.|Interface **can not implement** an interface, it can extend an interface.
Abstract class can have any type of members like **private, public**.|Interface can only have **public members**.


------------



####  Difference between String, StringBuilder, and StringBuffer.
| Factor  |  String | StringBuilder  | StringBuffer  |
| ------------ | ------------ | ------------ | ------------ |
|  **Storage Area** |Constant String Pool   |  Heap Area | Heap Area  |
| **Mutability**  | Immutable  |  Mutable |   Mutable|
|  **Thread Safety** | Yes  |  No |  No |
**Performance**|Fast|More efficient|Less efficient


------------



#### Difference between this() and super() 
|  this() |  super() |
| ------------ | ------------ |
| this() represents the **current instance** of a class  |  super() represents the **current instance of a parent/base class** |
Used to call the **default constructor of the same class**|Used to call the **default constructor of the parent/base class**
Used to **access methods of the current** class|Used to access **methods of the base class**
Used for **pointing the current class** instance|Used for **pointing the superclass instance**
Must be the **first line of a block**|Must be the **first line of a block**


#### Difference Between the constructors and methods
|  Methods |  Constructors |
| ------------ | ------------ |
|   Used to represent the **behavior** of an **object** |   Used to **initialize** the **state** of an object|
Must have a **return type**|Do not have any **return type**
Needs to be invoked **explicitly**|Is invoked **implicitly**
**No default method** is provided by the compiler|A **default constructor** is provided by the compiler if the class has none
Method name **may or may not be** same as class name|Constructor name must always be the **same as the class name**


------------



#### Difference Between Method Overloading and Method Overriding 

| Method Overloading  |  Method Overriding |
| ------------ | ------------ |
| Method overloading is a **compile-time** polymorphism.  |  Method overriding is a **run-time** polymorphism. |
It helps to **increase** the **readability** of the program|It is used to **grant** the **specific** implementation of the method which is already **provided** by its **parent** class or **superclass**
It occurs within the **class**.|It is performed in **two** classes with **inheritance** relationships.
Method overloading **may or may not** require inheritance.|Method overriding a**lways needs** inheritance.
In method overloading, methods must have the same name and **different signatures**|In method overriding, methods must have the **same name and same signature**.
In method overloading, the return type **can or can not be the same**, but we just have to change the parameter.|In method overriding, the **return type must be the same**

------------

#### Difference Between == and equals method in java

| == operator   |  equals method  |
| ------------ | ------------ |
|  == operator is used to compare **primitives**  |  equals() method is used to check **equality** of objects. |
== compares two objects based on **memory reference**|equals method compares based on the **values**.
== is a **binary** operator| equals() is a **method**.

------------

####  Difference Between HashMap and Hashtable 

| HashMap  | Hashtable  |
| ------------ | ------------ |
| HashMap is **non synchronized** and **not thread safe**. |Hashtable is **thread safe** and **synchronized**.
HashMap is better for **non-threading applications.**| Hashtable should be used in **multithreading applications**.
 HashMap **allows one null key and any number of null values**|Hashtable do **not allow null keys and null values** in the Hashtable object.
 HashMap object values are iterated by using **iterator** |Hashtable is the only class other than vector which uses **enumerator** to iterate the values of Hashtable object.
 The iterator in HashMap is **fail-fast** iterator | if the Hashtable is **structurally modified** at any time after the iterator is created in any way except the iterator's own remove method , then the iterator will throw **ConcurrentModification Exception**.Structural modification means adding or removing elements from the Collection object (here HashMap or Hashtable) . Thus the enumerations returned by the Hashtable keys and elements methods are **not fail fast**.
HashMap  works on the Principle of **Hashing** |Hashtable   works on the Principle of **Hashing** 

------------



#### Difference Between Collection and Collections in Java
| Collection  |  Collections |
| ------------ | ------------ |
| Collection is a **root level interface** in Java Collection Framework   |  Collections is a **utility class** in java which contains only **static methods** that operate on or return **collections**. |
Collection is an **interface**. Interface can contain **static methods** since java 8. **Before java8**, interface was not allowed to contain static methods. Interface can also contain **abstract** methods and **default** methods.|Collections class contains only **static methods**.
Collection interface **extends** **iterable** interface.|Collections class e**xtends Object** class.
Both are part of the **Java Collections Framework**.|Both are present in **java.util package.**
Important Methods in Collections **Class** |Important Methods in Collection **Interface** 
Collections.sort()|boolean add()
Collections.reverse()|boolean remove(Object o)
Collections.min()|boolean isEmpty()

------------



