复习之前的
```java
String url ="http://localhost:8032/mymovie/admin.php/Login/index.html";
System.setProperty("webdriver.gecko.driver","D:\\demo\\geckodriver.exe");
System.setProperty("webdriver.firefox.bin","D:\\app\\firefox.exe");
WebDriver driver = new FirefoxDriver();
driver.get(url);



WebElement element1 = driver.findElemnet(By.id());

js.executor
```


##使用JavascriptExecutor操作页面的滚动条

•滚动条到页面的最下面

•滚动条到页面的某个元素

•滚动条向下移动某个数量的像素

1. JavascriptExecutor执行js代码的两种方法介绍。

        Object executeScript(String script, Object... args);

        Object executeAsyncScript(String script, Object... args);

前一个是异步执行js，后一个是同步执行js

        executeScript方法接收两个参数和一个返回值：

        script，javascript代码片段，这段js代码片段是作为js函数的完整方法体，可以使用return语句作为函数的返回值。

        args, 参数数组，参数数组用于将外部数据传递给script(js代码片段)，script中可以通过arguments[index]方式索引args数组中的参数；参数数据类型必须是以下几种（number, boolean, String, WebElement, 或者以上数据类型的List集合），当然无参数可以保留为空。

        返回值，返回值是由js代码片段计算后通过return语句返回，返回值数据类型可以为（WebElement，Double，Long，Boolean，String，List或Map），没有return语句，这里返回数据为null。


###JS补充1.window.scroll()

概述

滚动窗口至文档中的特定位置。

语法

```
window.scroll(x-coord, y-coord)  // 参数表示想要置于左上角的像素点的横纵坐标
```

```java
driver.get("http://news.baidu.com/");

Thread.sleep(5000);
//因为要让网页去执行测试人员的 js测试代码   ，所以生成js执行者（Executor）
//事实上webdriver就是通过new Function方式定义匿名函数来运行javascript代码的。


JavascriptExecutor js=(JavascriptExecutor)driver;

//滚动条到最下方
//scrollHeight滚动高度
//这句话就是   网页窗口轴滚动0，Y轴滚动  


js.executeScript("window.scroll(0,document.body.scrollHeight)");

Thread.sleep(5000);

//滚动条到指定元素的位置

js.executeScript("arguments[0].scrollIntoView()",driver.findElement(By.linkText("全球新冠肺炎确诊病例超19万 欧盟将关闭边境30天")));

Thread.sleep(5000);

js.executeScript("window.scrollBy(0,800)");

```

## 使用JavascriptExecutor单击元素 

这一部分要求js基础较好



**arguments** 是一个对应于传递给函数的参数的类数组对象。 

js基础知识补充

```js
function func1(a, b, c) {
  console.log(arguments[0]);
  // expected output: 1

  console.log(arguments[1]);
  // expected output: 2

  console.log(arguments[2]);
  // expected output: 3
}

func1(1, 2, 3);
```





```java

报错     (6)是不是页面太长，向下移动滚动条



click不报错，点击无响应，可以使用这个方法 


//当常规的click事件无效时
WebElement login_link=driver.findElement(By.linkText("登录"));
if(login_link.isDisplayed()&&login_link.isEnabled()) {
	JavascriptExecutor js=(JavascriptExecutor)driver;
	js.executeScript("arguments[0].click();",login_link);//arguments[0]？
}
//按照我的理解 arguments[0]   =  login_link  = driver.findElement(By.linkText("登录")

/*
1.arguments对象和Function是分不开的。

2.因为arguments这个对象不能显式创建。

3.arguments对象只有函数开始时才可用。

这里的arguments[0]表示你这个调用方法的第一个参数
*/
```

```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html lang="en">
<head>  
<meta charset="utf-8"> 
<title>07</title> 
</head>
<body >
用户名<input type="text" id="uid" value="秋天了" size=100></input><p>
请输入内容简介
<textarea id="id1" style="width:98%" cols="50" class="txtarea" rows="4">

</textarea>
</body>
</html>
```


```java
//修改页面元素属性
driver.get("http://localhost:8032/test/05js.html");
JavascriptExecutor jse=(JavascriptExecutor)driver;
Thread.sleep(3000);
WebElement loginLink=driver.findElement(By.id("uid"));

jse.executeScript("arguments[0].setAttribute(arguments[1],arguments[2]);",loginLink,"style","background:yellow;border:2px solid red");
//  arguments[0] = loginLink；  arguments[1] = "style",arguments[2] = "background:yellow;borde

jse.executeScript("arguments[0].setAttribute(arguments[1],arguments[2]);",loginLink,"size", "10");

//修改界面元素的属性
jse.executeScript("arguments[0].setAttribute(arguments[1],arguments[2]);",loginLink,"value", "星期二");

Thread.sleep(3000);


```



练习，根据AddRecord.html

```java
//
driver.get("file:///F:/upupw32/htdocs/test/AddRecord.html");
driver.findElement(By.id("1ImgPr")).sendKeys("C:\\Users\\DELL\\Pictures\\Camera Roll\\103840-150095032034c0.jpg");
//这个方法不可行

//    思路1:1upload_preview     style="display:block" 。下一步 sendKeys
//    思路2：1ImgPr        src属性
```

##失败截屏，文件名为当前时间 

```java
SimpleDateFormat sdf = new SimpleDateFormat("yyyyMMdd-HHmmss");
String nowDateTime = sdf.format(new Date());
File s_file = ((TakesScreenshot) driver).getScreenshotAs(OutputType.FILE);
Files.copy(s_file, new File("D:\\demo\\" + nowDateTime + ".jpg"));

```



# Webdriver的三种等待方式 

##强制等待  

Thread.sleep（3000）

##隐式等待



```JAVA
//隐式等待

driver.manage().timeouts().implicitlyWait(10, TimeUnit.SECONDS);

```



原理：隐式等待，就是在创建driver时，为浏览器对象设置一个全局的等待时间。这个方法是得不到某个元素就等待一段时间，直到拿到某个元素位置。过了这个时间如果对象还没找到的话就会抛出NoSuchElement异常。

注意：在使用隐式等待的时候，实际上浏览器会在你自己设定的时间内不断的刷新页面去寻找我们需要的元素。

## 显式等待 

原理：显式等待，就是明确的要等到某个元素的出现或者是某个元素的可点击等条件，等不到，就一直等，除非在规定的时间之内都没找到，那么就抛出Exception。 

| 元素是否可用和被单击   | elementToBeClickable(locator)                |
| ---------------------- | -------------------------------------------- |
| 元素是否选中           | elementToBeSelected(element)                 |
| 元素是否存在           | presenceOfElementLocated(locator)            |
| 元素是否包含特定的文本 | textToBePresentInElement(locator, text)      |
| 页面元素值             | textToBePresentInElementValue(locator, text) |

```java
//显式等待
WebDriverWait wait =new WebDriverWait(driver, 5);
wait.until(ExpectedConditions.elementToBeClickable(By.linkText("登录")));
System.out.println("继续执行");

```

## 自定义的显式等待 

```java
WebElement input = (new WebDriverWait(driver, 5)).until(new ExpectedCondition<WebElement>() {
@Override
public WebElement apply(WebDriver arg0) {
return driver.findElement(By.id("username"));
}
});

Boolean flag = (new WebDriverWait(driver, 5)).until(new ExpectedCondition<Boolean>() {
@Override
public Boolean apply(WebDriver arg0) {
return driver.findElement(By.id("")).getText().contains("");
}
});

```



## 5.5 H5元素 

   Selenium需要使用JavaScript语句调用，HTML5对象提供的内部变量和函数来实现各类操作，可以参考相关HTML5的接口文档来实现各种复杂的测试操作， HTML5的JS接口文档http://html5index.org/



## HTML5中的视频播放器 

```java
System.setProperty("webdriver.gecko.driver","D:\\demo\\geckodriver.exe");
WebDriver driver = new FirefoxDriver();


driver.get("https://videojs.com/");
WebElement video=driver.findElement(By.tagName("video"));

JavascriptExecutor js=(JavascriptExecutor) driver;
//返回的obj 所以强制转换String
String src=(String)js.executeScript("return arguments[0].currentSrc", video);
System.out.println(src);//获取资源
Double time=(Double)js.executeScript("return arguments[0].duration", video);
System.out.println(time);
Thread.sleep(3000);
js.executeScript("arguments[0].play()", video);

```





```java
driver.get("file:///D:/upupw32/htdocs/test/webstorage.html");
JavascriptExecutor js=(JavascriptExecutor) driver;
String name1=(String)js.executeScript("return localStorage.lastname");
String name2=(String)js.executeScript("return localStorage.getItem('lastname')");
System.out.println(name1);
System.out.println(name2);
```

# 代码检查点 

Ø检查方式元素是否正常 try catch

Ø验证页面是否存在某文字

Ø验证页面是否存在某元素

Ø验证某一个元素是否包含某些文字

Ø验证元素的属性title

Ø验证某个元素对应的value必须是某值

##面试题目

一面是用编程题   中级以上的    java C语言   要刷题  开发还要难一点

二面是  视频和电话  项目实训  遇到的问题  怎么解决的问题；

#

### 1、你在项目中如何使用自动化测试工具？



### 2、如何解决遇到的问题？



### 3、定位方式有哪一种？你是如何选择的？



### 4、如何处理动态元素？



id是变化的  要怎么办

### 5、自动化测试的流程是什么？



分析测试需求  哪些适合UI自动化测试，哪些不适合。

1）分析测试需求

2）选择方案

3）制定测试计划

4）环境搭建

5）用例准备

6）编码

7）分析结果

（答案看下一期）