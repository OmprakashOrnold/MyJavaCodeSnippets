#### Q1 Given a list of integers, find out all the even numbers exist in the list using Stream functions?

```package com.java8.programs;
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class EvenNumbers {
   public static void main(String[] args) {
          List<Integer> myList = Arrays.asList(10,15,8,49,25,98,32);
          myList.stream().filter(i->i%2==0).collect(Collectors.toList())
         .forEach(System.out::println);
  }
}```


#### Q2 Given a list of integers, find out all the numbers starting with 1 using Stream functions?

```
package com.java8.programs;

import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class FindAllNumbersStartingWithOne {
  public static void main(String[] args) {
        List<Integer> myList = Arrays.asList(10,15,8,49,25,98,32);
        myList.stream().map(s->s+"").filter(s->s.startsWith("1"))
        .collect(Collectors.toList()).forEach(System.out::println);
   }
}```
