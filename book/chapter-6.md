# Exercise for sign up flow

In last chapter we just checked if URL of the page is correct or not. In this chapter we will cover entire signup workflow.
So let's begin.

## Setting email for test account

In a same namespace we will create another test case for adding the email for the new account. So create a new `it` block just below previous one.

When you visit the signup page you will see the field for the email. Now we have to setup the email for the our test account.
To enter values to the input fields webdriver gives handy method called as `setValue()`. Set value accepts two values here, first one is
selector and second one is the value for input field.

So add a value for email as follows in `it` block.

```
browser.setValue('input[name="email"]', 'test@webdriver.com');
```

After adding an email we also want to click on the submit button. So our next step will be clicking on submit button using `click()` function as follows.

```
browset.click('.btn.btn-primary');
```

After entering an email for our new account server will redirect us to the second step for adding a password. Here we can check that if field for password is present or not. For this test case we will use a another function called as `getCssProperty()`. This function also accepts two values. First one the selector for the element and second one the CSS value that we want to fetch.

So we can check that height for the password field is not 0 using

```
var passwordHight = browser.getCssProperty('input[name="password"]', 'height');
```

Before making an assertion let's check what this function returns. Check the value for `passwordHeight` using console log. You will see something like this

```
{ property: 'height',
  value: '38px',
  parsed: { type: 'number', string: '38px', unit: 'px', value: 38 } }
```

So here we can see that function returns the complete CSS data about the element. For this test case we will be using parsed height of the element which is 38 and will be checking that height is always greater than 0 as

```
assert.notEqual(passwordHeight.parsed.value, 0);
```

At this moment our file will look something like this

```
describe('AceInvoice SignUp', () => {
  it('URL has sign_up and staging.aceinvoice.com as a server address', () => {
    browser.url('./');
    browser.click('.signup-button.border-radius-lg');

    var url = browser.getUrl();
    assert.equal(url, 'https://staging.aceinvoice.com/sign_up');
  });

  it('Navigates to password page after adding an valid email', () => {
    browser.setValue('input[name="email"]', 'test@webdriverio.com');
    browser.click('.btn.btn-primary');

    var passwordInputHeight = browser.getCssProperty('input[name="password"]', 'height');
    assert.notEqual(passwordInputHeight.parsed.value, 0);
  });
});
```
