# Checking Contents on a page in a test case

In the last chapter, we just checked if the title is correct or not, but whether sign in failed or not a title for the page will be always the same.
So in this chapter, we will look around how to check actual content on the page, for this purpose we will be using our same old program
and will modify it to meet our requirement.

## 6.1 Test for unsuccessful login

So when we enter the wrong email or password, an error message pops up saying, `Incorrect email or password`. In this test case, we will check that. So go ahead and change out test case to enter wrong login details, make a wrong entry for email or/and password. So taking our old test case into consideration, this will look something like this

```
const assert = require('assert');

describe('My first program for test runner', () => {
  it('My first test', () => {
    browser.url('./');
    browser.setValue('input[name="email"]', 'fake@bigbinary.com');
    browser.setValue('input[name="password"]', 'wrong_password');
    browser.click('input.btn.btn-primary');

    const title = browser.getTitle();
    assert.equal(title, 'Random');
  });
});
```

Try to run a test case, what you are seeing on console? Getting error right?

```
An element could not be located on the page using the given search parameters ("div.toast-message").
```

What is wrong here? In a normal workflow, we are seeing an error message then why test case is failing? The reason is, after clicking a submit button we are not waiting for the response, we are checking for error message immediately which is not on the screen at that time, it getting rendered sometime after the clicking sign in button.

For now, we will hold the execution flow with `pause` callback. So add some pause after clicking submit button and wait for the response and then check for the error.

This time you will see our test case is getting rendered correctly. Now what we decided as a good practice while writing a test case? Now your test case passed, try to fail them and cross verify that your test case is right.

_Plenty of ways to check that, add correct cred and check if an error pops up or enter wrong creds and check if an error does not pop up._

## 6.2 Check for successful login

Now as we checked unsuccessful login we should also check successful login as well. Add a new test case in the same file with a different description that will explain a test case purpose and add a new test case below our old test case.

In this new test case, we are going to check after successful login are we getting time entry panel shown on the screen. So go ahead and try to add a test case this time to check if time entry panel or calendar is being shown after successful login.

Here is code which will look something like this, we added a test case to check header text in the time entry panel, your test case may look something different, don't worry this does not has to be a perfect match, after all, everyone's thinking is different.

```
const assert = require('assert');

describe('AceInvoice SignIn', () => {
  describe('When login is un-successful', () => {
    it('Shows error message', () => {
      browser.url('./');
      browser.setValue('input[name="email"]', 'neeraj@bigbinary.com');
      browser.setValue('input[name="password"]', 'welcom');
      browser.click('input.btn.btn-primary');
      browser.pause(10000);

      const error = browser.getText('div.toast-message');

      assert.equal(error, 'Incorrect email or password');
    });
  });

  describe('When login is successful', () => {
    it('Logs user into application', () => {
      browser.url('./');
      browser.setValue('input[name="email"]', 'neeraj@bigbinary.com');
      browser.setValue('input[name="password"]', 'welcome');
      browser.click('input.btn.btn-primary');
      browser.pause(10000);
      const headerText = browser.getText('h4.log-entry-header');

      assert.equal(headerText, 'Log Time');
    });
  });
});

```

_Getting timeout exception while running test cases? Changing timeout won't help this time, you need to add a timeout in mochaOpts as shown below_

```
mochaOpts: {
  ui: 'bdd',
  timeout: 100000
}
```

The last thing to cover on the sign-in page is `signup` link. After hitting a link user should be redirected to the sign-up page. This time take your shot and write a test case for the sign-up link, after clicking link check URL of the browser matches the sign-up link.

Now you are a good sail on your own. Write some more test about sign-in that you can think of.
