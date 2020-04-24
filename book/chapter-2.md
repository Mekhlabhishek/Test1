In this chapter, we will be write a program that will go to [qa.aceinvoice.com](http://qa.aceinvoice.com)
and will complete signup procedure.

## Writing and understanding a basic program


Create a file by executing the following command.

```bash
$ touch first_program.js
```

Add the following code in that file:

```js
const webdriverio = require('webdriverio');

webdriverio
  .remote({ desiredCapabilities: { browserName: 'chrome' } })
  .init()
  .url('https://qa.aceinvoice.com')
  .scroll("//strong[contains(text(),'Sign Up')]")
  .pause(100)
  .$("//strong[contains(text(),'Sign Up')]").click()
  .getUrl().then(url => { console.log('URL is: ', url) })
  .end();
```

## Running  first program

Start the `selenium-standalone` server, using the following command in the terminal.

```bash
$ node_modules/.bin/selenium-standalone start
```

Start a new terminal tab and  execute the program.

```bash
$ node first_program.js
```

We will see Chrome window popping up and navigating to `qa.aceinvoice.com`, then completing the sign up flow and closing chrome window. And on the terminal, we will see following output.

```msg
URL is :  https://qa.aceinvoice.com/sign_up
```

Here are the things selenium did behind the scene.

* Selenium opened chrome browser
* Asked the browser to visit `https://qa.aceinvoice.com`
* Found the location of text containing word "Sign Up"
* Clicked on the "Sign Up"
* Grabbed the new url
* Write the new url on console.

All this happened so fast that it was hard to notice all that.
So we will ask Selenium to pause in between. Here is modified code.
Notice that we have added `pause` statements.

```js
const webdriverio = require('webdriverio');

webdriverio
  .remote({ desiredCapabilities: { browserName: 'chrome' } })
  .init()
  .url('https://qa.aceinvoice.com')
  .scroll("//strong[contains(text(),'Sign Up')]")
  .pause(200)
  .$("//strong[contains(text(),'Sign Up')]").click()
  .pause(3000)
  .getUrl().then(url => { console.log('URL is: ', url) })
  .end();
```

That's it, we ran our first program successfully. 
Here, we captured the URL and printed it.


## Code walkthrough

Let's go over the code.

First, we are importing `webdriverio` from the node package that we installed earlier.

```js
const webdriverio = require('webdriverio');
```

Next, we are creating a remote client with some basic options like the browser that we want to use.

```msg
webdriverio
  .remote({ desiredCapabilities: { browserName: 'chrome' } })
```

Selenium provides lots of [options](https://webdriver.io/docs/options.html). 
We will be covering these in the later parts of the book. 

After that, we are initializing the remote client by calling the `init()` method, which will assign the session to the remote client.

Then we are navigating to our website by calling `url('https://qa.aceinvoice.com')`.

The above url will redirect to the signin page of AceInvoice.

On this page, there are input elements for the user's email & password and a link for `signup`.

Here we are looking for any element containing the desired text `$("//strong[contains(text(),'Sign Up')]")`.

Command `scroll` is used to move the mouse pointer to the particular element.

_How do we know what selector value we should give? On signin page, right click on the signup link and select `inspect`. You will see some html code for the link. Check for CSS class that signup link has. We can use CSS class, id or name attribute as the selector value._

_If you want to take a quick look at what other selectors are available, [visit](https://webdriver.io/docs/selectors.html). Don't worry about them now, as said earlier we will be taking look at selectors in later part of the course._

Once we get hold of the link, we are clicking that link using `click()`.

Once we click the link, we are checking the URL of the page by `getUrl()` function.

_`then()` is a way to resolve a promise. You can get the basic idea of a promise in JS [here](https://javascript.info/promise-basics). You may have noticed that we are not calling an `await` and `async` here, these are reserved keywords to resolve the promise. As moving to the WebdriverIO@5 in later part of the course we will start using them, but don't worry about it now._

_`url => console.log('URL is: ', url)` is example of an arrow function, learn more about arrow functions [here](https://codeburst.io/javascript-arrow-functions-for-beginners-926947fc0cdc)._

After getting the URL for the webpage, we are terminating our session by calling `end()`.

Now, this is the time to run our first program.
