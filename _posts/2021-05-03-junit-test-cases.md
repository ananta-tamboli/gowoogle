---
layout: post
title:  "JUnit Test Cases"
author: Ananta
date: "03-05-2021"
category: [Selenium & Junit, tutorial]
image: assets/images/testing.png
comments: false
---

Selenium and JUnit testing environment setup tutorial link:

[CLICK HERE](https://gowoogle.com/selenium-and-junit-downloads/)

Complete series on software testing:

[Complete Selenium and JUnit Tutorial](https://gowoogle.com/categories#Selenium-&-Junit)

JUnit is the most popular unit Testing framework in Java. It is explicitly recommended for Unit Testing. JUnit does not require server for testing web application, which makes the testing process fast.

JUnit framework also allows quick and easy generation of test cases and test data. The org.Junit package consist of many interfaces and classes for JUnit Testing such as Test, Assert, After, Before, etc.

## What is Test fixture

Before we understand what a test fixture is, let's study the code below

This code is designed to execute two test cases on a simple file.

```java
public class OutputFileTest {
    private File output; 
    output = new File(...);
    output.delete(); 
public void testFile1(){
        //Code to verify Test Case 1
}
    output.delete();
    output = new File(...);
public void testFile2(){
        //Code to verify Test Case 2
}
 output.delete(); 
}
```

`Few issues here`

* The code is not readable
* The code is not easy to maintain.
* When the test suite is complex the code could contain logical issues.

Compare the same code using JUnit

```java
public class OutputFileTest  
{
    private File output; 
    @Before public void createOutputFile() 
    { 
       output = new File(...);
    }
  
 @After public void deleteOutputFile() 
    {
        output.delete(); 
    } 
     
    @Test public void testFile1() 
    {
       // code for test case objective
    } 
 @Test public void testFile2() 
    {
       // code for test case objective
    }
}
```

The code far more readable and maintainable. The above code structure is a Test fixture.

A test fixture is a context where a Test Case runs. Typically, test fixtures include:

* Objects or resources that are available for any test case.
* Activities required that makes these objects/resources available.
* These activities are
    -allocation (setup)
    -de-allocation (teardown).

## Setup and Teardown

* Usually, there are some repeated tasks that must be done prior to each test case. **Example**: create a database connection.
* Likewise, at the end of each test case, there may be some repeated tasks. Example: to clean up once test execution is over.
* JUnit provides annotations that help in setup and teardown. It ensures that resources are released, and the test system is in a ready state for next test case.
These annotations are discussed below-

### Setup

**@Before** annotation is used on a method containing Java code to run before each test case. i.e it runs before each test execution.

Teardown (regardless of the verdict)

**@After** annotation is used on a method containing java code to run after each test case. These methods will run even if any exceptions are thrown in the test case or in the case of assertion failures.

Note:

* It is allowed to have any number of annotations listed above.
* All the methods annotated with @Before will run before each test case, but they may run in any order.
* You can inherit @Before and @After methods from a super class, Execution is as follows: It is a standard execution process in JUnit.

1. Execute the @Before methods in the superclass
2. Execute the @Before methods in this class
3. Execute a @Test method in this class
4. Execute the @After methods in this class
5. Execute the @After methods in the superclass

-Example: Creating a class with file as a test fixture

```java
public class OutputFileTest  
{
    private File output; 
    @Before public void createOutputFile() 
    { 
       output = new File(...);
    }
  
 @After public void deleteOutputFile() 
    {
        output.delete(); 
    } 
     
    @Test public void testFile1() 
    {
       // code for test case objective
    } 
 @Test public void testFile2() 
    {
       // code for test case objective
    }
}
```

In the above example the chain of execution will be as follows-

`createOutputFile() > testFile1() > deleteOutputFile() > createOutputfile() > testFile2() > deleteOutputFile()`

Assumption: testFile1() runs before testFile2()– which is not guaranteed.

## Once-only setup

* It is possible to run a method only once for the entire test class before any of the tests are executed, and prior to any @Before method(s).
* "Once only setup" are useful for starting servers, opening communications, etc. It's time-consuming to close and re-open resources for each test.
* This can be done using the @BeforeClass annotation

```java
@BeforeClass public static void Method_Name() { 
    // class setup code here 
 } 
```

## Once-only tear down

Similar to once only setup , a once-only cleanup method is also available. It runs after all test case methods and @After annotations have been executed.
It is useful for stopping servers, closing communication links, etc.
This can be done using the @AfterClass annotation

```java
 @AfterClass public static void Method_Name() 
 { 
    // class cleanup code here 
 } 
```

## JUnit Test Suites

If we want to execute multiple tests in a specified order, it can be done by combining all the tests in one place. This place is called as the test suites. More details on how to execute test suites and how it is used in JUnit is covered in this [tutorial](https://gowoogle.com/junit-test/).

## Junit Test Runner

JUnit provides a tool for execution of your test cases.

* JUnitCore class is used to execute these tests.
* A method called runClasses provided by org.junit.runner.JUnitCore, is used to run one or several test classes.
* Return type of this method is the Result object (org.junit.runner.Result), which is used to access information about the tests. See following code example for more clarity.

```java
public class Test {    
   public static void main(String[] args) {         
         Result result = JUnitCore.runClasses(CreateAndSetName.class);     
   for (Failure failure : result.getFailures()) {       
           System.out.println(failure.toString());     
      }  
      System.out.println(result.wasSuccessful());     
   }  
}    
```  

In above code "result" object is processed to get failures and successful outcomes of test cases we are executing.

Let's understand Unit Testing using a live example. We need to create a test class with a test method annotated with @Test as given below:

MyFirstClassTest.java

```java
package gowoogle.JUnit;  

import static org.JUnit.Assert.*;    

import org.JUnit.Test;  

public class MyFirstClassTest {    

    @Test  
    public void myFirstMethod(){     
        String str= "JUnit is working fine";     
        assertEquals("JUnit is working fine",str);     
    }
}  
```

To execute our test method (above) ,we need to create a test runner. In the test runner we have to add test class as a parameter in JUnitCore's runclasses() method . It will return the test result, based on whether the test is passed or failed.

For more details on this see the code below :

TestRunner.java

```java
package gowoogle.JUnit;  

import org.JUnit.runner.JUnitCore;  
import org.JUnit.runner.Result;  
import org.JUnit.runner.notification.Failure;  

public class TestRunner {    
   public static void main(String[] args) {         
            Result result = JUnitCore.runClasses(MyFirstClassTest.class);     
   for (Failure failure : result.getFailures()) {       
              System.out.println(failure.toString());     
      }  
      System.out.println("Result=="+result.wasSuccessful());       
   }  
}

```

### Output

Once TestRunner.java executes our test methods we get output as failed or passed. Please find below output explanation:

1. In this example, after executing MyFirstClassTest.java , test is passed and result is in green.
2. If it would have failed it should have shown the result as Red and failure can be observed in failure trace.

Summary:

* JUnit is a framework which supports several annotations to identify a method which contains a test.
* JUnit provides an annotation called @Test, which tells the JUnit that the public void method in which it is used can run as a test case.
* A test fixture is a context where a test case runs
* To execute multiple tests in a specified order, it can be done by combining all the tests in one place. This place is called as the test suites.
* JUnit provides a tool for execution of the tests where we can run our test cases referred as Test Runner.
