####  Explain the difference between constructor and setter injection?

- In constructor injection, **partial injection is not allowed**
      whereas it is allowed in setter injection.
- The constructor injection doesn’t override the **setter property**
      whereas the same is **not true** for setter injection.
- Constructor injection **creates a new instance** if any modification is done. The creation of a new instance is not possible in setter injection.
- In case the **bean has many properties**, then constructor injection is preferred. If it has **few properties**, then setter injection is preferred.

#### What are the bean scopes available in Spring
The Spring Framework has five scope supports. They are:

- **Singleton**: The scope of bean definition while using this would be a single instance per IoC container.
- **Prototype**: Here, the scope for a single bean definition can be any number of object instances.
- **Request**: The scope of the bean definition is an HTTP request.
- **Session**: Here, the scope of the bean definition is HTTP-session.
- **Global-session**: The scope of the bean definition here is a Global HTTP session.

Note: The last three scopes are available only if the users use web-aware ApplicationContext containers.

####  Explain Bean life cycle in Spring Bean Factory Container.


#### Hash Map Internal Working

**Hashing**

> Hashing is a process of **converting an object into integer** form by using the method **hashCode()**. Its necessary to write **hashCode()** method properly for better performance of **HashMap**. 

- **Initially Empty hashMap:** Here, the hashmap is size is taken as 16. 

     `HashMap map = new HashMap();`
- **Inserting Key-Value Pair:** Putting one key-value pair in above HashMap

 `map.put(new Key("vishal"), 20);`
  1. Calculate hash code of Key {“vishal”}. It will be generated as 118.
  2. Calculate index by using index method it will be 6.
  3. Create a node object as :
  ```java
  {
  int hash = 118
  // {"vishal"} is not a string but 
  // an object of class Key
            Key key = {"vishal"}
            Integer value = 20
            Node next = null
  }
  ```
  
  
- **Inserting another Key-Value Pair:** Now, putting other pair that is,

 `map.put(new Key("sachin"), 30);`
 
  1.  Calculate hashCode of Key {“sachin”}. It will be generated as 115.
  2. Calculate index by using index method it will be 3.
  3. Create a node object as : 
  ```java
  {
    int hash = 115
    Key key = {"sachin"}
    Integer value = 30
    Node next = null
  }
 ```
