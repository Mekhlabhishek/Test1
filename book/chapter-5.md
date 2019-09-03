# Writing your first test case

In the last chapter, we successfully ran our program using wdio test runner. We also configured our script for the test, but we are not checking if the page that we are landing on is correct page after sign in. In this chapter, we will write our actual test case.

## 5.1 Add assertion to the program

We will be using our `first_program.spec.js` file. This will no longer our first program, so let's change the name of it to the `first_test.spec.js`.

Now to start with we will just check the URL of the page using `getUrl()` function. In order to make an assertion, first, let's import `assert` function by

```
const assert = require('assert')
```

Now let's capture the url in some variable shown as below

```
const url = browser.getUrl();
```

Next, we will make an assertion, in layman language, we will check if the URL we got is exactly what we are expecting.

While writing test cases it is best practice to fail a test case first and then pass it, so we will give any random title at a start like

```
assert.equal(url, 'http://www.google.com');
```

Also remove `.pause()` calls from the program as we don't need them anymore. Our test case at this moment will look like

```
const assert = require('assert')

describe('My first program for test runner', () => {
  it('My first test', () => {
    browser.url('./');
    browser.click('.signup-button.border-radius-lg');
    const url = browser.getUrl();
    assert.equal(url, 'http://www.google.com');
  });
});
```

Go ahead and run test case using `npm test`. You will see that our test case fails with following error

```
F

0 passing (14.60s)
1 failing

1) My first program for test runner My first test:
'https://qa.aceinvoice.com/sign_up' == 'http://www.google.com'
running chrome
AssertionError [ERR_ASSERTION]: 'https://qa.aceinvoice.com/sign_in' == 'http://www.google.com'
```


This is a way to tell the URL of the page is not `http://www.google.com`, as per our expectations. `assert` is comparing `http://qa.aceinvoice.com/sign_up` with `http://www.google.com`.

Now we can correct test case by replacing `http://www.google.com` with `https://qa.aceinvoice.com/sign_up` and running test again, this time it should pass.

In this chapter we are only checking a URL of the page what about complete flow of signing in a user. We will take steps toward it in coming chapters. So let's discover more in next chapter.
