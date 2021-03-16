## •通过By.cssSelector("css定位表达式")在页面上查找元素 

```java
package com.lhz.ch03;

import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.firefox.FirefoxDriver;

public class CssDemo {

	public static void main(String[] args) {
		String url = "file:///D:/upupw32/htdocs/test/03.html";

		System.setProperty("webdriver.gecko.driver", "D:\\demo\\geckodriver.exe");


		WebDriver driver = new FirefoxDriver();
		driver.get(url);
		driver.findElement(By.cssSelector("html body input"))
		.sendKeys("lhz");

		driver.findElement(By.cssSelector("input#password"))
		.sendKeys("20");
		
//		driver.findElement(By.xpath("//input[contais(text(),'hao')]")).click();
	

	}

}
```

## 通过 By.tagName(“页面中的html标签名称”)在页面上查找元素 

```java
package com.lhz.ch03;

import java.util.List;

import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.firefox.FirefoxDriver;

public class TagNameDemo {

	public static void main(String[] args) {
		String url = "file:///D:/upupw32/htdocs/test/03.html";

		System.setProperty("webdriver.gecko.driver", "D:\\demo\\geckodriver.exe");
//		System.setProperty("webdriver.firefox.bin", "C:\\Program Files\\Mozilla Firefox\\firefox.exe");
		// 1、创建Driver对象

		WebDriver driver = new FirefoxDriver();
		driver.get(url);
		driver.findElement(By.tagName("input")).sendKeys("first");
		
		 List<WebElement> list=driver.findElements(By.tagName("input"));
		
		 list.get(0).clear();
		 list.get(0).sendKeys("name");
		 list.get(1).sendKeys("123456");
		 list.get(2).sendKeys("20");

	}

}

```

## xpath 

```java
package com.lhz.ch03;

import java.util.List;

import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.firefox.FirefoxDriver;

public class XpathDemo {

	public static void main(String[] args) {
		String url = "file:///D:/upupw32/htdocs/test/03.html";

		System.setProperty("webdriver.gecko.driver", "D:\\demo\\geckodriver.exe");


		WebDriver driver = new FirefoxDriver();
		driver.get(url);
		driver.findElement(By.xpath("//input[starts-with(@id,'user')]"))
		.sendKeys("lhz");
//		driver.findElement(By.xpath("//input[ends-with(@id,'ord')]"))
//		.sendKeys("123456"); //在新版本，这种方式不支持
		driver.findElement(By.xpath("//input[contains(@id,'ge')]"))
		.sendKeys("20");
		
//		driver.findElement(By.xpath("//input[contais(text(),'hao')]")).click();
	

	}

}

```

```html
<a href="https://www.hao123.com" target="_blank" class="mnav c-font-normal c-color-t" one-link-mark="yes">hao123</a>
```

```java
//input[contains(@name,'a')]         查找name属性中包含a关键字的页面元素
```

