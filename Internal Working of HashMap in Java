# Internal Working of HashMap in Java

### Step 1: Create an empty HashMap

     Map map = new HashMap()
 
The default size of HashMap is taken as 16

### Step 2: Inserting first element Key-Value Pair

    map.put(new Key("Dinesh"), "Dinesh");
   
 - First, it will calculate the hash code of Key {“Dinesh”}.    
 - Hash code will be generated as 4501. 
 - Calculate index by using a generated hash code, 
 - According to the index calculation formula, 
 - It will be 5.

      Now it creates a node object. This node will be placed at index 5.            


### Step 3: Adding another element Key-Value Pair

       map.put(new Key("Anamika"), "Anamika);
       
 - First, it will calculate the hash code of Key {“Anamika”}.  
 - Hash code will be generated as 4498.    
 - Calculate index by using a  generated hash code,  
 - According to the index calculation formula, 
 - It will be 2.

      Now it creates a node .This node will be placed at index 2.

### Step 4: (Case of Collision) Adding another element Key-Value Pair

  

    map.put(new Key("Arnav"), "Arnav);

 - First, it will calculate the hash code of Key {“Arnav”}.   
 - Hash code will be generated as 4498.    
 - Calculate index by using a generated hash code,  
 - According to the index calculation formula,  
 - It will  be 2
 
      Now it creates a node object
 - This node will be placed at index 2 
 - if no other  object is  presented there. 
 - But at this index 2, one node is already presented,
 -  So this is the case of a collision.
 -  Now, it will check hashCode() and equals() method
 -  if both keys are same then it will replace the
 -   old value with current  value.
 -   If both keys are not the same then it will connect
 -   this node to the previous node

 object via the linked list and both are stored at  index 2.

# HashMap’s get() method work internally

   Now we will fetch a value from the HashMap using the get() method.
   
             map.get(new Key("Arnav")) 

 - First, it will calculate the hash code of Key {“Arnav”}. 
 - hash code will be generated as 4498.
 -    Calculate index by using a generated hash code,
 -  according to the index calculation formula, it will be 2
 -  Go to index 2 of an array and compare the firs
 -   element’s key with given key.
 - If both are equals then return the value,
 - otherwise, check for next element if it exists.
 -   In our case, it is not found as the first element and
 -  next of node object is not null.
 -  If next of node is null then return null.
 - If next of node is not null traverse to the second element and repeat the process 3 until a key is not found or next is not    null.

