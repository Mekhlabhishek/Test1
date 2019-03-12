# Running program with wdio

In the last chapter, we installed wdio command line interface for running our test suit.

At this point, you might be wondering why to install test runner when we can execute the program without it. Here are some reasons

1. Right now we are running only one program, but as go further into course there will different files for different modules. Executing those files one by one will be a very time-consuming job.
2. Once we add a wdio as a test runner then we do not need to set up the environment every time we run our program. When you execute your program with the test runner, some basic settings will be available to you by default.

So let us take a step further

## 4.1 Create a folder for test cases

In the configuration, we have a path for test cases as

```
specs: [ './test/specs/**/*.js' ]
```

So in order to run a test suit let's create a folder test and specs respectively in `aceinvoice_web_selenium_tests` folder as follows

```
mkdir tests && cd tests && mkdir specs && cd specs && cp ../../first_program.js first_program.spec.js
```

_Test runner will find program into this folder so the name has to be case sensitive. If you wish to give a different name to the folder change the configuration file respective to the path._


So let's tweak our old program little bit to run using test runner.

## 4.2 Changing our program.

Open `tests/specs/first_program.spec.js` into your favorite text editor and change it so as to run with the test runner.

1. Remove remote client

When executing test runner we don't have to configure and initialize the client every time and by default, you will be able to get hold of `browser` variable in the program file, so replace

```
const webdriverio = require('webdriverio');

webdriverio
  .remote({ desiredCapabilities: { browserName: 'chrome' } })
  .init()
```

and use `browser` instead, so the program will look like

```
browser.url('https://staging.aceinvoice.com');
```

2. Change url() call

When we use `browser` variable from the configuration, it already has base url with it, so we don't need to pass an explicit URL anymore, now replace

```
https://staging.aceinvoice.com
```

with just `./`

3. Update `click()` call

Instead of selecting a element with JQuery and then clicking on element is not a good practice. WebdriverIO give a clean way to do it.
We can pass a selector to the click function, so update our click function to look like.

```
.click('.signup-button.border-radius-lg')
```

4. Remove `.end()` call

As said earlier we will not have to worry about the starting and closing the browser session. So go ahead and remove `.end()` method call

At this stage, your final program will look like something this

```
browser.url('./');
browser.pause(1000);
browser.click('.signup-button.border-radius-lg');
browser.pause(2000);
```

Try running program now with the command

```
npm test
```

we will see the output on console `0 passing` but browser not performing any operations.

## 4.3 Wrap program into the mocha framework

In mocha framework `describe` is a way to group multiple test cases under some namespace, while `it` is a way to define a test case, you can find more about mocha framework [here](https://mochajs.org/#getting-started)

Let's create our first mocha test case.

Create a namespace using `describe`, paste following code at the top of `first_program.spec.js`

```
describe('My first program for test runner', () => {

});
```

As you can see `describe` takes two arguments, first one is the namespacing title and second is a function to execute.

Now we will add a blank test case to it using `it` as

```
describe('My first program for test runner', () => {
  it('My first test', () => {

  });
});
```

Now as the last step, move browser code to the test case function definition, your final code in `first_program.spec.js` should look like

```
describe('My first program for test runner', () => {
  it('My first test', () => {
      browser.url('./');
      browser.pause(1000);
      browser.click('.signup-button.border-radius-lg');
      browser.pause(1000);
  });
});
```

## 4.4 Kickoff execution

Once again kickoff program with `npm test`. This time you will see the browser making some progress and logging in into AceInvoice app.

So far so good. Till now we are just logging into an app but we are not checking if elements on the browser after completing the execution flow are correct or not. In the next chapter, we will convert our program into a test case.
