In the last chapter, we installed wdio command line interface for running our test suite.

At this point, we might be wondering why to install test runner when we can execute the program without it. Here are some reasons

1. Right now we are running only one program, but as we go further into the course there will be different files for different modules. Executing those files one by one will be a very time-consuming job.
2. Once we add wdio as a test runner, we do not need to set up the environment every time we run our program. When we execute our program with the test runner, some basic settings will be available to us by default.

So let us take a step further

## Create a folder for test cases

In the configuration, we have a path for test cases as

```msg
specs: [ './test/specs/**/*.js' ]
```

So, in order to run a test suite, let's create folders test & specs respectively in `aceinvoice_web_selenium_tests` folder as follows

```bash
$ mkdir test && cd test && mkdir specs && cd specs && cp ../../first_program.js first_program.spec.js
```

_Test runner will find program into this folder so the name has to be case sensitive. If we wish to give a different name to the folder change the configuration file respective to the path._


So let's tweak our old program a little bit to run using test runner.

## Changing our program

Let us open `test/specs/first_program.spec.js` into our favorite text editor and change it so as to run with the test runner.

1. Remove remote client

When executing test runner we don't have to configure and initialize the client every time and by default, we will be able to get hold of `browser` variable in the program file, so replace

```js
const webdriverio = require('webdriverio');

webdriverio
  .remote({ desiredCapabilities: { browserName: 'chrome' } })
  .init()
```

and use `browser` instead, so the program will look like

```js
browser.url('https://qa.aceinvoice.com');
```

2. Change url() call

When we use `browser` variable from the configuration, it already has base url with it, so we don't need to pass an explicit URL anymore, now replace

```msg
https://qa.aceinvoice.com
```

with just `./`

3. Update `click()` call

Instead of selecting an element with JQuery and then clicking on that element, is not a good practice. WebdriverIO gives a clean way to do it.
We can pass a selector to the click function, so update our click function to look like.

```js
.click("//strong[contains(text(),'Sign Up')]")
```

4. Remove `.end()` call

As said earlier, we will not have to worry about starting & closing the browser session. So go ahead and remove `.end()` method call.

At this stage, our final program will look like this.

```js
browser.url('./');
browser.scroll("//strong[contains(text(),'Sign Up')]");
browser.pause(200);
browser.click("//strong[contains(text(),'Sign Up')]");
browser.waitForVisible("//form[@id='new_user']");
console.log(browser.getUrl());
```

Try running the program now with the command

```bash
npm test
```

we will see the output on console `0 passing` and it seems browser is not performing any operations. However, that is not the case. Above commands are executed however they are not yet using synchronous mode of test runner. As we can see that `browser.getUrl()` is returning a promise here and not resolving.

Output is `{ state: 'pending' }`. Let's fix that in the next section.

## Wrap program into the mocha framework

In mocha framework, `describe` is a way to group multiple test cases under some namespace, while `it` is a way to define a test case.
You can find more about mocha framework [here](https://mochajs.org/#getting-started)

Let's create our first mocha test case.

Create a namespace using `describe` by adding following code at the top of `first_program.spec.js`

```js
describe('My first program for test runner', () => {

});
```

As we can see `describe` takes two arguments. First one is the namespacing title and second is a function to execute.

Now, we will add a blank test case to it using `it` as

```js
describe('My first program for test runner', () => {
  it('My first test', () => {

  });
});
```

Now as the last step, move browser code to the test case function definition. We can also add browser.pause() statements to actually see what's happening
Our final code in `first_program.spec.js` should look like


```js
describe('My first program for test runner', () => {
  it('My first test', () => {
      browser.url('./');
      browser.pause(1000);
      browser.click("//strong[contains(text(),'Sign Up')]");
      console.log(browser.getUrl());
      browser.pause(1000);
  });
});
```

Output now is `https://qa.aceinvoice.com/sign_up`.

## Kickoff execution

Once again, kickoff program with `npm test`. This time, we will see the browser making some progress and logging in into AceInvoice app.

So far so good. Till now we are just logging into an app but we are not checking if elements on the browser are correct or not, after completing the execution flow. In the next chapter, we will convert our program into a test case.
