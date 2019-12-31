## Automated Tests with Selenium

Getting Started in Visual Studio
Create a new Test project called PortfolioTestProject. 
In this demo we're going to be using NUnit . net, but you could also be using xUnit or MSTest. 
- Install the Selenium. WebDriver package, and this will give us access to the WebDriver API, and allow us to automate the browser. 
- Install the Selenium. Support package
- Install the Selenium.Firefox.Webdriver NuGet package. This will copy geckodriver.exe to the bin folder.
- Install the Selenium.Chrome.Webdriver NuGet package. This will copy geckodriver.exe to the bin folder.
- Install the DotNetSeleniumExtras.WaitHelpers NuGet package.

## Creating the First Test

```C#

        [Test]
        public void InitialLoad()
        {
            using (IWebDriver driver = new FirefoxDriver())
            {               
                driver.Manage().Window.Maximize();
                driver.Navigate().GoToUrl("http://www.judith-herrera.com/");
                Assert.AreEqual("Judith's Portfolio", driver.Title);
            }
        }
```
To decouple the UI from our automation code, a page model will be created. 

## Using a model

```C#
   public class PorfolioPage
    {
        public readonly IWebDriver _driver;
        private const string PageUri = @"http://www.judith-herrera.com/";

        public PorfolioPage(IWebDriver driver)
        {
            _driver = driver;
        }

        public static PorfolioPage NavigateTo(IWebDriver driver)
        {
            driver.Manage().Window.Maximize();
            driver.Navigate().GoToUrl(PageUri);
            return new PorfolioPage(driver);
        }
    }

        [Test]
        public void InitialLoad()
        {
            using (IWebDriver driver = new ChromeDriver())
            {
                var model = PorfolioPage.NavigateTo(driver);
                Assert.AreEqual("Judith's Portfolio", model.Title);
            }
        }
```
## Populating Form Inputs

```C#
     public string Email
        {
            set
            {
                _driver.FindElement(By.Id("email")).SendKeys(value);
            }
        }
```
## Wait for element to be clickable
```C#
   new WebDriverWait(_driver, TimeSpan.FromSeconds(20)).Until(ExpectedConditions.ElementToBeClickable(By.ClassName("mainmenu-icons"))).Click();
```
## Populating Form Inputs

```C#
     public string Email
        {
            set
            {
                _driver.FindElement(By.Id("email")).SendKeys(value);
            }
        }
```

