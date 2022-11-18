# selecting-web-elements
A bunch of tables about selecting Web things
##### Locators
``` csharp
this.driver.FindElement(By.ClassName("className"));
this.driver.FindElement(By.CssSelector("css"));
this.driver.FindElement(By.Id("id"));
this.driver.FindElement(By.LinkText("text"));
this.driver.FindElement(By.Name("name"));
this.driver.FindElement(By.PartialLinkText("pText"));
this.driver.FindElement(By.TagName("input"));
this.driver.FindElement(By.XPath("//*[@id='editor']"));
// Find multiple elements
IReadOnlyCollection<IWebElement> anchors =
this.driver.FindElements(By.TagName("a"));
// Search for an element inside another
var div = this.driver.FindElement(By.TagName("div"))
.FindElement(By.TagName("a"));
```
##### Basic browser operations
``` csharp
// Navigate to a page
this.driver.Navigate().GoToUrl(@"http://google.com");
// Get the title of the page
string title = this.driver.Title;
// Get the current URL
string url = this.driver.Url;
// Get the current page HTML source
string html = this.driver.PageSource;
```
##### Basic elements operations
``` csharp
IWebElement element = driver.FindElement(By.Id("id"));
element.Click();
element.SendKeys("someText");
element.Clear();
element.Submit();
string innerText = element.Text;
bool isEnabled = element.Enabled;
bool isDisplayed = element.Displayed;
bool isSelected = element.Selected;
IWebElement element = driver.FindElement(By.Id("id"));
// Select element
SelectElement select = new SelectElement(element);
select.SelectByIndex(1);
select.SelectByText("Ford");
select.SelectByValue("ford");
select.DeselectAll();
select.DeselectByIndex(1);
select.DeselectByText("Ford");
select.DeselectByValue("ford");
IWebElement selectedOption = select.SelectedOption;
IList<IWebElement> allSelected = select.AllSelectedOptions;
bool isMultipleSelect = select.IsMultiple;
```
##### Advanced driver operations
``` csharp
// Drag and Drop
IWebElement element = driver.FindElement(
By.XPath("//*[@id='project']/p[1]/div/div[2]"));
Actions move = new Actions(driver);
move.DragAndDropToOffset(element, 30, 0).Perform();
// How to check if an element is visible
Assert.IsTrue(driver.FindElement(
By.XPath("//*[@id='tve_editor']/div")).Displayed);
// Upload a file
IWebElement element =
driver.FindElement(By.Id("RadUpload1file0"));
String filePath = @"D:\WebDriver.Series.Tests\\WebDriver.xml";
element.SendKeys(filePath);
// Scroll focus to control
IWebElement link =
driver.FindElement(By.PartialLinkText("Previous post"));
string js = string.Format("window.scroll(0, {0});",
link.Location.Y);
((IJavaScriptExecutor)driver).ExecuteScript(js);
link.Click();
// Taking an element screenshot
IWebElement element =
driver.FindElement(By.XPath("//*[@id='tve_editor']/div"));
var cropArea = new Rectangle(element.Location, element.Size);
var bitmap = bmpScreen.Clone(cropArea, bmpScreen.PixelFormat);
bitmap.Save(fileName);
// Focus on a control
IWebElement link =
driver.FindElement(By.PartialLinkText("Previous post"));
// Wait for visibility of an element
WebDriverWait wait = new WebDriverWait(driver,
TimeSpan.FromSeconds(30));
wait.Until(ExpectedConditions.VisibilityOfAllElementsLocatedBy(
By.XPath("//*[@id='tve_editor']/div[2]/div[2]/div/div")));
```
#### CSS Selectors
##### Element Selectors
| LOCATOR       | EXPLANATION                                      |
|---------------|--------------------------------------------------|
| `ul#myId`       | \<ul> element with @id= 'myId'                    |
| `#myUniqueId`   | any element with @id='myId'                      |
| `ul.myForm`     | \<ul> element with @class = 'myForm'              |
| `.myForm.front` | any element with @classes = 'myform' and 'front' |
##### CSS Attribute Selectors
| LOCATOR                                         | EXPLANATION                                                                 |
|-------------------------------------------------|-----------------------------------------------------------------------------|
| `ul[name = "automateName"][style = "style_name"]` | \<ul> element with attributes @name =‘automateName’ and @style= 'style name' |
| `ul[@id]`                                         | elements with @id attribute                                                 |
| `ul[id = "myId"]`                                 | \<ul > element with @id='myId'                                               |
| `*[name='N'][value= 'v']`                        | elements with name N and specified value 'v'                                |
| `ul[id ^= "my"]`                                  | all elements with an attribute beginning with 'my'                          |
| `ul[id$= "Id"]`                                   | all elements with an attribute ending with 'Id'                             |
| `ul[id *= "unique"]`                              | all elements with an attribute containing the substring 'unique'            |
| `ul[id ~= "unique"]`                              | all elements with an attribute containing the word 'unique'                 |
| `a[href='url']`                                   | anchor with target link 'url'                                               |
##### Pseudo-class selectors applying to siblings
| LOCATOR                                   | EXPLANATION                                                              |
|-------------------------------------------|--------------------------------------------------------------------------|
| `ul#myUniqueId li:first-child`              | first child element                                                      |
| `ul#myUniqueId li:nth-of-type(1)`           | first child element                                                      |
| `ul#myUniqueId li:last-child`               | last child element                                                       |
| `ul#myUniqueId li:nth-of-type(3)`           | last child element                                                       |
| `#TestTable tr:nth-child(3)td:nth-child(2)` | cell by row and column (e.g. 3rd row, 2nd column)                        |
| `p::first-letter`                           | selects the first letter of the specified \<p>                            |
| `p::first-line`                             | selects the first line of the specified \<p>                              |
| `p:first-of-type`                           | selects any \<p> that is the first element of its type among its siblings |
| `p:last-of-type`                            | selects any \<p> that is the last element of its type among its siblings  |

##### Contextual selectors
| LOCATOR                | EXPLANATION                                                                                      |
|------------------------|--------------------------------------------------------------------------------------------------|
| `ul#myUniqueId > li`   | direct child element li                                                                            |
| `ul#myUniqueId li`     | sub child element                                                                                |
| `div > p`              | elements \<p> that are a direct descendant of a \<div> element                                         |
| `div + p`              | elements \<p> that are the next sibling of a \<div> element (i.e. placed directly after)               |
| `div ~p`       | elements \<p> that follow, and are siblings of \<div> elements                                         |
| `form myForm.front + ul` | next sibling                                                                                     |
| `div.row *`              | selects all elements that are descendant (or child) of the elements with 'div' tag and 'row' class |
##### Pseudo-class selectors for link and user states
| LOCATOR               | EXPLANATION                                                        |
|-----------------------|--------------------------------------------------------------------|
| `a:visited ul[@id]    ` | all visited links                                                  |
| `a:link               ` | all unvisited links                                                |
| `a:hover              ` | all links on mouse hover                                           |
| `input:active         ` | every active \<input> element                                       |
| `input:disabled       ` | every disabled \<input> element                                     |
| `input:enabled        ` | every enabled \<input> element                                      |
| `input:focus          ` | the \<input> element which has focus                                |
| `input:read-only      ` | \<input> elements with the 'readonly' attribute specified           |
| `input:required       ` | \<input> elements with the 'readonly' attribute specified           |
| `input:checked        ` | checkbox (or radio button) that is checked                         |
| `a:contains('Log Out')` | anchor with inner text containing 'Log Out'                        |
| `td:contains('t') ~ td` | cell immediately following cell containing 't'                     |
| `p:lang(language)     ` | all \<p> elements with a @lang attribute equal to 'language '       |
| `:not(p)              ` | selects any element that is NOT a paragraph                        |
| `p:not(.fancy)        ` | \<p> elements that are not in the class '.fancy'                    |
| `p:not(.crazy, .fancy)` | \<p> elements that are not '.crazy' or '.fancy'                             |
| `div:empty            ` | returns all \<div> elements that have no children                   |
| `:root                ` | selects the root element of the document                           |
| `input:in-range       ` | selects all elements with a value that is within a specified range |
| `input:out-of-range   ` | select all elements with a value that is outside a specified range |

#### XPath Selectors
##### Contextual selectors
| LOCATOR         | EXPLANATION                |
|-----------------|----------------------------|
| `//img           `| image element              |
| `//img/*[1]      `| first child of element img |
| `//ul/child::li  `| first child 'li' of 'ul'   |
| `//img[1]        `| first img child            |
| `//img/*[last()] `| last child of element img  |
| `//img[last()]   `| last img child             |
| `//img[last()-1] `| second last img child      |
| `//ul[*]         `| 'ul' that has children     |

##### Attribute selectors
| LOCATOR                            | EXPLANATION                                                   |
|------------------------------------|---------------------------------------------------------------|
| `//img[@id='myId']                 ` | image element with @id= 'myId'                                |
| `//img[@id!='myId']                ` | image elements with @id not equal to 'myId'                   |
| `//img[@name]                      ` | image elements that have name attribute                       |
| `//*[contains(@id, 'Id')]          ` | element with @id containing                                   |
| `//*[starts-with(@id, 'Id')]       ` | element with @id starting with                                |
| `//*[ends-with(@id, 'Id')]         ` | element with @id ending with                                  |
| `//*[matches(@id, 'r')]            ` | element with @id matching regex ‘r’                           |
| `//*[@name='myName']               ` | image element with @name= 'myName'                            |
| `//*[@id='X' or @name='X']         ` | element with @id X or a name X                                |
| `//*[@name="N"][@value="v"]        ` | element with @name N & specified @value ‘v’                   |
| `//*[@name="N" and @value="v"]     ` | element with @name N & specified @value ‘v’                   |
| `//*[@name="N" and not(@value="v")]` | element with @name N & not specified @value ‘v’               |
| `//input[@type="submit"]           ` | input of type submit                                          |
| `//a[@href="url"]                  ` | anchor with target link 'url'                                 |
| `//section[//h1[@id='hi']]         ` | returns \<section> if it has an \<h1> descendant with @id= 'hi' |
| `//*[@id="TestTable"]//tr[3]//td[2]` | cell by row and column                                        |
| `//input[@checked]                 ` | checkbox (or radio button) that is checked                    |
| `//a[@disabled]                    ` | all 'a' elements that are disabled                            |
| `//a[@price > 2.50]                ` | 'a' with price > 2.5                                          |

##### XPath methods
| LOCATOR                               | EXPLANATION                                     |
|---------------------------------------|-------------------------------------------------|
|`//table[count(tr) > 1]               `| return table with more than 1 row               |
|`//*[.="t"]                           `| element containing text 't' exactly             |
|`//a[contains(text(), "Log Out")]     `| anchor with inner text containing 'Log Out'     |
|`//a[not(contains(text(), "Log Out"))]`| anchor with inner text not containing 'Log Out' |
|`//a[not(@disabled)]                  `| all 'a' elements that are not disabled          |
| `//a[contains(normalize-space(), "Log out")]` | anchor with inner text 'Log Out' stripped of leading and trailing white-space and sequences of whitespace replaced by a single space|

##### Axis navigation
| LOCATOR                                      | EXPLANATION                                            |
|----------------------------------------------|--------------------------------------------------------|
| `//td[preceding-sibling::td="t"]              `| cell immediately following cell containing 't' exactly |
| `//td[preceding-sibling::td[contains(.,"t")]] `| cell immediately following cell containing 't'         |
| `//input/following-sibling::a                 `| 'a' following some sibling 'input'                     |
| `//a/following-sibling::*                     `| sibling element immediately following 'a'              |
| `//input/preceding-sibling::a                 `| 'a' preceding some sibling 'input'                     |
| `//input/preceding-sibling::*[1]              `| sibling element immediately preceding 'input'          |
| `//img[@id='MyId']::parent/*                  `| the parent of image with id                            |
