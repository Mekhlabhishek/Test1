# Insides for WebdriverIO and Selenium

Welcome to the advanced section of the WebdriverIO course. In the last chapters, we saw how to write test cases and run them with the mocha framework. Now, we are going to see some advanced features for WebdriverIO. So, let's begin with taking a look into how webdriverio and selenium do a job for us.

## Getting ready

For this chapter, we will be using our very own old program which we learned in previous chapters for signing into AceInvoice, with an addition to a print title on the console.

```
const assert = require('assert');

describe('My first program for test runner', () => {
  it('My first test', () => {
    browser.url('./');
    browser.setValue('input[name="email"]', 'neeraj@bigbinary.com');
    browser.setValue('input[name="password"]', 'welcome');
    browser.click('input.btn.btn-primary');

    var title = browser.getTitle();
    console.log('Title is : ', title);
  });
});
```

Now, the next thing, which is important to this chapter, change log level to `verbose` in `wdio.config.js`.

Good to go, let's run our program with `npm test` command. You will see a bunch of operations happening and printing some data on the console, something like this.

```
[17:24:04]  COMMAND     POST     "/wd/hub/session"
[17:24:04]  DATA                {"desiredCapabilities":{"javascriptEnabled":true,"locationContextEnabled":true,"handlesAlerts":true,"rotatable":true,"maxInstances":5,"browserName":"chrome","loggingPrefs":{"browser":"ALL","driver":"ALL"},"requestOrigins":{"url":"http://webdriver.io","version":"4.14.2","name":"webdriverio"}}}
[17:24:07]  INFO        SET SESSION ID 5facf94a01bffed6e1479c39778b89bf
[17:24:07]  RESULT              {"acceptInsecureCerts":false,"acceptSslCerts":false,"applicationCacheEnabled":false,"browserConnectionEnabled":false,"browserName":"chrome","chrome":{"chromedriverVersion":"2.43.600229 (3fae4d0cda5334b4f533bede5a4787f7b832d052)","userDataDir":"/var/folders/v6/_6sh53vn5gl3lct18w533gr80000gn/T/.org.chromium.Chromium.bWkuWC"},"cssSelectorsEnabled":true,"databaseEnabled":false,"goog:chromeOptions":{"debuggerAddress":"localhost:58451"},"handlesAlerts":true,"hasTouchScreen":false,"javascriptEnabled":true,"locationContextEnabled":true,"mobileEmulationEnabled":false,"nativeEvents":true,"networkConnectionEnabled":false,"pageLoadStrategy":"normal","platform":"Mac OS X","rotatable":false,"setWindowRect":true,"takesHeapSnapshot":true,"takesScreenshot":true,"unexpectedAlertBehaviour":"","version":"72.0.3626.121","webStorageEnabled":true,"webdriver.remote.sessionid":"5facf94a01bffed6e1479c39778b89bf"}
[17:24:07]  COMMAND     POST     "/wd/hub/session/5facf94a01bffed6e1479c39778b89bf/url"
[17:24:07]  DATA                {"url":"https://staging.aceinvoice.com/"}
[17:24:17]  COMMAND     POST     "/wd/hub/session/5facf94a01bffed6e1479c39778b89bf/elements"
[17:24:17]  DATA                {"using":"css selector","value":"input[name=\"email\"]"}
[17:24:17]  RESULT              [{"ELEMENT":"0.07742633503067009-1"}]
[17:24:17]  COMMAND     POST     "/wd/hub/session/5facf94a01bffed6e1479c39778b89bf/element/0.07742633503067009-1/clear"
[17:24:17]  DATA                {}
[17:24:17]  COMMAND     POST     "/wd/hub/session/5facf94a01bffed6e1479c39778b89bf/element/0.07742633503067009-1/value"
[17:24:17]  DATA                {"value":["n","e","e","r","a","j","@","b","i","g","(10 more items)"],"text":"neeraj@bigbinary.com"}
[17:24:17]  COMMAND     POST     "/wd/hub/session/5facf94a01bffed6e1479c39778b89bf/elements"
[17:24:17]  DATA                {"using":"css selector","value":"input[name=\"password\"]"}
[17:24:17]  RESULT              [{"ELEMENT":"0.07742633503067009-2"}]
[17:24:17]  COMMAND     POST     "/wd/hub/session/5facf94a01bffed6e1479c39778b89bf/element/0.07742633503067009-2/clear"
[17:24:17]  DATA                {}
[17:24:17]  COMMAND     POST     "/wd/hub/session/5facf94a01bffed6e1479c39778b89bf/element/0.07742633503067009-2/value"
[17:24:17]  DATA                {"value":["w","e","l","c","o","m","e"],"text":"welcome"}
[17:24:17]  COMMAND     POST     "/wd/hub/session/5facf94a01bffed6e1479c39778b89bf/element"
[17:24:17]  DATA                {"using":"css selector","value":"input.btn.btn-primary"}
[17:24:17]  RESULT              {"ELEMENT":"0.07742633503067009-3"}
[17:24:17]  COMMAND     POST     "/wd/hub/session/5facf94a01bffed6e1479c39778b89bf/element/0.07742633503067009-3/click"
[17:24:17]  DATA                {}
[17:24:19]  COMMAND     GET      "/wd/hub/session/5facf94a01bffed6e1479c39778b89bf/title"
[17:24:19]  DATA                {}
[17:24:19]  RESULT              "Ace Invoice"
Title is :  Ace Invoice
․[17:24:19]  COMMAND    DELETE   "/wd/hub/session/5facf94a01bffed6e1479c39778b89bf"
[17:24:19]  DATA                {}
```

## Whats exactly happening here?

Webdriver does not handle browser on its own, it is Selenium which takes care of the browser operations. Webdriver just sends a request to selenium server and then selenium server performs operations on browser based on the requests that it gets from the webdriver.

The very first request that webdriver will send out to selenium server is for getting the session ID. As you can see in a response above the first webdriver is requesting the session id to the selenium server. The server then starts the browser and gives back the session ID in JSON response. This session id will be used by webdriver to make all future requests to the selenium server.

Next, as we are setting URL to the browser, webdriver sends out a `post` request to the selenium server with URL in `DATA` as follows

```
[17:24:07]  COMMAND     POST    "/wd/hub/session/5facf94a01bffed6e1479c39778b89bf/url"
[17:24:07]  DATA                {"url":"https://staging.aceinvoice.com/"}
```

Very next thing, we are setting a value to the email field. In a program, this may look like a single command but setting a value into text input is a set of three post requests.

First, webdriver will send out the `post` request to find out the element that we want, with the type of selector and value for a selector. The request then sends back element ID, which webdriver IO will use while setting text in input variable.

```
[17:24:17]  COMMAND     POST    "/wd/hub/session/5facf94a01bffed6e1479c39778b89bf/elements"
[17:24:17]  DATA                {"using":"css selector","value":"input[name=\"email\"]"}
[17:24:17]  RESULT              [{"ELEMENT":"0.07742633503067009-1"}]
```

Once webdriver gets element ID, it will send out a request to clear out the content in that element and then will make a third request to set a value in it.

```
[17:24:17]  COMMAND     POST     "/wd/hub/session/5facf94a01bffed6e1479c39778b89bf/element/0.07742633503067009-1/clear"
[17:24:17]  DATA                {}
[17:24:17]  COMMAND     POST     "/wd/hub/session/5facf94a01bffed6e1479c39778b89bf/element/0.07742633503067009-1/value"
[17:24:17]  DATA                {"value":["n","e","e","r","a","j","@","b","i","g","(10 more items)"],"text":"neeraj@bigbinary.com"}
```

Same way, webdriver will send out requests for setting value in the password field and then for clicking on submit button.

Then we are fetching the title of the webpage, similarly, webdriver will send out the request for fetching the title of the page. Selenium will then return the title in response as follows.

```
[17:24:19]  COMMAND     GET      "/wd/hub/session/5facf94a01bffed6e1479c39778b89bf/title"
[17:24:19]  DATA                {}
[17:24:19]  RESULT              "Ace Invoice"
```

After that, you will see the console log that we created from the program.

Last request webdriver will send to the selenium server to kill the session. After receiving the delete command for the session, selenium will quit the browser.

Pretty interesting, isn't it? Let's move and explore some other features of WebdriverIO.
