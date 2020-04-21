In the last chapter, we successfully ran our program using wdio test runner. We also configured our script for the test, but we are not checking if the page that we are landing on is correct page after sign in. In this chapter, we will write our actual test case.

## Add assertion to the program

We will be using our `first_program.spec.js` file. This will no longer be our first program, so let's change the file name to `first_test.spec.js`.

Now to start with, we will just check the URL of the page using `getUrl()` function. In order to make an assertion, first, let's import `assert` function by

```js
const assert = require('assert')
```

And capture the title in some variable, shown as below

```js
const url = browser.getUrl();
```

Next, we will make an assertion, in layman language. We will check if the URL we got is exactly what we are expecting.

While writing test cases, it is a good practice to make a test case fail first and then make it pass. So, we will give any random url at the start like

```js
assert.equal(url, 'http://www.google.com');
```

Also remove `.pause()` calls from the program as we don't need them anymore. Our test case at this moment will look like

```js
const assert = require('assert')

describe('My first program for test runner', () => {
  it('My first test', () => {
    browser.url('./');
    browser.$('input.btn.btn-primary').click();
    const url = browser.getUrl();
    assert.equal(url, 'http://www.google.com');
  });
});
```

Go ahead and run the test case using `npm test`. You will see that our test case failed, with the below error in console.

```msg
F

0 passing (14.60s)
1 failing

1) My first program for test runner My first test:
'https://qa.aceinvoice.com/sign_up' == 'http://www.google.com'
running chrome
AssertionError [ERR_ASSERTION]: 'https://qa.aceinvoice.com/sign_in' == 'http://www.google.com'
```

This is a way to tell that the URL of the page is not `http://www.google.com` that we are expecting. `assert` is comparing `http://qa.aceinvoice.com/sign_up` with `http://www.google.com`.

Now, correct the test case by replacing `http://www.google.com` with `http://qa.aceinvoice.com/sign_up` and run it again. This time it will pass.

In this chapter, we are only checking the URL of the page but what about complete flow of signing up a user. We will take steps towards it in coming chapters. So let's discover more in next chapter.
