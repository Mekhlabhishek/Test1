# Your first program

In this chapter, we will be writing program with [webdriverio](https://webdriver.io).
This program will go to the [qa.aceinvoice.com](htp://qa.aceinvoice.com)
and will complete signup procedure.

## 2.1 Writing and understanding basic program


Create a file by executing the following command.

```
touch first_program.js
```

Add the following code in that file:

```
const webdriverio = require('webdriverio');

webdriverio
  .remote({ desiredCapabilities: { browserName: 'chrome' } })
  .init()
  .url('https://qa.aceinvoice.com')
  .$('.signup-button.border-radius-lg').click()
  .getUrl().then(url => { console.log('URL is: ', title) })
  .end();
```

Let's break down the code, to understand it.

First, we are importing `webdriverio` from the node package that we installed earlier by calling

```
const webdriverio = require('webdriverio');
```

Next, we are creating a remote client with some basic options like the browser that we want to use

```
webdriverio
  .remote({ desiredCapabilities: { browserName: 'chrome' } })
```

_You can go through all the available options [here](https://webdriver.io/docs/options.html). We will be covering those in later part of the book. So, you don't have to worry about it now._

After that, we are initializing the remote client by calling the `init()` method, which will assign the session to the remote client.

Then we are navigating to our website by calling `url('https://qa.aceinvoice.com')`.

The above url will navigate us to the signin page of AceInvoice application.

On the page, there are input elements for user's email & password and a link for `signup` .

First, we are selecting an anchor tag by JQuery code `$(".signup-button.border-radius-lg")`.

_How do we know what selector value we should give? On signin page, right click on the signup link and select `inspect`. You will see some html code for the link. Check for CSS class that signup link has. We can use CSS class, id or name attribute as the selector value._

_If you want to take a quick look at what other selectors are available, [visit](https://webdriver.io/docs/selectors.html). Don't worry about them now, as said earlier we will be taking look at selectors in later part of the course._

Once we get hold of the link, we are clicking that link using `click()`.

Once we click the link, we are checking the URL of the page by `getUrl()` function.

_`then()` is a way to resolve a promise. You can get the basic idea of a promise in JS [here](https://javascript.info/promise-basics). You may have noticed that we are not calling an `await` and `async` here, these are reserved keywords to resolve the promise. As moving to the WebdriverIO@5 in later part of the course we will start using them, but don't worry about it now._

_`url => console.log('URL is: ', url)` is example of an arrow function, learn more about arrow functions [here](https://codeburst.io/javascript-arrow-functions-for-beginners-926947fc0cdc)._

After getting the URL for the webpage, we are terminating our session by calling `end()`.

Now, this is the time to run our first program.

## 2.2 Running your first program

Start the `selenium-standalone` server, using the following command in the terminal

```
selenium-standalone start --version=3.4.0
```

Once the server starts, open up a new terminal window and navigate to the directory and start executing a program by

```
node first_program.js
```

You will see Chrome window popping up and navigating to `qa.aceinvoice.com`, then completing the sign up flow and closing chrome window. And on the terminal, you will see the output as

```
URL is :  https://qa.aceinvoice.com/sign_up
```

Program execution may be too fast to figure you out what exactly is going on, so to slow it down a bit, add a sleep time after each step using `pause(#time_in_ms)`. So our new program will look something like this

```
const webdriverio = require('webdriverio');

webdriverio
  .remote({ desiredCapabilities: { browserName: 'chrome' } })
  .init()
  .url('https://qa.aceinvoice.com')
  .pause(1000)
  .$('.signup-button.border-radius-lg').click()
  .pause(1000)
  .getUrl().then(url => { console.log('URL is: ', url) })
  .end();
```

That's it, you ran your first program successfully. It's time to get more serious. Here, we have just completed login flow for AceInvoice. We are not testing if it is correct or not. In the upcoming chapters, we will write and run test cases with the `wdio` test runner. Till that time, you can play with the code above and try some permutations and combinations.

See you in the next chapter.
