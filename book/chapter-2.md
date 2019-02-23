# Your first program

In this chapter we will be writing our basic program with `webdriverio`. This program will go to the `staging.aceinvoice.com` and will complete login procedure for us.

So let's start.

## 1.1 writing and understanding basic program

Create a file using `touch first_program.js` command on terminal and paste a following code into it

```
const webdriverio = require('webdriverio');

webdriverio
  .remote({ desiredCapabilities: { browserName: 'chrome' } })
  .init()
  .url('https://staging.aceinvoice.com')
  .$('input[name="email"]').setValue('neeraj@bigbinary.com')
  .$('input[name="password"]').setValue('welcome')
  .$('input.btn.btn-primary').click()
  .getTitle().then(title => { console.log('Title is : ', title) })
  .end();

```

Let's take a quick look at a code and understand it.

We are importing a `webdriverio` first from the node package that we installed earlier by calling

```
const webdriverio = require('webdriverio');
```

Next we are creating remote client with some basic options like browser that we want to use as follows

```
webdriverio
  .remote({ desiredCapabilities: { browserName: 'chrome' } })
```

_Take a look at all options [here](https://webdriver.io/docs/options.html). We will be covering those in later part of the book
so you don't have to worry about it now._

After that we are initializing the remote client by calling `init()` method, which will assign the session to a remote client.

Then we are navingating to our website by calling `url('https://staging.aceinvoice.com/sign_in')`.

On the page there are input elements for user's email and password. Now we have to add an values to that field.

First we are selecting element to which we want to add a value by simple JQuery code as `$('input[name="email"]')`.

Once we get hold of the element, we are setting value to the field using `setValue()` function

_If you want to take quick look at what other selectors are available, [visit](https://webdriver.io/docs/selectors.html). Don't worry about them now, 
as said earlier we will taking look at selectors in later part of the course._

Similarly we are setting password for the user using

```
.$('input[name="password"]').setValue('welcome')
```

After setting a value to the input fields we are clicking button on page by function

```
.$('input.btn.btn-primary').click()
```

and once we hit the button we are checking the title of the page by `getTitle()` function.

_`then()` is way to resolve a promise you can get a basic idea of a promise in JS [here](https://javascript.info/promise-basics). You may have noticed we are not calling a `await` and `async` here, these are reserved keywords to resolve promise. As moving to the WebdriverIO@5 in later part of the course we will start using them, but don't worry about it now._

_`title => console.log('Title is : ', title)` is called an arrow function, learn more about arrow functions [here](https://codeburst.io/javascript-arrow-functions-for-beginners-926947fc0cdc)._

After getting title for the webpage we are terminating our session by calling `end()`.

Now this is the time to run our first program.

## 1.2 Running your first program

Start the `selenium-standalone` server by command in the terminal

```
selenium-standalone start --version=3.4.0
```

Once server starts, open up a new terminal window and navigate to the directory and start executing a program by

```
node first_program.js
```

You will see Chrome window popping up and navingating to `staging.aceinvoice.com`, then completing login flow and closing chrome window. And on terminal you will see the output as

```
Title is :  Ace Invoice
```

Program execution may be too fast to figure you out what exactly is going on, so to tackle this situation go ahead and add a sleep time after each step using `pause(#time_in_ms)`. So our new program will look something like this

```
const webdriverio = require('webdriverio');

webdriverio
  .remote({ desiredCapabilities: { browserName: 'chrome' } })
  .init()
  .url('https://staging.aceinvoice.com')
  .pause(1000)
  .$('input[name="email"]').setValue('neeraj@bigbinary.com')
  .pause(1000)
  .$('input[name="password"]').setValue('welcome')
  .pause(1000)
  .$('input.btn.btn-primary').click()
  .pause(2000)
  .getTitle().then(title => { console.log('Title is : ', title) })
  .end();
```

That's it, you ran your first program successfully. It's time to get more serious. Here we are just completed login flow for AceInvoice. We are not testing if it is correct or not. In coming chapters we will write and run test cases with the `wdio` test runner, till that time you can play with the code above and try some permutations and combinations.

See you in next chapter.
