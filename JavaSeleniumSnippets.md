# Java Selenium Code Snippets

### To Set Chrome Driver Use This Method

```java

 public static WebDriver setChromeDriver() 
	{	
		WebDriver driver = null;
		ChromeOptions options=null;
		try
		{
			options = new ChromeOptions();
			//options.addArguments("--headless");
			options.addArguments("--disable-gpu");
			options.addArguments("--allow-insecure-localhost");
			options.addArguments("--allow-running-insecure-content");
			options.addArguments("--ignore-certificate-errors");
			options.addArguments("--no-sandbox");
			options.addArguments("--disable-notifications");
			//options.addArguments("--start-maximized");
			options.addArguments("--window-size=2000,6000");
			options.setCapability("acceptSslCerts", true);
			options.setCapability("acceptInsecureCerts", true);	
      //chnage chrome driver based on current version
			System.setProperty("webdriver.chrome.driver","C:\\Users\\OmPrakashPeddamadtha\\Downloads\\chromedriver_win32\\chromedriver.exe");

			driver = new ChromeDriver(options);
			driver.manage().window();

		}
		catch(Exception e)
		{
			e.printStackTrace();
		}
		return driver;
	}
 ```
 ### To Check Element is Present or not
 
 ```java
 public static boolean isElementPresent(WebDriver driver ,By selector)
	{
		return driver.findElements(selector).size()>0;
	}

  ````
 ### To Scroll To The Bottom
```java
	public static void scrollBottom(WebDriver driver) {
		JavascriptExecutor jse = (JavascriptExecutor) driver;
		//Below code will scroll the web page till end
		jse.executeScript("window.scrollTo(0, document.body.scrollHeight)");
	}
  ```

 ### To Scroll By Visibility
```java
	public static void scrollByVisibility(WebDriver driver) {
		JavascriptExecutor jse = (JavascriptExecutor) driver;

		WebElement el = driver.findElement(By.linkText("About Us"));	
		jse.executeScript("arguments[0].scrollIntoView();", el);
	}
```

 ### To Scroll By Pixels
```java
	public static void scrollByPixels(WebDriver driver) {
		JavascriptExecutor jse = (JavascriptExecutor) driver;
		// Below code will scroll up the page by  900 pixel vertical	
		jse.executeScript("window.scrollBy(0,200)");
	}
```

 ### To Login to linkedin website
```java
public static WebDriver loginLinkedinWebsite(WebDriver driver) 
	{
		String loginUrl = "https://www.linkedin.com/login";				
		try
		{
			driver.get(loginUrl);
			driver.findElement(By.cssSelector("input[name='session_key']")).sendKeys("xyz@gmail.com");	
			driver.findElement(By.cssSelector("input[name='session_password']")).sendKeys("xyz");					
			driver.findElement(By.cssSelector("button[aria-label='Sign in']")).click();	
			driver.manage().timeouts().implicitlyWait(10, TimeUnit.SECONDS);   
		}
		catch(Exception e)
		{
			e.printStackTrace();
		}
		return driver;
	}
```

 ### To Get Specific Page
```java
	public static  WebDriver getSpecificPage(WebDriver driver,String url)
	{	
		try{
			driver.navigate().to(url);
		}
		catch(Exception e){
			e.printStackTrace();
		}
		return driver;
	}
	
```

 
 
 
 
