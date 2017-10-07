# Selenium Testing Tools Cookbook - Unmesh Gundecha

---------------------

## Locating Elements

* FindElement method returns a webelement object based on a specific search criteria
* FindElements returns all elements that meet a certain criteria
* The ways you can find elements are:
	* By.id
	* By.name
	* By.class
	* By.tagname
	* By.linkText
	* By.partialLinkText
	* By.CssSelector
	* By.xpath

## Working With Selenium API
* Once you have found an element, selenium provides many methods to interact and manipulate elements
* To get the text from an element, use `.getText()`
* To get a elements attribute, use `.getAttribute("x")` on the element
* To get the CSS value, use `.getCssValue("x")`
* To do actions on a element, use the Actions class

Eg

```
Actions builder = new Actions(driver);
builder.click(element)
builder.keyDown(Keys.CONTROL)
.build().perform

//Note, you must use build().perform() at the end to do the series of actions
```

 * To double click, use `.doubleClick(element)` from the actions class
 * To drag and drop, Use `dragAndDrop(source,target)` in the actions builder class

### Executing Javascript Code

 * To execute javascript code, use a Javascript executor

 Example

 ```

 JavascriptExector js = (JavascriptExecutor) driver;

 String title = (String) js.executeScript ("return document.title");

 ```

### Taking Screenshots

 * Selenium provides the TakeScreenshot interface

 ### Maximizing the Browser Window

 * Use `driver.manage().window().maximize();`

TODO AUTOMATING DROPDOWNS

## Controlling the Test Flow


### Implicit Waits

* You can use implicit waits to wait a specific amount of time.
* This is useful if a element is not in the DOM when you get to that part of the test

Example

```
driver.manage().timeouts().implicitlyWait(10, TimeUnit.SECONDS);
```

### Synchronizing a test with an explicit wait

* Explicit waits wait until a condition is met
* Selenium provides the ExpectedCondition class for this.
* Some of the methods provided are:
	* `elementToBeClickable(x)`
	* `elementToBeSelected(x)`
	* `presenceOfElementLocated(x)`
	* `textTobePresentInElement(x,y)`
	* `titleContains(x)`
* Explicit waits work by checking the object every 500ms to see whether the operation can be before
* You also need to specify when after a certain period of polling, selenium should stop

Example

```
WebDriverWait wait = new WebDriverWait(driver, 10);
wait.until(ExpectedConditions.titleContains("selenium"));
```

### Synchronizing a test with custom-expected conditions

* Can use the ExpectedConditions class on our explicit waits to check for our own custom conditions

Example
```
WebElement message = (new WebDriverWait(driver, 5)). until( new ExpectedCondition<WebElement>(){
@Override
public WebElement apply (WebDriver d) {
return d.findElement(By.id("page4"));
}});
```

#### Waiting for Elements Attribute Value To Update

```

(new WebDriverWait(driver, 10)).until(new ExpectedCondition<Boolean>()

```

#### Waiting for an Elements Visibility

```
 (new WebDriverWait(driver, 10)).until(new ExpectedCondition<Boolean>()

```

#### Waiting for Dom Events

```
(new WebDriverWait(driver, 10)).until(new ExpectedCondition<Boolean>()

```

### Checking Elements Presence


* Use `isElementPresent(x)`

### Checking Elements Status

* Use one of the following:
	* `isEnabled()`
	* `isSelected()`
	* `isDisplayed()`

### Identifying and Handling Pop windows By Name

* You can switch between windows using `webdriver.switchTo().window(Windowtitle)`
* The window title should be the windows Name Attribute value

### Identifying and Handlign Pop windows By Title

```
   @Test

```

### Identifying and Handling a pop-up window by its content

```
@Test
    //Find the Close Button on Chat Popup Window and


```

### Handling Javascript Alerts

* WebDriver provides the Alert class for handling alerts

```
		try {
            } catch (NoAlertPresentException e) {

```

* driver.switchTo.alert() will throw a NoAlertPresentException if the alert is not present

### Handling A Confirmation Box Alter

* These boxes provide the options to press OK or Cancel
* The OK button returns true, and the cancel button return false

Example

```
@Test
public void testConfirmAccept()
{
	WebElement button = driver.findElement(By.id("confirm"));
	button.click()

		try{

			Alert alert = driver.switchTo().alert();
			alert.accept()

			WebElement message = driver.findElement(By.id("demo"));
			assertEquals("Alert message", message.getText();

			} catch (NoAlertPresentException e) {
			e.printStackTrade();
			}
		}
	}

```

### Handling A Promt Box Alert

* These are alerts where the user needs to add text to the box
* Has Ok and cancel buttons.
* Ok returns the input value, cancel returns null

Example

```
@Test
public void testPrompt() {
	WebElement button = driver. findElement(By.id("prompt"));
	//Click to open the alert
	button.click();

	try {
		Alert alert = driver.switchTo().alert();
		//Send value into the input box on the alert
		alert.sendKeys("Foo")

		WebElement message = driver.findElement(By.id("prompt_demo"));
		assertEquals("Hello!", message.getText());

		} catch (NoAlertPResentException e) {
		e.printStackTrade();
		}
	}

```

---------
## The Page Object Model

* Test code needs to be treated as production code, and be required to have similar standard and patterns.
* The Page Object model pattern helps in reducing duplication, and making the tests easier to maintain.
* We develop a class that serves as an interface to a web page in the application. This models the web pages properties and behaviours
* Tests should use objects of the page at a high level, where any change in layout or attributes used for the fields in the page should not break the test

### Using the PageFactory class for exposing elements from  a page

* Before exposing elements of the page, you need to:
	* Identify locators that will be needed
	* Define the structure of the page & classes for the objects of the page

#### The example

Following example is implemented a page object test for a BMI Calulator page

1. Define and create all the page objects in a locial grouping
2. Create a new Java Class which has all the elments of the page
3. Define the classes constructor which calles `PageFactory.initEelemnts();` This method should map the elements on the page to the variables in the BmiCalcPage Class
4. Then create a test

```
public class BmiCalcPage {

	public WebElement heightCMS;
	public WebElement weightKg;
	public WebElement Calculate;
	public WebElement bmi;
	public WebElement bmi_category

	public BmiCalcPage(WebDriver driver) {
		PageFactory.initElements(driver, this);
		}

```


```
public void BmiCalculatorTests {

@Test
public void BmiCalculatorTests {

@Test
public void testBmiCalculation() {

	WebDriver driver = new ChromeDriver();
	driver.get(...)
	BmiCalcPage bmiCalcPage = new BmiCalcPage(driver);

	bmiCalcPage.heightCMS.sendKeys("181");
	bmiCalcPAge.weightKg.sendKeys("80");

	bmiCalcPage.Calculate.click()

	assertEquals(somestuff)

	driver.close()

```

#### FindBy Annotations

* This can be used to find elements

```
@FindBy(id = "heightCMS")
public WebElement heightField;
```

#### CacheLookUp Attribute

* If you know that the element will not change. You can find the element once then store it in a cache.
* This is good as FindBy finds the element again each time the element is used.

```
@FindBy(id = "heightCMS")
@CacheLookup
public WebElement heightField;
```

### Using the PageFactory Class for exposing an operation on a page


Example

```

       public BmiCalcPage() {

//load the page
 public void load() {
   //close the page

       }

```

```
package seleniumcookbook.tests;
 public void testBmiCalculation()

```

####Using the LoadableComponent Class

```
package seleniumcookbook.tests.pageobjects;

   private String url = "http://dl.dropbox.com/u/55228056/

```

```
import org.junit.Test;

    //Verify Bmi & Bmi Category values
```

#### Implemented Nested Page Object Intances

1. Create a borwse rclass - This wil rpovide static and shared WebDriver instance for all pages
2. Create HomePage class. This will allow naviagtion on the home page
3. Create search class. This provides all pageObjectModel objects for searching
4. Create SearchResult class, which is nested in the search class. This represents result page
5. create a suitable test

```
  import org.openqa.selenium.WebDriver;
```

```
   import org.openqa.selenium.support.PageFactory;
```

```
import org.openqa.selenium.support.FindBy;
```

```
package demo.magentocommerce.pages;
    }

 ```

 ```
 import org.junit.Test;
public class SearchTest {


```

## Extending Selenium


###Creating an Extension Class for Web Tables

* Currently there is no build in support for web tables or table elements

Example

* Create a consutrcotr and create setter and getters
```
import org.openqa.selenium.WebElement;

* Add methods to get the rows and collumns of a table

```
  public int getRowCount() {
```
* Add a method to retrieve data from a specific cell

```
public WebElement getCellEditor(int rowIdx, int colIdx, int
```

* Create method to retrieve the cell editor element.

```
 public WebElement getCellEditor(int rowIdx, int colIdx, int

   List<WebElement> tableCols = currentRow.findElements(By.

```

* create a test to test it out

```
import org.openqa.selenium.WebDriver;
                 //Verify that specified value exists in second cell of
```

### Setting Attribute Values

```

```

### Highlighting Elements

```
import org.openqa.selenium.JavascriptExecutor;
           element, "");
           }
           }
           }

```

* Using the JSExecutor, we can set the style to a different color (This will help us greatly when debugging)

### Capturing Screenshots of element sin the Selenium WebDriver

* TakeScreenshot interface captures screenshots of the entire page.

```
ublic static File captureElementBitmap(WebElement element) throws

   WrapsDriver= wrapsDriver = (WrapsDriver) element;

   //Get the entire Screenshot from the driver of passed WebElement
```

* YOu can then call WebElementExtender.CaptureElementBItmap(Element, filelocation);


### Comparing Elements in Selenum

* Selenium does not have features for comparing images. We have to make this ourself

```
import java.awt.Image;

           System.out.println(baseImageGrab.getWidth() +  "<>" +

```

* The above class uses PixelGrabber to compare pixels from each image (It puts them in an array and compares them that way
* They are testing firstly for size missmatch, then pixel mismatch

Example test:

```

       File screenshotFile = ((TakesScreenshot) driver).getScreenshotAs(OutputType.FILE);

       FileUtils.copyFile(screenshotFile, new File(scrFile));
```

## Testing On Mobile Browsers

TODO


## Client-side Performance Testing


### Measuring The Response Time using a timer

* This can be implemented using Date/Time Classes

Example
```
// Get the Start Time
```

### Using the BrowserMob proxy for measuring perormance

* Captures performance data from a web application using HTML Archive (HAR) formzt
* Can also manipulate browser behaviour and trafic

Steps:

* Launch the browerMob Proxy server
```
ProxyServer server = new ProxyServer(9090);
server.start()
```
* Create an object of SeleniumProxy and DesiredCapabilities

```
Proxy proxy = server.seleniumProxy();

DesiredCapabilties capabilities = new DesiredCapabilities();
capabilities.setCapability(CapabilityType.PROXY, proxy);
```
* Create and launch a browser instance with the proxy

```
WebDriver driver = new FirefoxDriver(capabilities);
```
* Create a new HAR file and navigate to the url

```
server.newHar("bmiCalculator");
```
* Implement the test steps
```
// Open the BMI Calculator Application
       weight.sendKeys("80");
```

* Collect the performance data from the proxy

```
// Get the HAR data
```

## Testing HTML5 Web Applications


### Automating the HTML5 Video Player

* Need to test the `<video>` element
* * Can use `play()` and `pause()` in an javascript executor to start and stop the video

Example

```
@Test

       assertEquals(25, duration);

       //Play the Video
       jsExecutor.exectureScript("return arguments[0].play()", VideoPlayer);

       Thread.sleep(5000);

       //Pause the video

```

### Automating Interaction on the HTML5 Canvas Element

* The `<Canvas>` element is used for drawing and charting applications by using javascript.
* `<canvas>` has several methods, such as:
	* ` paths`
	* `boxes`
	* `circles`
	* `characters`
	* `images`

Example

```
@Test

     Actions builder = new Actions(driver);
				moveByOffset(-10,-50).

 //Get a screenshot of Canvas element after Drawing and
```

* We first select the pencil option from the drawing tool drop down
* We then draw the shape
* We finally capture the element and compare with a base line image to assert that the image drawn is correct


### Web Storage - Testing Local Storage

* HTML5 provides a `localstorage` interface through javascript
* This interface stores data indefinatly
* In Chrome, you can see this data by click on Inspect Element in the resources tab


Example

```
@Test
```

### Web Storage - Testing Session Storage

* This is similar to local storage, however this only stores data for one session (Until the window is closed)
* We use the `sessionStorage` interface through Javascript to get this information

Example

```
@Test

     WebElement clicksField = driver.findElement(By.id("clicks"));
```


### Cleaning Local and Session Storage

* When running mutliple tests in batch on the same environment, it is advised to clean the the storage before each test


For Local Storage:
```


For Session Storage:

```
 //To Remove a specific Key along with it's value
```

Clearing all items for local storage

```
 //To Clear Storage values
```

Clearing all items for session storage

```
 //To Clear Storage values
```


## Recording Videos of Tests

### Recording Videos of Tests Using Monte Media Library IN Java

* Monte Media Library provides a class called `ScreenRecorder` to record movies of tests

To create a recorder You need the following:

* A setup() method to start the screen recorder
* A test to a record
* A tearDown() method to stop the recorder


Example

```
import org.openqa.selenium.firefox.FirefoxDriver;
      import java.awt.*;
```


```
@Before

    driver = new FirefoxDriver();

```

 @Test
```

```
         @After
```