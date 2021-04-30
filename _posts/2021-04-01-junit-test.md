---
layout: post
title: "4. First JUnit Test"
date: 2021-04-01 22:32 +0000
tags: selenium junit testing
category: [Selenium & Junit, tutorial]
author: Ananta
image: assets/images/testing.png

---
## Simple JUnit code that tests addition function

1. Create a new java class
`src > New > Class`
![Alt](/assets/images/selenium_and_junit_testing/img(39).png "New Class")
2. Name it.
You can skip selecting `public static void main(String[] args)` method checkbox.
Don't forget to name file same as class name.

![Alt](/assets/images/selenium_and_junit_testing/img(40).png "Name it")

3. Code

```java
import org.junit.Test;
//to test junit config
public class junitTest {
	@Test   //gets executed everytime we run junit program
	public void test1() {
		if (add(10,3)==15) {
			System.out.println("Pass");
		}else {
			System.out.println("Fail");
		}
	}
	@Test   //gets executed everytime we run junit program
	public void test2() {
		if (add(10,5)==15) {
			System.out.println("Pass");
		}else {
			System.out.println("Fail");
		}
	}
	public int add(int x, int y){   //simple addition function returns addition of two numbers
		return x+y;
	}
}

```
There is no main method in this code because JUnit does not require you to compulsorily have a main method in java code.
Here's a gist version.
<script src="https://gist.github.com/ananta-tamboli/79e88a8c5b406161bd1bcd9d2f49815b.js"></script>

![Alt](/assets/images/selenium_and_junit_testing/img(41).png "Code Screenshot")

4. Run as JUnit test to run it and get output in JUnit panel.

![Alt](/assets/images/selenium_and_junit_testing/img(42).png "Run as")

5. Output

Everything went well that is why we do not have errors in execution and both tests ran correctly.
The Image Below shows Expected Output of the given code.

![Alt](/assets/images/selenium_and_junit_testing/img(43).png "Output")

[Github Repo](https://github.com/ananta-tamboli/Go-Woogle/tree/main/Selenium%20and%20JUnit%20codes)

Here's Link to My YouTube Channel.
[Go Woogle](https://www.youtube.com/channel/UC-DZPwBRyAiR2MoOYN3M4xA)