# Polishing code

Ok, so with our last chapter we saw how to define getters and elements. With all the data in same file doesn't look good. Let's separate out our code based on the scope.

First we will create a two more folders at the same level where our `specs` folder is namely, `selectors` and `getters`. You can choose any name you want, we found these names pretty easy to understand and get idea about what folder exactly contains.

## Creating a selector file

First we will create a file in selector folder with the name `sign_up_selectors.js`. Let's move all our selectors to this file and export them from this file as

```
signUpSelectors = {
  signUpButtonSelector: ".signup-button.border-radius-lg",
  emailInputSelector: "input[name='email']",
  primaryButtonSelector: ".btn.btn-primary",
  passwordInputSelector: "input[name='password']",
  confirmPasswordInputSelector: "input[name='password_confirmation']",
  firstNameInputSelector: "input[name='user[first_name]']",
  lastNameInputSelector: "input[name='user[last_name]']",
  timeZoneDropdownSelector: "select[name='user[time_zone]']",
  dateFormatDropdownSelector: "select[name='user[date_format]']",
  startWeekDropdownSelector: "select[name='user[start_of_week]']",
  pageHeaderSelector: ".page-header-left",
  organizationNameInputSelector: "input[name='name']",
  organizationEmailInputSelector: "input[name='email']",
  sortingIconSelector: ".sorting_1",
}

module.exports = signUpSelectors;
```

So as you can see we created one single hash object with all the selectors and then exported it.

## Creating a getter file

Similar to the selector create a getter file in `getters` folder with the name `sign_up_getters.js`. In this file first we will import the selectors as

```
const signUpSelectors = require('../selectors/sign_up_selectors');
```

Next, same like selector we will define a hash object and export it like

```
const signUpGetter = {
  get signUpLink() { return $(signUpSelectors.signUpButtonSelector) },
  get emailInput() { return $(signUpSelectors.emailInputSelector); },
  get primaryButton() { return $(signUpSelectors.primaryButtonSelector); },
  get passwordInput() { return $(signUpSelectors.passwordInputSelector); },
  get confirmPasswordInput() { return $(signUpSelectors.confirmPasswordInputSelector); },
  get firstNameInput() { return $(signUpSelectors.firstNameInputSelector); },
  get lastNameInput() { return $(signUpSelectors.lastNameInputSelector); },
  get timeZoneDropdown() { return $(signUpSelectors.timeZoneDropdownSelector); },
  get dateFormatDropdown() { return $(signUpSelectors.dateFormatDropdownSelector); },
  get startWeekDropdown() { return $(signUpSelectors.startWeekDropdownSelector); },
  get pageHeader() { return $(signUpSelectors.pageHeaderSelector); },
  get organizationNameInput() { return $(signUpSelectors.organizationNameInputSelector); },
  get organizationEmailInput() { return $(signUpSelectors.organizationEmailInputSelector); },
  get sortingIcon() { return $(signUpSelectors.sortingIconSelector); }
}

module.exports = signUpGetter;
```

Now back to our test case file, we will remove all the declarations that we made about selectors and getters and only import `sign_up_getters.js` like

```
const signUpGetter = require('../getters/sign_up_getters');
```

and now we will use this getter object from imported file to get elements like

```
signUpGetter.primaryButton.click();
```

## Setting a babel

Babel is next gen JS compiler and allows source code transformations. To write test cases using next gen JS install all necessary dependencies using.

```
npm install --save @babel/core @babel/cli @babel/preset-env @babel/register
```

## Creating a babel config file

Create a file with name `babel.config.js` at the root of the folder with the following config in it.

```
module.exports = {
    presets: [
        ['@babel/preset-env', {
            targets: {
                node: 8
            }
        }]
    ]
}
```

## Add before hook in wdio config

Next step is to tell wdio use babel to compile all of our JS files. We will use the before hook from `wdio.config.js` so uncomment the before hook in wdio config file and add `require('@babel/register');` to the function. Your before hook should look like

```
before: function (_capabilities, _specs) {
    require('@babel/register');
},
```

## Setup babel for mocha

As a last step, we will set mocha to use babel compiler by adding config in `mochaOpts`, which is `compilers: ['js:@babel/register']`. Add this config right after `ui` option and we are done with babel setup.

With babel setup in place we can change the JS code in our test cases. First we will rewrite how we export our data from the files.

Instead of exporting using `module.exports` we will export it with `export default`. So now your selector file with have export statement as

```
export default signUpSelectors;
```

With this we can change the import statements as well like

```
import signUpSelectors from '../selectors/sign_up_selectors';
```

For importing assert from chai though we will have to use little twist in syntax like

```
import { assert } from 'chai';
```

_When library is exporting multiple data instead of default data then wrapping what we want in `{}` makes sure that we will import only what we need._

With babel and other settings in place our code now should look like

```
import { assert } from 'chai';

import signUpGetter from '../getters/sign_up_getters';

describe('AceInvoice Signup', () => {
  it('URL has sign_up and staging.aceinvoice.com as a server address', () => {
    browser.url('./');
    signUpGetter.signUpLink.click();

    var url = browser.getUrl();
    assert.equal(url, 'https://staging.aceinvoice.com/sign_up');
  });

  it('Navigates to password page after adding an valid email', () => {
    signUpGetter.emailInput.setValue(`test${Math.random()}@webdriverio.com`);
    signUpGetter.primaryButton.click();

    var passwordInputHeight = signUpGetter.passwordInput.getCssProperty('height');
    assert.notEqual(passwordInputHeight.parsed.value, 0);
  });

  it('Creates a user with the email id and password', () => {
    signUpGetter.passwordInput.setValue('welcome');
    signUpGetter.confirmPasswordInput.setValue('welcome');
    signUpGetter.primaryButton.click();
    browser.pause(500);
    var headerText = signUpGetter.pageHeader.getText();
    assert.equal(headerText, 'Enter your Preferences');
  });

  it('Create preferences', () => {
    signUpGetter.firstNameInput.waitForVisible(5000);
    signUpGetter.firstNameInput.setValue('test');
    signUpGetter.lastNameInput.setValue('webdriverio');

    browser.waitUntil(() => signUpGetter.timeZoneDropdown.getText().length > 1000, 3000);
    var timezoneSelector = signUpGetter.timeZoneDropdown;
    timezoneSelector.selectByAttribute('value', 'Mumbai');

    var dateSelector = signUpGetter.dateFormatDropdown;
    dateSelector.selectByAttribute('value', '%m/%d/%Y');

    var startSelector = signUpGetter.startWeekDropdown;
    startSelector.selectByAttribute('value', 'monday');
    signUpGetter.primaryButton.click();

    signUpGetter.organizationNameInput.waitForVisible(3000);
    const createOrgHeader = signUpGetter.pageHeader.getText();
    assert.equal(createOrgHeader, 'Add New Organization');
  });

  it('Creates an organization', () => {
    signUpGetter.organizationNameInput.waitForVisible(3000);
    signUpGetter.organizationNameInput.setValue('WebdriverIO');
    signUpGetter.organizationEmailInput.setValue('test2@webdriverio.com');
    signUpGetter.primaryButton.click();
    signUpGetter.sortingIcon.waitForVisible(3000);

    const name = signUpGetter.sortingIcon.getText();
    assert.equal(name, 'test webdriverio');

    const url = browser.getUrl();
    assert.include(url, '/team/active');
  });
});
```
