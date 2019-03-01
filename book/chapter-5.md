# Writing your first test case

In last chapter we successfully ran our program using wdio test runner. We also configured our script for test, but we are not checking if
page that we are landing on is correct page after sign in. In this chapter we will write our actual test case

## 5.1 Add assertion to the program

We will be using our `first_program.spec.js` file. This will no longer our first program, so let's change name of it to the `first_test.spec.js`.

Now to start with we will just check title of the page using `getTitle()` function. In order to make assertion, first let's import `assert` function by

```
const assert = require('assert')
```

Now let's capture title in some variable shown as below

```
const title = browser.getTitle();
```

Next we will make assertion, in layman language, we will check if title we got is exactly what we are expecting.

While writing a test cases it is best practice to fail a test case first and then pass it, so we will give any random title at a start like

```
assert.equal(title, 'Random');
```

Also remove `.pause()` calls from program as we don't need them anymore. Our test case at this moment will look like


```
const assert = require('assert');

describe('My first program for test runner', () => {
  it('My first test', () => {
    browser.url('./');
    browser.setValue('input[name="email"]', 'neeraj@bigbinary.com');
    browser.setValue('input[name="password"]', 'welcome');
    browser.click('input.btn.btn-primary');

    const title = browser.getTitle();
    assert.equal(title, 'Random');
  });
});
```

