# Running program with wdio

In last chapter we installed wdio command line interface for running our test suit.

At this point you might be wondering why to install test runner when we can execute the program without it. Here are some reasons

1. Right now we are running only one program, but as go further into course there will different files for different modules. Executing those files one by one will be very time consuming job.
2. Once we add a wdio as a test runner then we do not need to setup the environment everytime we run our program. When you execute your program with the test runner, some basic settings will be available to you by default.

So let us take a step further

## 4.1 Create folder for test cases

In configuration we gave path for test cases as

```
specs: [ './test/specs/**/*.js' ]
```

So in order to run test suit let's create a folder test ans specs respectively in `aceinvoice_web_selenium_tests` folder as follows

```
mkdir tests && cd tests && mkdir specs && cd specs && cp ../../first_program.js first_program.spec.js
```

_Test runner will find program into this folder so name has to be case sensetive. If you wish to give different name to the folder change the configuration file respective to the path._


So let's tweak our old program little bit to run using test runner.

## 4.2 Changing our program.

Open `tests/specs/first_program.spec.js` into your favourite text editor and change it so as to run with the test runner.

1. Remove remote client

When executing test runner we don't have to configure and initialize the client every time and by default you will be able to get hold of `browser` variable in program file, so replace

```
const webdriverio = require('webdriverio');

webdriverio
  .remote({ desiredCapabilities: { browserName: 'chrome' } })
  .init()
```

and use `browser` instead, so program will look like

```
browser.url('https://staging.aceinvoice.com');
```

2. Change url() call

When we use `browser` variable from the configuration, it already has base url with it, so we don't need to pass a explicit URL anymore, now replace

```
https://staging.aceinvoice.com
```

with just `./`

3. Change `setValue` function call

Right now we are using JQuery's way to set value for elements. We will not use them anymore. `setValue` accepts two arguments by default, first one query selector expression and second is value. So we will replace

```
.$('input[name="email"]').setValue('neeraj@bigbinary.com')
```

with 

```
browser.setValue('input[name="email"]', 'neeraj@bigbinary.com');
```

3. Remove `.end()` call

As said earlier we will not have to worry about the starting and closing the browser session. So go ahead and remove `.end()` method call

At this stage you final program will look like something this

```
browser.url('./');
browser.pause(1000);
browser.setValue('input[name="email"]', 'neeraj@bigbinary.com');
browser.setValue('input[name="password"]', 'welcome');
browser.pause(1000);
browser.click('input.btn.btn-primary');
browser.pause(2000);
```

Try running program now with command

```
npm test
```

we will see output on console `0 passing` but browser not performing any operations.

## 4.3 Wrap program into mocha framework

In mocha framework `describe` is way to group multiple test cases under some namespace, while `it` is way to define test case, you can find more about mocha framework [here](https://mochajs.org/#getting-started)

Let's create our first mocha test case. 

Create a namespace using `describe`, paste following code at the top of `first_program.spec.js`

```
describe('My first program for test runner', () => {

});
```

As you can see `describe` takes two arguments, first one is namespacing title and second is function to execute.

Now we will add a blank test case to it using `it` as

```
describe('My first program for test runner', () => {
  it('My first test', () => {

  });
});
```

Now as a last step, move browser code to the test case function definition, your final code in `first_program.spec.js` should look like

```
describe('My first program for test runner', () => {
  it('My first test', () => {
    browser.url('./');
    browser.pause(1000);
    browser.setValue('input[name="email"]', 'neeraj@bigbinary.com');
    browser.setValue('input[name="password"]', 'welcome');
    browser.pause(1000);
    browser.click('input.btn.btn-primary');
    browser.pause(2000);
  });
});
```

## 4.4 Kickoff execution

Once again kickoff program with `npm test`. This time you will see browser making some progress and logging in into AceInvoice app.

So far so good. Till now we are just logging into an app but we are not checking if elements on browser after completing the execution flow are correct or not. In next chapter we will convert our program into test case.
