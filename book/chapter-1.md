## Introduction to Selenium WebDriver

Selenium is an open source tool to test web applications running in browsers.
Selenium does not test desktop applications.


Selenium WebDriver was the first cross platform testing framework that could control the browser from the OS. 
Using Selenium WebDriver we identify web elements on web pages and then actions are performed on those elements.
We have one web driver for each browser.

* Chrome Driver
* Firefox Driver (Gecko Driver)
* Internet Explorer Driver
* Opera Driver
* Safari Driver
* HTML Unit Driver

Selenium Webdriver supports following languages. What it means is that we can
write selenium webdriver tests in any of the following languages.

* C#
* Java
* JavaScript
* Ruby
* Python
* PHP


## WebdriverIO

[WebdriverIO](https://webdriver.io) is a custom implementation for selenium's webdriver API. 
It is written in Javascript and runs on Node.js.
WebdriverIO is distributed as an npm package.

In this book we will be writing selenium webdriver tests using JavaScript programming language.
We will be using [webdriver](https://www.selenium.dev/documentation/en/webdriver/) to write our tests.

## Check Node.js is installed

Open `terminal` on your machine and check the version of the node using the following command.

```bash
$ node -v
```

_This should print node version >= 8.15.0_.

## Installing Node.js

Download the desired node version from the [official](https://nodejs.org/en/download) site.


## Installing selenium-standalone and WebdriverIO

Once web have Node.js in place, let's go ahead and install tools that we will be using to test our application.


```bash
$ mkdir aceinvoice-selenium-tests
$ cd aceinvoice-selenium-tests
```

Initialize npm using the following command.
`-y` is yes for  all questions that npm init will ask.


```bash
$ npm init -y
```

Next let's install `webdriverio`.
`--save` will add these modules as project dependencies and `xx@4` is the version number for the package.

```bash
$ npm install --save webdriverio@4
```

We will be installing `selenium-standalone` by running the following command.

```bash
$ npm install selenium-standalone
```

Let's install dependencies for selenium by executing the following command.
This will install "selenium-server", "ChromeDriver" for chrome browser and "geckodriver" for Firefox.

```bash
$ node_modules/.bin/selenium-standalone install
```

Verify that you installed the selenium correctly by executing the following command.

```bash
$ node_modules/.bin/selenium-standalone start
```


This will start the selenium server on port `4444`.
Open your browser and try to visit [http://localhost:4444](http://localhost:4444)
and you will see the selenium webpage.

To see the console and all sessions visit 
[http://localhost:4444/wd/hub/](http://localhost:4444/wd/hub/) 
and you will see the console screen.

Hola! That's it. Now, you are ready to write your first program.

https://www.loom.com/share/274bc70c98b5473fa7a230a1a1fe4416
