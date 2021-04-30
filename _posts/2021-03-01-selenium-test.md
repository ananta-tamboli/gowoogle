---
layout: post
title: "3. First Selenium Test"
date: 2021-03-01 22:32 +0000
tags: selenium junit testing
category: [Selenium & Junit, tutorial]
author: Ananta
image: assets/images/testing.png

---
## How to Open a Website in Chrome Browser using Selenium

1. Create a new class in source folder

![Alt](/assets/images/selenium_and_junit_testing/img(32).png "New Class")

2. Name it and Click `Finish`
check `public static void main(String[] args)`
option so that you do not need to type it manually.
You can completely avoid it in JUnit testing since, it is not required in JUnit Test cases.

![Alt](/assets/images/selenium_and_junit_testing/img(33).png "Name it")

3. Code

As you can see I have added comments to help you understand code.

```java

import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;

public class openWebsite {

	public static void main(String[] args) {
		System.setProperty("webdriver.chrome.driver", "C:\\chromedriver.exe"); //Driver path
		WebDriver driver = new ChromeDriver();
		driver.get("https://gowoogle.com"); //URL that will open in chrome
		driver.manage().window().maximize(); //Maximizes Browser Window
	}

}
```
Please save the file name as `openWebsite.java` because Java is case sensitive and filename and class name must be same.
Here's the gist version.
<script src="https://gist.github.com/ananta-tamboli/f05ab05a2bdd1672ad78cbbd118ddfac.js"></script>

![Alt](/assets/images/selenium_and_junit_testing/img(34).png "Code")

4. Run as Java Application. This can be done by mulltiple ways I find Right Click `Run as > Java Application` as my go to method to run specific java files.

![Alt](/assets/images/selenium_and_junit_testing/img(35).png "Run as")

5. Console will show execution progress.

![Alt](/assets/images/selenium_and_junit_testing/img(36).png "Console ouput")

6. Website will Open in Chrome Browser just as web programmed it.

![Alt](/assets/images/selenium_and_junit_testing/img(37).png "Website opened")

7. Browser Window will Maximize.

![Alt](/assets/images/selenium_and_junit_testing/img(38).png "Window maximized")

[Github Repo](https://github.com/ananta-tamboli/Go-Woogle/tree/main/Selenium%20and%20JUnit%20codes)


[Next Article to learn How to Write Your First JUnit Test](https://gowoogle.com/junit-test)