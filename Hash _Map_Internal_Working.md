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
