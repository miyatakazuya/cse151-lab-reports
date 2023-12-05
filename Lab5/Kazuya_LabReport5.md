# Lab Report 5 
  
## Part 1  
### **Original Post:**  
> **Title: My bash script doesn't run on my local computer but works on ieng6.**  
> I wanted to work on my ListExamples project on ieng6 instead of my local computer so I can work on it remotely. However, when I tried running the test script, this error occurs.
> Can someone help me identify why my script runs on my computer but not on ieng6? 
```
[cs15lfa23ou@ieng6-201]:lab7:259$ bash test.sh
JUnit version 4.13.2
..
Time: 0.016

OK (2 tests)

[cs15lfa23ou@ieng6-201]:lab7:260$ vim test.sh
[cs15lfa23ou@ieng6-201]:lab7:261$ bash test.sh
ListExamplesTests.java:1: error: package org.junit does not exist
import static org.junit.Assert.*;
                       ^
ListExamplesTests.java:2: error: package org.junit does not exist
import org.junit.*;
^
ListExamplesTests.java:8: error: cannot find symbol
        @Test(timeout = 500)
         ^
  symbol:   class Test
  location: class ListExamplesTests
ListExamplesTests.java:15: error: cannot find symbol
        @Test(timeout = 500)
         ^
  symbol:   class Test
  location: class ListExamplesTests
ListExamplesTests.java:12: error: cannot find symbol
                assertArrayEquals(new String[]{ "a", "b", "x", "y"}, ListExamples.merge(l1, l2).toArray());
                ^
  symbol:   method assertArrayEquals(String[],Object[])
  location: class ListExamplesTests
ListExamplesTests.java:19: error: cannot find symbol
                assertArrayEquals(new String[]{ "a", "b", "c", "c", "d", "e" }, ListExamples.merge(l1, l2).toArray());
                ^
  symbol:   method assertArrayEquals(String[],Object[])
  location: class ListExamplesTests
6 errors
Error: Could not find or load main class org.junit.runner.JUnitCore
Caused by: java.lang.ClassNotFoundException: org.junit.runner.JUnitCore
```


This is my test.sh
```
javac -cp ".;lib/hamcrest-core-1.3.jar;lib/junit-4.13.2.jar" *.java
java -cp ".;lib/junit-4.13.2.jar;lib/hamcrest-core-1.3.jar" org.junit.runner.JUnitCore ListExampleTests
```

### Original Responce (Instructor / TA)  
> First I would make sure that you have commited all the changes on the ieng6 side, and then pull the latest version of your project to ensure you are synched up.
> Are you on a windows computer? If so this bug might be caused by the difference in syntax between Linux and Windows. Try swapping the test.sh syntax with the appropriate windows syntax (mentioned in week 4):
```
javac -cp .:lib/hamcrest-core-1.3.jar:lib/junit-4.13.2.jar *.java
java -cp .:lib/hamcrest-core-1.3.jar:lib/junit-4.13.2.jar org.junit.runner.JUnitCore ListExamplesTests
```

### Reply #! (From Student)
> Thank you that fixed the issue of the tests not running properly. However, my tests are failing and I am not sure why. Out of the 3 tests in ListEaxmplesTests.java, only of them fails. When the input to merge is already sorted, the test runs fine. But when they are not sorted, the program ends earlier than it should and the array is length 3 instead of 4.
>
     
Terminal Output:
```
[cs15lfa23ou@ieng6-201]:lab7:283$ bash test.sh
JUnit version 4.13.2
.E..
Time: 0.022
There was 1 failure:
1) testMerge1(ListExamplesTests)
array lengths differed, expected.length=4 actual.length=3; arrays first differed at element [3]; expected:<y> but was:<end of array>
        at org.junit.internal.ComparisonCriteria.arrayEquals(ComparisonCriteria.java:89)
        at org.junit.internal.ComparisonCriteria.arrayEquals(ComparisonCriteria.java:28)
        at org.junit.Assert.internalArrayEquals(Assert.java:534)
        at org.junit.Assert.assertArrayEquals(Assert.java:285)
        at org.junit.Assert.assertArrayEquals(Assert.java:300)
        at ListExamplesTests.testMerge1(ListExamplesTests.java:12)
        ... 9 trimmed
Caused by: java.lang.AssertionError: expected:<y> but was:<end of array>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at org.junit.Assert.assertEquals(Assert.java:146)
        at org.junit.internal.ComparisonCriteria.arrayEquals(ComparisonCriteria.java:87)
        ... 15 more

FAILURES!!!
Tests run: 3,  Failures: 1
```
ListExampleTests.java:
```
import static org.junit.Assert.*;
import org.junit.*;
import java.util.*;
import java.util.ArrayList;


public class ListExamplesTests {
        @Test(timeout = 500)
        public void testMerge1() {
                List<String> l1 = new ArrayList<String>(Arrays.asList("x", "y"));
                List<String> l2 = new ArrayList<String>(Arrays.asList("a", "b"));
                assertArrayEquals(new String[]{ "a", "b", "x", "y"}, ListExamples.merge(l1, l2).toArray());
        }

        @Test(timeout = 500)
        public void testMerge2() {
                List<String> l1 = new ArrayList<String>(Arrays.asList("a", "b", "c"));
                List<String> l2 = new ArrayList<String>(Arrays.asList("c", "d", "e"));
                assertArrayEquals(new String[]{ "a", "b", "c", "c", "d", "e" }, ListExamples.merge(l1, l2).toArray());
        }

        @Test
        public void testMerge3() {
                List<String> l1 = new ArrayList<String>(Arrays.asList("a"));
                List<String> l2 = new ArrayList<String>(Arrays.asList("b"));
                assertArrayEquals(new String[]{"a", "b"}, ListExamples.merge(l1, l2).toArray());
        }
}
```
ListExamples.java:
```
import java.util.ArrayList;
import java.util.List;

interface StringChecker { boolean checkString(String s); }

class ListExamples {

  // Returns a new list that has all the elements of the input list for which
  // the StringChecker returns true, and not the elements that return false, in
  // the same order they appeared in the input list;
  static List<String> filter(List<String> list, StringChecker sc) {
    List<String> result = new ArrayList<>();
    for(String s: list) {
      if(sc.checkString(s)) {
        result.add(0, s);
      }
    }
    return result;
  }


  // Takes two sorted list of strings (so "a" appears before "b" and so on),
  // and return a new list that has all the strings in both list in sorted order.
  static List<String> merge(List<String> list1, List<String> list2) {
    List<String> result = new ArrayList<>();
    int index1 = 0, index2 = 0;
    while(index1 < list1.size() && index2 < list2.size()) {
      if(list1.get(index1).compareTo(list2.get(index2)) < 0) {
        result.add(list1.get(index1));
        index1 += 1;
      }
      else {
        result.add(list2.get(index2));
        index2 += 1;
      }
    }
    while(index1 < list1.size() - 1 ) {
      result.add(list1.get(index1));
      index1 += 1;
    }
    while(index2 < list2.size()) {
      result.add(list2.get(index2));
      index2 += 1;
    }
    return result;
  }

}
```
**Files in the project:**  
- lab 7  
     - ListExamples.java  
     - ListExamplesTests.java  
     - test.sh  
 

### Responce #2 (Instructor / TA) 


