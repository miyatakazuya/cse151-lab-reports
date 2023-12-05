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
> Thank you that fixed the issue of the tests not running properly. However, my tests is still not outputing the result of the tests as intended. Once I run the bash script, it never returns any out put and I am not sure why. It may be infinite running somewhere but with no output I can't determine in which file or line the program is getting stuck at. 
>
     
Terminal Output:  
```
[cs15lfa23ou@ieng6-201]:lab7:291$ bash test.sh
JUnit version 4.13.2
.

(Never outputs anything)
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
> That is an interesting bug. I would recommend ussing jdb to try and debug if any local variables like index1 or index2 is not the value it is supposed to be. Since this may be an infinite loop, use the `suspend` command to pause the threads to see where jdb has stopped in ListExamples.java. This may help you understand where the loop is occuring.
  
```
jdb <classpath>
stop at Class:
```

### Final Responce (Student)
>  I followed your advice and ran ListExamplesTests using jdb to debug where my program is going wrong. Here's what i got:
>
  
```
[cs15lfa23ou@ieng6-201]:lab7:297$ jdb -classpath .:lib/* org.junit.runner.JUnitCore ListExamplesTests
Initializing jdb ...
> run
run org.junit.runner.JUnitCore ListExamplesTests
Set uncaught java.lang.Throwable
Set deferred uncaught java.lang.Throwable
> 
VM Started: JUnit version 4.13.2
.suspend
All threads suspended.
> threads
Group system:
  (java.lang.ref.Reference$ReferenceHandler)397 Reference Handler   running
  (java.lang.ref.Finalizer$FinalizerThread)398  Finalizer           cond. waiting
  (java.lang.Thread)399                         Signal Dispatcher   running
  (java.lang.Thread)396                         Notification Thread running
Group main:
  (java.lang.Thread)1                           main                running
Group InnocuousThreadGroup:
  (jdk.internal.misc.InnocuousThread)437        Common-Cleaner      cond. waiting
> where 1
  [1] ListExamples.merge (ListExamples.java:38)
  [2] ListExamplesTests.testMerge1 (ListExamplesTests.java:12)
  [3] java.lang.invoke.LambdaForm$DMH/0x0000000801012400.invokeVirtual (null)
  [4] java.lang.invoke.LambdaForm$MH/0x0000000801013000.invoke (null)
  [5] java.lang.invoke.Invokers$Holder.invokeExact_MT (null)
  [6] jdk.internal.reflect.DirectMethodHandleAccessor.invokeImpl (DirectMethodHandleAccessor.java:154)
  [7] jdk.internal.reflect.DirectMethodHandleAccessor.invoke (DirectMethodHandleAccessor.java:104)
  [8] java.lang.reflect.Method.invoke (Method.java:578)
  [9] org.junit.runners.model.FrameworkMethod$1.runReflectiveCall (FrameworkMethod.java:59)
  [10] org.junit.internal.runners.model.ReflectiveCallable.run (ReflectiveCallable.java:12)
  [11] org.junit.runners.model.FrameworkMethod.invokeExplosively (FrameworkMethod.java:56)
  [12] org.junit.internal.runners.statements.InvokeMethod.evaluate (InvokeMethod.java:17)
  [13] org.junit.runners.ParentRunner$3.evaluate (ParentRunner.java:306)
  [14] org.junit.runners.BlockJUnit4ClassRunner$1.evaluate (BlockJUnit4ClassRunner.java:100)
  [15] org.junit.runners.ParentRunner.runLeaf (ParentRunner.java:366)
  [16] org.junit.runners.BlockJUnit4ClassRunner.runChild (BlockJUnit4ClassRunner.java:103)
  [17] org.junit.runners.BlockJUnit4ClassRunner.runChild (BlockJUnit4ClassRunner.java:63)
  [18] org.junit.runners.ParentRunner$4.run (ParentRunner.java:331)
  [19] org.junit.runners.ParentRunner$1.schedule (ParentRunner.java:79)
  [20] org.junit.runners.ParentRunner.runChildren (ParentRunner.java:329)
  [21] org.junit.runners.ParentRunner.access$100 (ParentRunner.java:66)
  [22] org.junit.runners.ParentRunner$2.evaluate (ParentRunner.java:293)
  [23] org.junit.runners.ParentRunner$3.evaluate (ParentRunner.java:306)
  [24] org.junit.runners.ParentRunner.run (ParentRunner.java:413)
  [25] org.junit.runners.Suite.runChild (Suite.java:128)
  [26] org.junit.runners.Suite.runChild (Suite.java:27)
  [27] org.junit.runners.ParentRunner$4.run (ParentRunner.java:331)
  [28] org.junit.runners.ParentRunner$1.schedule (ParentRunner.java:79)
  [29] org.junit.runners.ParentRunner.runChildren (ParentRunner.java:329)
  [30] org.junit.runners.ParentRunner.access$100 (ParentRunner.java:66)
  [31] org.junit.runners.ParentRunner$2.evaluate (ParentRunner.java:293)
  [32] org.junit.runners.ParentRunner$3.evaluate (ParentRunner.java:306)
  [33] org.junit.runners.ParentRunner.run (ParentRunner.java:413)
  [34] org.junit.runner.JUnitCore.run (JUnitCore.java:137)
  [35] org.junit.runner.JUnitCore.run (JUnitCore.java:115)
  [36] org.junit.runner.JUnitCore.runMain (JUnitCore.java:77)
  [37] org.junit.runner.JUnitCore.main (JUnitCore.java:36)
main[1] locals
Method arguments:
list1 = instance of java.util.ArrayList(id=1025)
list2 = instance of java.util.ArrayList(id=1026)
Local variables:
result = instance of java.util.ArrayList(id=1027)
index1 = 0
index2 = 2
```
I noticed that index1 was 0, when the program was suspended, meaning that the while loop was infinitely running because it was not being properly incremented. 
**Fixing ListExamples.java:**  
```
[cs15lfa23ou@ieng6-201]:lab7:299$ vim ListExamples.java
```
Here I added the missing line that increments the value of the index, fixing the infinite loop that runs. 
+ `index1 += 1;`


**Ran the Test again to make sure it was fixed: **  
```
[cs15lfa23ou@ieng6-201]:lab7:300$ bash test.sh
JUnit version 4.13.2
...
Time: 0.011

OK (3 tests)

```

## Part 2 Reflection
> In general I was amazed at how easily ssh can be utilized to access remote servers and computers. Before the class, such operation would've sounded very foreign and very complicated, but getting to experience how to interact with a server like ieng6 and how to set up methods for accessing it was very interesting. Another thing that I found very helpful was the information we learned regarding URLs. I found myself noticing the division of paths and querys in my daily life using URLs and it was very interesting ot see how those websites exchanged information and accessed data. 

