1、登录 admin 123456

2、新建bug

3、填写内容，保存

```java
package Selenium;

import java.util.List;
import java.util.Set;

import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.firefox.FirefoxDriver;

public class MyBugfree {
	public static void main(String[] args) throws InterruptedException {
		
		String url = "http://localhost:8032/bugfree/index.php/site/login";
		System.setProperty("webdriver.gecko.driver","D:\\demo\\geckodriver.exe");
		
		System.setProperty("webdriver.firefox.bin","D:\\app\\firefox.exe");
		WebDriver driver = new FirefoxDriver();
		driver.get(url);
		
		driver.findElement(By.id("LoginForm_username")).sendKeys("admin");
		driver.findElement(By.id("LoginForm_password")).sendKeys("123456");
		driver.findElement(By.id("LoginForm_password")).submit();
		
		Thread.sleep(2000);
		//获取第一个窗口的句柄
		String firstHandle = driver.getWindowHandle();
		driver.findElement(By.partialLinkText(" 新建 Bug")).click();
		
		
		Thread.sleep(2000);
		//因为点击新建 Bug会弹出另外一个窗口
		
		//获得所有窗口的句柄
		Set<String> handles = driver.getWindowHandles();
		for(String handle : handles) {
			if(handle.equals(firstHandle)==false) {
				driver.switchTo().window(handle);
			}
		}

		
		driver.findElement(By.id("BugInfoView_title")).sendKeys("A");
		driver.findElement(By.id("BugInfoView_assign_to_name")).sendKeys("Active");
		driver.findElement(By.id("BugInfoView_severity")).sendKeys("3");
		driver.findElement(By.id("Custom_BugType")).click();
		driver.findElement(By.xpath("//option[@value = '代码错误']")).click();
		driver.findElement(By.id("Custom_HowFound")).click();
		driver.findElement(By.xpath("//option[@value = '功能测试']")).click();
		driver.findElement(By.id("Custom_OpenedBuild")).sendKeys("UI自动化测试作业");
		
		driver.findElement(By.xpath("//input[@value = '保存(S)']")).click();
				                  
		
	}
}

```

