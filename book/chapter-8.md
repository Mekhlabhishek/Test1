# Integrating chai library

Chai is assertion rich library which gives us the plenty of the options to make the assertion over Node's assertion library.

## Installation

Install chai as package dependency with the following command.

```
npm install --save chai
```

This will install latest version of chai. Now we are ready to use assertions from the chai library

## Replacing the assertions from Node with chai

Replacing the assertion is simple step. Instead of importing the assertion from node let's import it from chai as

```
const assert = require('chai').assert;
```

That's all, you have integrated chai successfully, go ahead and run test cases once again and make sure they all are passing.

You can also choose the which style you want to use as `chai` provide other two styles as `expect` and `should`. `expect` style is very popular in real world as it makes your test cases more readable, but we will stick to the assert for now, you are free to update the style if you are not comfortable with the `assert`.

## Quick peak in chai's assertion

Take a look at assertion that are available to use [here](https://www.chaijs.com/api/assert/). Let's use one of them, `include` in the last test case for creating a organization after signup.

After creating a organization user get's redirected to the create a team member page. Let's test this. First we will fetch the browser URL and then check if URL includes `/team/active` in it as follows. Paste following code in last test case and run them.

```
const url = browser.getUrl();
assert.include(url, '/team/active');
```

You can try some of them for practise. Also try `expect` style as a warm up and then jump on to the next chapter.
