####  Difference between HashMap and Hashtable / HashMap vs Hashtable  

| HashMap  | Hashtable  |
| ------------ | ------------ |
| HashMap is **non synchronized** and **not thread safe**. |Hashtable is **thread safe** and **synchronized**.
HashMap is better for **non-threading applications.**| Hashtable should be used in **multithreading applications**.
 HashMap **allows one null key and any number of null values**|Hashtable do **not allow null keys and null values** in the Hashtable object.
 HashMap object values are iterated by using **iterator** |Hashtable is the only class other than vector which uses **enumerator** to iterate the values of Hashtable object.
 The iterator in HashMap is **fail-fast** iterator | if the Hashtable is **structurally modified** at any time after the iterator is created in any way except the iterator's own remove method , then the iterator will throw **ConcurrentModification Exception**.Structural modification means adding or removing elements from the Collection object (here HashMap or Hashtable) . Thus the enumerations returned by the Hashtable keys and elements methods are **not fail fast**.
HashMap  works on the Principle of **Hashing** |Hashtable   works on the Principle of **Hashing** 

#### Difference between == and equals method in java

| == operator   |  equals method  |
| ------------ | ------------ |
|  == operator is used to compare **primitives**  |  equals() method is used to check **equality** of objects. |
== compares two objects based on **memory reference**|equals method compares based on the **values**.
== is a **binary** operator| equals() is a **method**.

#### Difference between Collection and Collections in Java
| Collection  |  Collections |
| ------------ | ------------ |
| Collection is a **root level interface** in Java Collection Framework   |  Collections is a **utility class** in java which contains only **static methods** that operate on or return **collections**. |
Collection is an **interface**. Interface can contain **static methods** since java 8. **Before java8**, interface was not allowed to contain static methods. Interface can also contain **abstract** methods and **default** methods.|Collections class contains only **static methods**.
Collection interface **extends** **iterable** interface.|Collections class e**xtends Object** class.
Both are part of the **Java Collections Framework**.|Both are present in **java.util package.**
