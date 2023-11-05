# Lab Report 3

## Part 1 - Bugs

**Failure-inducing Input**  
```
import static org.junit.Assert.*;
import org.junit.*;

public class ArrayTests {
  @Test 
	public void testReverseInPlace() {
    int[] input2 = {1, 2, 3, 4, 5};
    ArrayExamples.reverseInPlace(input2);
    assertArrayEquals(new int[]{ 5, 4, 3, 2, 1 }, input2);
	}
}
```

**Output:**  
```
JUnit version 4.13.2
.E
Time: 0.01
There was 1 failure:
1) testReverseInPlace(ArrayTests)
arrays first differed at element [2]; expected:<1> but was:<3>
        at org.junit.internal.ComparisonCriteria.arrayEquals(ComparisonCriteria.java:78)
        at org.junit.internal.ComparisonCriteria.arrayEquals(ComparisonCriteria.java:28)
        at org.junit.Assert.internalArrayEquals(Assert.java:534)
        at org.junit.Assert.assertArrayEquals(Assert.java:418)
        at org.junit.Assert.assertArrayEquals(Assert.java:429)
        at ArrayTests.testReverseInPlace(ArrayTests.java:11)
        ... 30 trimmed
Caused by: java.lang.AssertionError: expected:<1> but was:<3>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at org.junit.Assert.assertEquals(Assert.java:146)
        at org.junit.internal.ExactComparisonCriteria.assertElementsEqual(ExactComparisonCriteria.java:8)
        at org.junit.internal.ComparisonCriteria.arrayEquals(ComparisonCriteria.java:76)
        ... 36 more

FAILURES!!!
Tests run: 1,  Failures: 1
```

**Input that doesn't induce a failure**  
```
import static org.junit.Assert.*;
import org.junit.*;

public class ArrayTests {
  @Test 
	public void testReverseInPlace() {
    int[] input2 = {1};
    ArrayExamples.reverseInPlace(input2);
    assertArrayEquals(new int[]{ 1 }, input2);
	}
}
```  
**Output:**  
```
JUnit version 4.13.2
.
Time: 0.007

OK (1 test)
```

**Program (Before Fix)**  
```
public class ArrayExamples {

  // Changes the input array to be in reversed order
  static void reverseInPlace(int[] arr) {
    for(int i = 0; i < arr.length; i += 1) {
      arr[i] = arr[arr.length - i - 1];
    }
  }
  
}
```

**Program (After Fix)**  
```
public class ArrayExamples {

  // Changes the input array to be in reversed order
  static void reverseInPlace(int[] arr) {
    for(int i = 0; i < (arr.length / 2); i += 1) {
      int temp = arr[i]; // Temporary Varible stores overriden value
      arr[i] = arr[arr.length - i - 1]; 
      arr[arr.length - i - 1] = temp;
    }
  }

}
```

* Analysis: The original method fails to pass the failure-inducing input because when overriding the value of [0] in the array, the value overriden is not actually switched with the other value. So in the failure inducing input above, when the index of [2] overrides [0], the contents of the input become  [3, 2, 3]. The method terminates here, meaning the test fails because the value at [2] is never properly switched out.

## Part 2 - Researching Commands  
