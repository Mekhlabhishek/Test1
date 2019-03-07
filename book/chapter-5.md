# Writing your first test case

In the last chapter, we successfully ran our program using wdio test runner. We also configured our script for the test, but we are not checking if the page that we are landing on is correct page after sign in. In this chapter, we will write our actual test case

## 5.1 Add assertion to the program

We will be using our `first_program.spec.js` file. This will no longer our first program, so let's change the name of it to the `first_test.spec.js`.

Now to start with we will just check the title of the page using `getTitle()` function. In order to make an assertion, first, let's import `assert` function by

```
const assert = require('assert')
```

Now let's capture the title in some variable shown as below

```
const title = browser.getTitle();
```

Next, we will make an assertion, in layman language, we will check if the title we got is exactly what we are expecting.

While writing test cases it is best practice to fail a test case first and then pass it, so we will give any random title at a start like

```
assert.equal(title, 'Random');
```

Also remove `.pause()` calls from the program as we don't need them anymore. Our test case at this moment will look like


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

Go ahead and run test case using `npm test`. You will see that our test case failed and what is an error on console?

```
F

0 passing (14.10s)
1 failing

1) My first program for test runner My first test:
'Ace Invoice' == 'Random'
running chrome
AssertionError [ERR_ASSERTION]: 'Ace Invoice' == 'Random'
```

This is a way to tell the title of the page is not `Random` that we are expecting. `assert` is comparing `Ace Invoice` with `Random`.

Now correct test case and replace `Random` with `Ace Invoice` and run test again, this time it will pass.

At this time we are only checking the title of the page. But on AceInvoice we will get the title even if login fails right. In the next chapter, we will
right more robust test case about a sign in, so see you in the next chapter.
