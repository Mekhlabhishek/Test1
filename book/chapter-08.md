# Integrating chai library

Chai is an assertion rich library which gives us plenty of options to make the assertion over Node's assertion library.

## 8.1 Installation

Install chai as a package dependency with the following command.

```
npm install --save chai
```

This will install the latest version of chai. Now we are ready to use assertions from the chai library

## 8.2 Replacing the assertions from Node with chai

Replacing the assertion is a simple step. Instead of importing the assertion from node let's import it from chai as

```
const assert = require('chai').assert;
```

That's all, you have integrated chai successfully, go ahead and run test cases once again and make sure they all are passing.

You can also choose the style which you want to use, as `chai` provides two more styles as `expect` and `should`. `expect` style is very popular in the real world as it makes your test cases more readable, but we will stick to `assert` for now. You are free to update the style if you are not comfortable with `assert`.

## 8.3 Quick peek in chai's assertion

Take a look at the assertions that are available to use [here](https://www.chaijs.com/api/assert/). Let's use one of them, `include` in the last test case for creating an organization after signup.

After creating an organization user gets redirected to the *Create a team member* page. Let's test this. First, we will fetch the browser URL and then check if the URL includes `/team/active` in it as follows. Paste the following code in the last test case and run them.

```
const url = browser.getUrl();
assert.include(url, '/team/active');
```

You can try some of them for practice. Also, try `expect` style as a warm up and then jump on to the next chapter.
