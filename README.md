# Robot-Framework
# 1. Introduction to Robot Framework
This document is for understanding terminologies related to Robot framework and installation instructions with a use case for Jio.com test automation using Robot-framework.

## 1.1. What is Robot Framework?
Robot framework is a generic open-source automation framework for acceptance testing, acceptance test-driven development, and robotic process automation (RPA).

It uses the keyword-driven testing technique approach. 

Provides test libraries implemented either with python or Java and users can create new high level keywords from existing ones using the same syntax that is used for creating test cases.

In short, it is a python based, extensible, keyword-driven, open source automation framework.

![Image](Screenshot/Robotframeworkarchitecture.png)


## 1.2. Why Robot Framework?
- It is platform and application independent.  
- Easy to install, As it is built over python all dependencies & libraries can be installed using pip.

[comment]: < Supports all platform including Web (Windows, Mac*), Mobile (Android, iOS*), Desktop (Using additional libraries) >

- External libraries that can be used to automate applications on various clients
    | Client             | Library                  |
    |:-------------------| :----------------------: |
    | WEB                | SeleniumLibrary (Python) |
    | DESKTOP (Windows)  | RPA.Windows              |
    | MOBILE             | AppiumLibrary            |

- It is Keyword-driven which means there are many built-in keywords provided by libraries, for example Selenium for Web-Browser, Appium for Mobile, RPA library for Desktop and Windows Automation they are easily available on their site.  
- Supports user-defined keywords. 
- Provides logs and reports in HTML format to identify test failure messages and warning along with screenshots.
- Active community, we can easily get the solution of any error or issue over their forums.  
- Allows tags, test cases that come handy while trying to run either of the Smoke Test Cases, Regression Test Cases, System Test Cases, etc. 

#
### 1.2.1. Advantages of Robot Framework
1. Free and Open-source
2. Easy to Learn and Implement
3. Easy to integrate with CI/CD
4. Supports multiple external libraries

### 1.2.2. Disadvantages
1. Hard to maintain Test suites.
2. Difficult to customize Test Results.
3. Difficult to handle parallel execution (They are few libraries, yet still complex to handle parallel execution)
4. Strict indentation rules while scripting.
5. Scripts get complex when Nested-Loops are used.
#

# 2. Jio.com Test Automation
## 2.1. Manual  vs Automation Flow
### 2.1.1. Manual Test Flow
- Opening and navigating the Jio.com website manually.
- Validate accessibility, functionality, and responsiveness.
- Performing actions like clicking links and buttons.
- Verify UI & content presence and correctness.
- Test specific features and functionalities.
- Document issues, bugs, and provide feedback.

### 2.1.2. Automation Test Flow
Robot Framework Automation Flow:
- Install required libraries & Set up the Test environment.
- Automate navigation, accessibility, and functionality testing.
- Automate the content validation on the website.
- Automate specific feature and functionality testing.
- Generate test reports and logs for documentation.
- Analyze results and investigate failures.
- Provide feedback and suggestions for improvements.
#

## 2.2. Advantages of Automation for Jio.com

- Re-usable test cases and keywords can be created, enabling efficient test case development and reducing duplication of effort. Once automated, test cases can be reused for regression testing or testing different scenarios.

- Repetitive journeys can be easily performed by creating a keyword(s) that can be called in any other page. (Eg: Pack page UI & Content verifications, multiple redirections of recharge journeys)

- Logs and Test Reports are customizable, artifacts are embedded with the reports which can be shared with stakeholders/ product/ Development team.

- Test suites are easily configurable as per requirement and can be executed on multiple browsers. Supports parallel test execution, allowing multiple tests to run concurrently.


## 2.3. Limitations of Automation for Jio.com

- UI understanding and Visual Verification is challenging as robot framework (Selenium) relies on locators to interact with web elements.

- Contextual understanding and Test Data handling is difficult as the context and intent behind certain actions cannot be processed by automation flow and creating test data for certain scenarios is very difficult.

- Automated tests required regular maintenance as web site undergoes changes on daily basis.

- Requires skills to handle the test scripts. Installation and setup of libraries, environment and learning curve is time-consuming for teams transitioning from manual testing.


# 3. Robot Project Structure

## 3.1. File Structure
The file structure of a `.robot` file in Robot Framework typically consists of the following sections:

```
Project-Title/
├── Resources
├── Tests
└── Results
```
### 3.1.1 Resources
<p> Resources consists of files that are called in any other robot file. </p>

### 3.1.2. Tests
<p> Consists of Test Suites. </p>

### 3.1.3. Results
<p> Consist of all output files that are generated post test execution.</p>

#


- __Settings__ : The `Settings` section in Robot Framework is where you configure the test suite and define its behavior. It includes importing resources, setting up libraries, and configuring variables. It allows customization of the test environment and execution flow.

- __Variables__ : The `Variables` section in Robot Framework is used to define and manage variables that can be used across test cases. It allows storing and accessing data dynamically, enabling flexibility in test case execution and data-driven testing. Variables can be set directly in the test file or imported from external sources like YAML files.

- __Keywords__ : The `Keywords` section in Robot Framework holds reusable sets of steps or actions that perform specific tasks. They can be built-in, provided by libraries, or user-defined. Keywords make test scripts more organized, reusable, and readable, improving code maintenance and test automation efficiency.

- __Test Case__ : The `Test Case` section in Robot Framework is where you define the individual test cases or scenarios. Each test case consists of a series of steps or keywords that define the actions to be performed and the expected outcomes. Test cases can be organized hierarchically and can include assertions, variable assignments, and other keywords to verify the behavior of the system under test.



```bash

                              *** Settings ***
Library                         SeleniumLibrary


                              *** Variables ***
  ${URL}          www.jio.com
  ${Browser}      chrome


                              *** Keywords ***
Load jio.com
    Go To     ${URL}
    Sleep     3s
    Take Page Screenshot

                              *** Test Cases ***

Visit jio.com
    [Documentation]   open chrome and visit jio.com
    Open Browser                about:blank    ${Browser}
    Maximize Browser Window
    Load jio.com
    Close Browser

```

<!---
```bash
                              *** Settings ***  minimize this code sinppet as per example of above 4 sections
                            
Library                         SeleniumLibrary

Resource                        ../../ResourcesFiberFiberDiscover_Page.robot
Resource                        ../../Resources/Common.robot
Resource                        ../../Resources/Local_Keywords.robot
Resource                        ../../Resources/Excel_Activity.robot
Resource                        ../../Resources/Tags.robot

Variables                       ../Fiber/FDiscover_Variables.yaml
Variables                       ../../Resources/Tester_Input.yaml

Suite Setup                     Fiber Discover Tag Activity

Test Setup                      Begin Web Test
Test Teardown                   End Web Test

                              *** Variables ***

*** Test Cases ***
Example
    Keyword    &{DICT}       named=arg
    Keyword    positional    @{LIST}       &{DICT}
    Keyword    &{DICT}       &{ANOTHER}    &{ONE MORE}

                              *** Keywords ***

Landing on Fiber Discover Page
    Visit Jio.com And Goto                                  ${Fiber}    
    URL Validation                                          ${Sub_NavBar_URLs[${0}]}
    Wait Until Keyword Succeeds                             30 sec    0 sec     CSS Verification                                           xpath:(//li[@onclick="window.location.href='/fiber'"])[2]    border-bottom-color    rgba(232, 232, 252, 1)
    Set Screenshot Directory                                ./Results/Fiber/Fiber/Fiber_Discover_Screenshots/
    Reading Data of Discover Page from Excel
    Take Page Screenshot                                    FiberPage_Loaded

                              *** Test Case ***

Confirm UI, Content and Redirection of Easy recharge and Pay bills section
    [Documentation]    TC08 to TC18                             #Details about the Test case
    [Tags]             ${TC08-FDiscover}    TC08-FDiscover      #Tags can be used to run Test cases  
    Easy recharge and Pay bills section Verification            #User Keyword
```

`Tip: (Ctrl+Click to go to its definition)` 
-->


# 

## 3.2. Settings

Settings section is used for importing test libraries, resource file and defining Meta data for test suites and test case.

We can have following sections under settings section:

- __DOCUMENTATION:__ We can write some information about our suite.

- __LIBRARY:__ We need to import libraries which we need further in our test script. For e.g. SeleniumLibrary

- __RESOURCE:__ We need to import resources which we need further in our test script.
For e.g. PATH to: module ‘page.robot’, ‘Common.robot’

- __VARIABLES:__ It is used to import the ‘.yaml’ file which contains global variables for the Test case.

- __Test Setup:__ This is a set of keywords or instruction that will execute multiple times before starting every test case. 

- __Test Teardown:__ This is a set of keywords or instruction that will execute multiple times after ending every test.

- __Suite Setup:__ This keyword will execute only one time, before executing the test case.

- __Suite Teardown:__
This keyword will execute only one time, after executing the Test case.

## 3.3. Variables
Variables are an integral feature of Robot Framework, and they can be used in most places in test data. Most commonly, they are used in arguments for keywords in Test Case and Keyword sections, but also all settings allow variables in their values. A normal keyword name cannot be specified with a variable, but the BuiltIn keyword Run Keyword can be used to get the same effect.

### 3.3.1. Variables are useful, for example, in these cases:

- When strings change often in the test data. With variables you only need to make these changes in one place.
- When there is a need to have objects other than strings as arguments for keywords. This is not possible without variables.
- When different keywords, even in different test libraries, need to communicate. You can assign a return value from one keyword to a variable and pass it as an argument to another.
- When values in the test data are long or otherwise complicated. For example, ${URL} is shorter than http://long.domain.name:8080/path/to/service?foo=1&bar=2&zap=42.

Robot Framework variables, similarly as keywords, are case-insensitive, and also spaces and underscores are ignored. However, it is recommended to use capital letters with global variables (for example, `${PATH}` or `${TWO WORDS}`) and small letters with local variables that are only available in certain test cases or user keywords (for example, `${my var}`). Much more importantly, though, case should be used consistently.


### 3.3.2. Scalar Variable
The most common way to use variables in Robot Framework test data is using the scalar variable syntax:  ${var}.  
 
### 3.3.3. Syntax  

The simplest possible variable assignment is setting a string into a scalar variable. This is done by giving the variable name (`including ${}`) in the first column of the Variable section and the value in the second one. 

If the second column is empty, an empty string is set as a value. 
```bash  
                    *** Variable ***
${Name}       RobotFramework
```
We can use the equals sign = after the variable name to make assigning variables.
```bash  
                    *** Variable ***
${Name}=       RobotFramework
```

  
### 3.3.4. List Variable

In this case the list is expanded and individual items are passed in as separate arguments. Robot Framework stores its own variables in one internal storage and allows using them as scalars, lists or dictionaries. Using a variable as a list requires its value to be a Python list or list-like object.   
Creating list variables is as easy as creating scalar variables. Again, the variable name is in the first column of the Variable section and values in the subsequent columns. A list variable can have any number of values, starting from zero, and if many values are needed, they can be split into several rows.  

```bash
        ***Variable***
@{Count}      one two three
```
### 3.3.5. Dictionary Variable

A variable containing a Python dictionary or a dictionary-like object can be used as a dictionary variable like &{EXAMPLE}. In practice this means that the dictionary is expanded and individual items are passed as named arguments to the keyword.   

```bash
         ***Variable***
&{USER1}    name=xyz    address=AddToCartBTN
```

Items need to be created using NAME=VALUE syntax or existing dictionary variables. If there are multiple items with same name, the last value has precedence. If a name contains a literal equal sign, it can be escaped with a backslash like `\=`.  

Values of these dictionaries can be accessed like attributes, which means that it is possible to use extended variable syntax like `${VAR.key}`.For example, individual value `& {USER}[name]`  can also be accessed like `${USER.name}`.


## 3.4. Keywords

Keywords can be predefined, user defined.
In Robot Framework, test cases are constructed in test case tables using keywords. There are two types of keywords used in robot :

- __Library/ Built In Keyword__ 

- __User defined or custom keyword__
 
### 3.4.1. Library or Built In keyword  

Library Keywords are keywords that come from the library we import in Robot Framework.  
Example: Open Browser, Log, and Should Be Equal etc.  
We can find the keyword in following link:   
https://robotframework.org/robotframework/latest/libraries/BuiltIn.html  

### 3.4.2. User defined or custom keyword  
User-defined keywords can be created to perform a particular action in the test case or it can also be created using the library keywords and built-in keywords in Robot framework. We generally use user defined keywords scripted by our team (Common.robot). 
 
Example: ***Keywords***
```bash

                            ***Keywords***
                            
    Opening Browser And Printing Page Title
    Open Browser        Chrome      www.jio.com
    Log                 Get Title
```
```bash

Landing on Fiber Discover Page
    Visit Jio.com And Goto                                  ${Fiber}    
    URL Validation                                          ${Sub_NavBar_URLs[${0}]}
    Wait Until Keyword Succeeds    30 sec    0 sec          CSS Verification    xpath:(//li[@onclick="window.location.href='/fiber'"])[2]    border-bottom-color     rgba(232, 232, 252, 1)
    Set Screenshot Directory                                ./Results/Fiber/Fiber/Fiber_Discover_Screenshots/
    Reading Data of Discover Page from Excel
    Take Page Screenshot                                    FiberPage_Loaded


```
<p>‘Open Browser’ and ‘Log’ are built in keywords, here we defined a custom keyword ‘Opening Browser and Printing Page Title’ using built in keywords. Similarly we can create many custom keywords and can later use in test script.</p>

 
<p>Keywords are used for optimization of the script, so that they can be called wherever required and the script will look optimized.</p>
The keywords are mainly classified as:

1.	Common Keywords
2.	Local Keywords

### 3.4.3. Common Keywords
Common keywords are the user defined keywords which can be used anywhere in the script and are fetched from `Common.robot`.

### 3.4.4. Local Keywords
Local Keywords are the keywords which are used for scripting and are created by the Programmer. They are used for optimization and the entire coding is done inside the keywords and are called in the respective `Page Objects.robot` and `main.robot`.

### 3.4.5. Keyword scope

<p>When only a keyword name is used and there are several keywords with that name, Robot Framework attempts to determine which keyword has the highest priority based on its scope. The keyword's scope is determined on the basis of how the keyword in question is created:</p>

- Created as a user keyword in the currently executed test case file. These keywords have the highest priority and they are always used, even if there are other keywords with the same name elsewhere.

- Created in a resource file and imported either directly or indirectly from another resource file. This is the second-highest priority.

- Created in an external test library. These keywords are used, if there are no user keywords with the same name. However, if there is a keyword with the same name in the standard library, a warning is displayed.
- Created in a standard library. These keywords have the lowest priority.

## 3.5. Test cases
```bash

Confirm Banner carousel is visible
    [Documentation]    TC01 to TC07         (TC02, TC06 Out of Scope)
    [Tags]             ${TC01-FDiscover}    TC01-FDiscover    Sanity    Test
    Carousel Visibility

Confirm UI, Content and Redirection of Easy recharge and Pay bills section
    [Documentation]                TC08 to TC18
    [Tags]    ${TC08-FDiscover}    TC08-FDiscover        
    Easy recharge and Pay bills section Verification

```


### 3.5.1. Tags
Using tags in Robot Framework is a simple, yet powerful mechanism for classifying test cases and also user keywords. Tags are free text and Robot Framework itself has no special meaning for them except for the reserved tags discussed below. Tags can be used at least for the following purposes:

- They are shown in test reports, logs and, of course, in the test data, so they provide metadata to test cases.

- Statistics about test cases (total, passed, failed and skipped) are automatically collected based on them.

- They can be used to include and exclude as well as to skip test case.

## 3.6. Page Object Model

Page Object Model (POM) is a design pattern in which we can maintain the page elements in separate files. There can be multiple pages and every page has a different file and the redirection of these pages can be done through the keywords used in the code. Page Objects is mainly used for Script Optimization. For Example: 

![Image](Screenshot/Pageobjectsoffiber.png)

```bash
        Resources/
        └── FiberPage_PO/
            ├── Discover_PO
            ├── Prepaid_PO
            ├── Postpaid_PO
            ├── Recharge_PO/
            │   ├── RedirectionToRecharge.robot
            │   ├── MobileTab.robot
            │   └── MobileTab.robot  
            ├── GetJioFiber_PO
            ├── PayBills_PO/
            │   ├── RedirectionToPaybills.robot
            │   └── Paybills.robot
            └── Services_PO

```


## 3.7. Library 


We need to import libraries which we need further in our test script. For e.g. SeleniumLibrary.


![Image](Screenshot/Library.png)

- SeleniumLibrary: SeleniumLibrary is primarily used for web application testing. It provides keywords for interacting with web browsers, locating elements, and performing actions like clicking, typing, and verifying page content.

- YAML: YAML stands for YAML Ain't Markup Language, but it originally stood for Yet Another Markup Language.
YAML is a human-readable data serialization language, just like XML and JSON.

- Requestlibrary: Request Library is a Robot Framework library aimed to provide the HTTP API testing functionalities by wrapping the well-known Python Requests Library.

- Dialogs: Dialogs is Robot Framework's standard library that provides means for pausing the test execution and getting input from users. The dialogs are slightly different depending on are tests run on Python or Jython but they provide the same functionality.

- Excellibrary: The Excel library can be used to read and write Excel files without the need to start the actual Excel application.

### 3.7.1. General built-in Libraries

- String Library: String Library in Robot Framework's standard library is for manipulating strings and verifying their contents.

- Collection library: Collection Library provides keywords for handling lists and dictionaries.

### 3.7.2. Dialogs Library
Dialogs library in Robot Framework is used for displaying dialog boxes or pop-up windows during test execution. Dialog boxes can provide useful interaction with the user while running automated tests, allowing you to obtain input or display important information.

To use the requests library in Python, you first need to install it. You can do this by running the following command:


```bash
*** Settings ***

Library    Dialogs

*** Test Cases ***

  Get User Input
      ${Variable_Name}   Keyword to take input from user    Text required to display on dialog box
    
      Log    Entered name: ${Variable_Name}
```
![Image](Screenshot/DialogboxScript.png)
![Image](Screenshot/Dialogboxpopup.png)

The Dialogs library provides keywords that allow you to open different types of dialog boxes, such as information, warning, error, and question dialogs. It also provides options to customize the appearance and behavior of the dialog boxes. By using the Dialogs library, you can enhance the interactivity and usability of your automated tests, making them more flexible and user-friendly.

### 3.7.3. Request Library
The requests library is a popular Python library used for making HTTP requests. It provides an easy-to-use interface to interact with web services and APIs. With the requests library, you can send HTTP requests, handle responses, and perform various operations related to web communication.

To use the requests library in Python, you first need to install it. You can do this by running the following command:
```bash
pip install robotframework-requests
```
The requests library provides various other functions such as post(), put(), delete(), etc., to handle different types of HTTP requests. It also supports authentication, headers, cookies, and more.
```bash
*** Test Cases ***

Send GET Request
    ${response}    Get Request    https://api.example.com/data
    Should Be Equal As Strings    ${response.status_code}    200
    ${response_data}    Get Request Body    ${response}
    Log    ${response_data}
```









## 3.8. YAML 
YAML is a data serialization language with a simple and human-friendly syntax. 

YAML stands for YAML Ain't Markup Language, but it originally stood for Yet Another Markup Language.
YAML is a human-readable data serialization language, just like XML and JSON.

In Automation we use YAML for declaring variables globally which makes automation more easier and readable.
The variables.yaml file is not a built-in feature of Robot Framework. It is a convention used by users to organize and manage variables in YAML format. 

Variable files can also be implemented as [YAML files] (https://yaml.org/).

### 3.8.1. Installation
```bash
pip install pyyaml
```


Using YAML files with Robot Framework requires PyYAML module to be installed. If you have pip_installed, you can install it simply by running pip install pyyaml. 

> YAML variables: 

YAML variable files can be used exactly like normal variable files from the command line using :option:`--variablefile`option, in the Settings section using :setting:`Variables`setting, and dynamically using the :name:`Import Variables`keyword.



![Image](Screenshot/YamlVariableDecleration.png)
![Image](Screenshot/YamlScript.png)
![Image](Screenshot/Fiberyamldecleration.png)
![Image](Screenshot/Fiberyamlcode.png)


YAML variable files must have either :file:`.yaml` or :file:`.yml` extension. Support for the :file:`.yml` extension is new in Robot Framework 3.2.




## 3.9. Excel Library in Robot Framework

Excel Library is a test library that includes keywords for opening, reading, writing, and saving Excel files from the Robot Framework. 

### 3.9.1. Installation:
We have to install the Excel Library into the robot framework by using the pip command as:   
```bash
pip install robotframework-excellib
```
Use pip list command to see the robotframework-excellib is installed or not.
Or Check the library in your python path (Ex.C:\Python27\Lib\site-packages\ExcelLibrary). 
- Import the library in your code (Library  ExcelLibrary) under Settings Section.

### 3.9.2. Documentation for Keywords refer the link below: 
• https://rawgit.com/peterservice-rnd/robotframework-excellib/master/docs/ExcelLibrary.html 



> Initialization :
- __Open Excel Document:__ Opens an Excel file for reading or writing.
- __Set Excel Cell Value:__ Sets the value of a specific cell in an Excel sheet.
- __Get Excel Cell Value:__ Retrieves the value of a specific cell in an Excel sheet.
- __Read Excel Column:__ Reads the data from a specific column in an Excel sheet.
- __Read Excel Cell:__ Reads the data from a particular cell (row & column) in an Excel sheet
- __Write Excel Row:__ Writes a row of data to an Excel sheet.
```bash
Begin Web Test
Open Excel Document filename=Resources/JioWeb.xlsx
Open Browser about:blank ${Browser}
```
### 3.9.3. We are opening the excel document
```bash
Open Excel Document         filename=Resources/fileName.xlsx   doc_id=filename 
```
### 3.9.4. Split String 

The split() method splits a string into a list.

You can specify the separator, default separator is any whitespace.

Split  String  is  used  in  Excel  for  putting  multiple  values  in a  single  variable separated by a unique character/symbol (for eg: “;”) in one excel cell. You can further call the variable by its index specified path.
```bash 
*** Settings ***
Library         ExcelLibrary
Library         String

*** Test Cases ***
Split String and Write to Excel
    ${string}    Set Variable    Hello;World;Robot
    ${split_list}    Split String    ${string}    ;
    Open Excel Document    path/to/your/excel/file.xlsx
    Write Excel Row     1    ${split_list}    Sheet1
    Close Excel Document
```
In this example, we first import the "String" library from Robot Framework, which provides the "Split String" keyword for splitting a string. We then define a string variable ${string} with the value "Hello;World;Robot".

Next, we use the "Split String" keyword to split `${string}` into a list using the semi-colon (;) as the separator. The resulting list ${split_list} will contain the split values.

After that, we open the Excel file using the "Open Excel Document" keyword from the "ExcelLibrary". Then, we use the "Write Excel Row" keyword to write the split values to a specific row in Sheet1 of the Excel file.

Finally, we close the Excel file using the "Close Excel Document" keyword.

Remember to replace "path/to/your/excel/file.xlsx" with the actual path to your Excel file.

![Image](Screenshot/Excelsheetfiber.png)
![Image](Screenshot/Fiberexcelsplit.string.png)
![Image](Screenshot/ExcelcactivityFiber.png)



 Adjust the sheet name and row number in the "Write Excel Row" keyword according to your requirements.This approach allows you to split a string using the String library and then work with the resulting list of values using the "ExcelLibrary" in Robot Framework.




## 3.10. XPath
The Selenium framework lets you interact with the WebElements in the DOM. For realizing the interaction(s), it is important to choose the appropriate locator from the available Selenium web locators. 
There are many options for web locators in Selenium – ID, Name, XPath, LinkText, Partial LinkText, Tag Name, and more. XPath is one of the widely preferred web locators, as it easily traverses through the DOM elements and attributes to identify an object.

- XPath, which stands for XML Path Language, uses ‘path-like’ syntax to identify and navigate through nodes in an XML or HTML document.
- It follows the XSLT(eXtensible Stylesheet Language Transformations) standard, which is predominantly used to navigate WebElements and attributes.

### 3.10.1. Types of XPath
---------------------------------------------------
- Absolute XPath

- Relative XPath 

#### 3.10.1.A **Absolute XPath**
As the name indicates, an absolute XPath contains the complete path from the root element to the desired element. The downside of using absolute XPath is that any changes made in the path of the element, HTML path, etc., results in the failure of the XPath.

The key characteristic of XPath is that it begins with the single forward slash(/) ,which means you can select the element from the root node.

##### Example of Absolute XPath.

```bash
  xpath=(/html/body/div[2]/section[4]/div/div[3]/div[2]/div[3]/button)
```

#### 3.10.1.B **Relative XPath**
In contrast to absolute XPath, a relative XPath starts by referencing the element we want to locate relative to a particular location. This means that the element is positioned relative to its normal position.

 __Relative Xpath is always preferred as it is not a complete path from the root element.__
Since the path of XPath is relative, it starts with a double forward-slash (//). 

- Relative XPath is usually preferred for Selenium Automation testing since it is not a complete path from the root element.
Hence, any changes in the page design or the DOM hierarchy will have a minimal (to no) impact on the existing XPath selector.

##### Example of Relative XPath.

```bash
  xpath=(//button[@class="j-button j-button-size__medium primary w-auto"])
```
### 3.10.2. Xpath Differentiation 
-----------------------------------------------

| Xpath type               | XPath       |
| ---                      | ---         |
| Absolute Xpath           |   ```XPath=/html/body/div[2]/section[4]/div/div[3]/div[2]/div[3]/button ``` |
| Relative Xpath           |```  XPath=(//button[@class="j-button    j-button-size__medium primary w-auto"]) ```|

---------------------

### 3.10.3. **Steps to get Absolute XPath:**
To start with Absolute XPath
- Go to the element you want the XPath for.

- Right click and select Inspect option.
  ![Image](Screenshot/AbsolutePath1.png)
 
-	Right Click on the highlighted section Click on Copy and Select Copy XPath.
 ![Image](Screenshot/AbsolutePath2.png)

  ![Image](Screenshot/AbsolutePath3.png)

  - You now have the absolute XPath for the given element.

> `Xpath generated = /html/body/div[2]/section[4]/div/div[3]/div[2]/div[3]/button`

### 3.10.4. **Steps to write Relative XPath:**

- Open inspect window.  

- Press Ctrl + F to open the search panel of inspect window.


- Write the formula, the attribute should be unique in case of similar attribute for different element and if not we give index.



- The basic format of XPath in selenium

![The basic format of XPath in selenium](https://www.lambdatest.com/blog/wp-content/uploads/2021/07/pasted-image-0-3.png)



>   __`XPath=//x[@y="z"]:`__

- __x =*html tag:*__ for e.g. div, section, a (anchor), img, span etc.

- __y= *attribute:*__ fore.g. id, class, role, aria-label etc.

- __*z= attribute value:*__ it’s the valueof the attribute you choose written in ‘ ’comma.

#### 3.10.4.A **Indexing**

Indexing in XPath can be done using [ ] with a number that specifies the node we intend to select. It can also be done using functions like last() or position() that specify the index of the elements.


As you can see we have `3 different buttons` for different sections of  
1. Mobile- Find out more,  
2. JioFiber- Know more and  
3. Business- Learn more.


![The format of XPath](Screenshot/RelativePath1.png)



To get relative XPath for the button in JioFiber section we will use indexing method to help locate its button. 

For same XPath there are three buttons. To target a specific button we will provide the index number for the locator to find the button. So that we can find a unique locator which is 1 of 1 from 1 of 3. 

![The format of XPath](Screenshot/RelativePath2.png)

Use unique attribute, if it doesn’t have unique attribute use indexing.

__In XPath indexing starts from 1.__

For 1 of 3 the XPath will be:

> (//button[@class="j-button j-button-size__medium primary w-auto"])
>
For 1 of 1 we specify the location with index. 

Given below is the example of JioFiber-Know more:

> `(//button[@class="j-button j-button-size__medium primary w-auto"])[2]`


#### 3.10.4.B **XPath using Contains():**

Contains() is a method used in XPath expression. It is used when the value of any attribute changes dynamically, for example, login information.

>//div[contains(@class,'shadow-vertical-mg ')]

##### i] Using Contains(attributes):

You can use attribute like class with 'contains' method, to define a shorter attribute value and use use text contain to define the text. 

- It should have text and should be unique X= html tag ,z= value of text. Buttons, div tag, class attribute, iframe, status enabled, status disabled.

##### ii] Using Contains(text):

This function is used to see if a certain element contains a particular Text. Use only when you are unable to find locator by any other method.

> //x[contains(text())]

>//x[contains(text(),"z")]

>//h1[contains(text(),'Jio helpful tips')]

##### iii] Using Logical operators OR & AND:

In OR expression, two conditions are used, whether 1st condition OR 2nd condition should be true. It is also applicable if any one condition is true or maybe both. Means any one condition should be true to find the element.

In AND expression, two conditions are used, both conditions should be true to find the element. It fails to find element if any one condition is false.


>//div[@class='j-accordion-panel' and @role='tablist']

>//button[contains(text(),'Pay Bills') or @aria-label='Order New Jio SIM']

> //div[contains(@class,"j-contentBlock__description") and contains(text(),'Planning')]


##### iv] Conclusion 

XPath is one of the widely used web locators in Selenium. However, coming with an efficient XPath is an art that largely depends on the document’s structure and the current position of the WebElement.

One of the most important things is finding the nearest unique WebElement relative to the target element and using the best technique to locate the WebElement.


# 4. Test execution

## 4.1. Using Terminal
Robot Framework provides a number of command line options that can be used to control how test cases are executed and what outputs are generated. This section explains the option syntax, and what options actually exist.

### 4.1.1 To run .robot file:
> robot filename.robot

### 4.1.2. To run and specify results folder, use '-d'

> robot -d ResultFolderName/ModuleFolderName TestFolderName/ModuleFolderName/filename.robot  

Here, the '-d' tag is used to specify the results folder to store the results and screenshots of the execution.

### 4.1.3. To run single Test case using test case name, use '-t'

> robot -d ResultFolderName/ModuleFolderName -t 'test case name' TestFolderName/ModuleFolderName/filename.robot

Here, the '-t' tag is used to specify the test case name in case you wish to execute an individual test case rather than the whole .robot file

### 4.1.4. To run single test case using Automation tags, use '-i'


>robot -d ResultFolderName/ModuleFolderName -i TC_ID TestFolderName/ModuleFolderName/filename.robot

Here, the '-i' tag is used to specify the Automation tags of the test case you wish to execute.
- Note: the id of the test case must be specified below the test case name using the `[Tags]` symbol.



## 4.2. Using Batch File

Imagine you have a .robot file that you have to execute on your computer over and over again.

 Instead of manually entering the execution command in the terminal every time, you can create a batch file to automate the process.

 ![Image](Screenshot/BatchFile1.PNG)
 ![Image](Screenshot/BatchFile6.PNG)
 
- A batch file is a text file that contains a series of commands that your computer can understand and execute.
- These commands tell your computer what actions to perform, like copying files, running programs, or even creating folders.
- To create a batch file, you can use any text editor like Notepad. 

> You write the commands in the text file, save it with a .bat extension (e.g. myscript.bat), and then you can double-click the batch file to run it.

Let's say you want to create a batch file to copy files from one folder to another. You can use the "copy" command followed by the source file or folder and the destination where you want to copy them. 

For example:
``` bash 
copy C:\Folder1\file1.txt C:\Folder2

copy C:\Folder1\file2.txt C:\Folder2
```



When you run this batch file, it will copy file1.txt and file2.txt from "Folder1" to "Folder2."

You can also include other commands in your batch file, like the "mkdir" command to create a new folder or the "del" command to delete files.

 Here's an example:
 ```bash 
mkdir C:\NewFolder
copy C:\Folder1\*.txt C:\NewFolder
del C:\Folder1\file1.txt
```
 ![Image](Screenshot/BatchFile4.PNG)

In this batch file, it creates a new folder called "NewFolder" copies all the .txt files from "Folder1" to "NewFolder," and then deletes file1.txt from "Folder1."

### 4.2.1. Commands in batch file
1. echo: Displays text on the screen. It's useful for providing information or messages to the user. For example:

   > echo Hello, World!   

2. mkdir: Creates a new folder or directory. For example:

   > mkdir C:\NewFolder

3. copy: Copies files or folders from one location to another. For example:
   >  copy C:\Folder1\file1.txt C:\Folder2

4. del: Deletes files. For example:

   > del C:\Folder1\file1.txt

5. move: Moves files or folders from one location to another. For example:
   > move C:\Folder1\file1.txt C:\Folder2

6. ren: Renames files. For example:
   >ren C:\Folder1\file1.txt newfile.txt

7. start: Opens a file or starts a program. For example:
   > start notepad.exe

8. ping: Sends a network request to a specified IP address to check the connection. For example:
   > ping google.com

9. timeout: Adds a delay or pause in the batch file execution for a specified time. For example, to wait for 5 seconds:

   > timeout /t 5

10. if: Performs conditional execution based on a specified condition. For example:
    > if exist C:\Folder1\file1.txt (
    >
    >    echo File exists!
    >
    > ) else (
    >
    >    echo File does not exist!
    >
    > )

  ![Image](Screenshot/BatchFile5.PNG)  

11. REM :  REM command is used to add comments in a batch file.

    > REM This is a comment explaining the purpose of the batch file.

12. cd(Change Directory) : The cd command in batch files is used to change the current working directory (folder) in the command prompt or script.

    > cd C:\Bond_Automation\Jio.com_Code_Cleanup\Results\JioFiber



Batch files are a handy way to automate repetitive tasks and save time. They allow you to run multiple commands in sequence without needing to type them one by one in the command prompt.

# 5. Test Results

## 5.1. Results Folder:

Report files contain an overview of the test execution results in HTML format. They have statistics based on tags and executed test suites, as well as a list of all executed test cases. When both reports and logs are generated, the report has links to the log file for easy navigation to more detailed information. It is easy to see the overall test execution status from report, because its background colour is green when all tests pass and bright red if any test fails. Background can also be yellow, which means that all tests were skipped.


### 5.1.1. Log File:

Log files contain details about the executed test cases in HTML format. They have a hierarchical structure showing test suite, test case and keyword details. Log files are needed nearly every time when test results are to be investigated in detail. Even though log files also have statistics, reports are better for getting a higher-level overview.

Click on Log (Ctrl and click), the browser gets open next to the code.

![alt text](Screenshot/Logfiber1.JPG)

Click on the highlighted button, so that the window gets generated in the Chrome.

![alt text](Screenshot/Logfiber5.png)



![alt text](Screenshot/Logfiber2.png)_

The above mentioned Testcase is of Fiber Pay bills tab, we are checking UI and functionality of Fiber Pay Bills section, where the Documentation includes the number of merged testcases and the tags includes the starting test case number. The Start/End/Elapsed time is the time required to execute that particular testcase. The status of the Testcase that is pass or fail is mentioned. And, the message is displayed after the status.

![alt text](Screenshot/Logfiber3.JPG)

In the log File the Screenshots are captured, of the failed keywords and that can be identified through the built-in keywords or locator.

- __Red Highlights:__ in the colour remark indicates the failure of the keywords.

- __Green Highlights:__ indicates that the keywords are successfully passed.

- __Grey Highlights:__ indicates that the execution did not reach till that particular keyword.

- __Yellow Highlights:__ indicates that the particular keyword or the testcase is skipped while execution.

![alt text](Screenshot/Logfiber4.png)




## 5.2. Robotmetrics
RF Metrics reports, helps us to create statistical data of the Test Cases we have automated using Robot Framework. It fetches the data from the output.xml file which is generated after execution. It generates dashboard which reflects the data in the form of Tables & Piecharts. It also gives us the data on keyword failures. The tabular data represents the No. of Testcases executed, No. of Passed, Failed and Skipped Test Cases. Time Taken by each TCs to execute. Also the Success Ratio of The Testcases executed with respect to Modules. 
RF Metrics has three pages- 
1. Dashboard- Dashboard represents overall data of Modules and Sub Modules.

![alt text](Screenshot/Reporting1.jpg)

2. Suite- Suite gives the detailed report of every Modules and Sub Modules executed. 

![alt text](Screenshot/Reporting2.jpg)

3. Test- Test page is the place where data are represented wrt Tags.

![alt text](Screenshot/Reporting3.jpg)

### 5.2.1. Installation:

```bash
pip install robotframework-metrics
```

### 5.2.2. Command lines:

Command to generate Robotmetrics report: 

Change the directory to the location where output.xml of the respective file is present. 
 >cd C:\Users\Gursharan.Saini\Documents\GitHub\Jiocom_Code_Cleanup\Results\Support

>robotmetrics --metrics-report-name  Final-Report.html

The report name (For eg: Final-Report.html) can be user defined. 


 


[Gursharan/Gursharan/result2.png]: Gursharan/Gursharan/result2.png
