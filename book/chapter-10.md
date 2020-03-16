# Polishing code

Ok, so with our last chapter we saw how to define getters and elements. With all the data in the same file doesn't look good. Let's separate our code based on the scope.

First, we will create two more folders at the same level where our `specs` folder is namely, `selectors` and `getters`. You can choose any name you want, we found these names pretty easy to understand and get an idea about what folder exactly contains.

## 10.1 Creating a selector file

First, we will create a file in the `selectors` folder with the name `sign_up_selectors.js`. Let's move all our selectors to this file and export them from this file as

```
const signUpSelectors = {
  signUpButtonSelector: "//strong[contains(text(),'Sign Up')]",
  emailInputSelector: "input[name='email']",
  getStartButton": "//input[@value='Get Started'],
  passwordInputSelector: "input[name='password']",
  confirmPasswordInputSelector: "input[name='password_confirmation']",
  firstNameInputSelector: "input[name='user[first_name]']",
  lastNameInputSelector: "input[name='user[last_name]']",
  timeZoneDropdownSelector: "select[name='user[time_zone]']",
  dateFormatSelector: "//div[contains(text(),'MM/DD/YYYY')]",
  startWeekSelector: "//div[contains(text(),'Monday')]",
  basicDetails: "//h1[contains(text(),'Basic details')]",
  addYourPreference: "//span[contains(text(),'Add your details and preferences.')]",
  byCheckingTextCheckBox: "//input[@name='user[terms_of_service_accepted]']",
  continue: "//input[@value='Continue']"
  organizationNameInputSelector: "input[name='name']",
  organizationSubmitButtonInput: "//input[@value='Continue']",
  skipThisStep: "//a[contains(text(),'Skip this step')]",
  continueButton: "//input[@value='Continue']"
}

module.exports = signUpSelectors;
```

So as you can see we created one single hash object with all the selectors and then exported it.

## 10.2 Creating a getter file

Similar to the selector create a getter file in `getters` folder with the name `sign_up_getters.js`. In this file first, we will import the selectors as

```
const signUpSelectors = require('../selectors/sign_up_selectors');
```

Next, same like selector, we will define a hash object and export it like

```
const signUpGetters = {
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
}

module.exports = signUpGetters;
```

Now back to our test case file, we will remove all the declarations that we made about selectors and getters and only import `sign_up_getters.js` like

```
const signUpGetters = require('../getters/sign_up_getters');
```

and now we will use this getter object from an imported file to get elements like

```
signUpGetters.primaryButton.click();
```

## 10.3 Setting babel

Babel is the next-gen JS compiler and allows source code transformations. To write test cases using next-gen JS install all necessary dependencies using.

```
npm install --save @babel/core @babel/cli @babel/preset-env @babel/register
```

## 10.4 Creating a babel config file

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

## 10.5 Add a hook in wdio config

Next step is to tell `wdio` to use babel to compile all of our JS files. We will use the before hook from `wdio.config.js`. So uncomment the before hook in `wdio` config file and add `require('@babel/register');` to the function. Your before hook should look like

```
before: function (capabilities, specs) {
    require('@babel/register');
},
```

## 10.6 Setup babel for mocha

As the last step, we will set mocha to use babel compiler by adding config in `mochaOpts`, which is `compilers: ['js:@babel/register']`. Add this config right after `UI` option and we are done with babel setup.

With babel setup in place, we can change the JS code in our test cases. First, we will rewrite how we export our data from the files.

Instead of exporting using `module.exports`, we will export it with `export default`. So now your selector file will have an export statement as

```
export default signUpSelectors;
```

With this, we can change the import statement `const signUpSelectors = require('../selectors/sign_up_selectors');` in getter file to

```
import signUpSelectors from '../selectors/sign_up_selectors';
```

For importing assert from chai though we will have to use a little twist in the syntax like

```
import { assert } from 'chai';
```

_When a library is exporting multiple data instead of default data then wrapping what we want in `{}` makes sure that we will import only what we need._

With babel and other settings in place, our code now should look like

```
import { assert } from 'chai';
import signUpGetters from '../getters/sign_up_getters';

describe('AceInvoice Signup', () => {
  it('URL has sign_up and qa.aceinvoice.com as a server address', () => {
    browser.url('./');
    signUpGetters.signUpLink.click();

    var url = browser.getUrl();
    assert.equal(url, 'https://qa.aceinvoice.com/sign_up');
  });

  it('Navigates to password page after adding an valid email', () => {
    signUpGetters.emailInput.setValue(`test${Math.random()}@webdriverio.com`);
    signUpGetters.primaryButton.click();

    var passwordInputHeight = signUpGetters.passwordInput.getCssProperty('height');
    assert.notEqual(passwordInputHeight.parsed.value, 0);
  });

  it('Creates a user with the email id and password', () => {
    signUpGetters.passwordInput.setValue('welcome');
    signUpGetters.confirmPasswordInput.setValue('welcome');
    signUpGetters.primaryButton.click();
    browser.pause(500);
    var headerText = signUpGetters.pageHeader.getText();
    assert.equal(headerText, 'Basic details\nCreate your profile by adding your personal details and setting some of the app preferences');
  });

  it('Create preferences', () => {
    signUpGetters.firstNameInput.waitForVisible(5000);
    signUpGetters.firstNameInput.setValue('test');
    signUpGetters.lastNameInput.setValue('webdriverio');


    signUpGetters.timeZoneDropdown.waitForVisible(2000)
    browser.waitUntil(() => signUpGetters.timeZoneDropdown.getText().length > 1000, 3000);
    var timezoneSelector = signUpGetters.timeZoneDropdown;
    timezoneSelector.selectByAttribute('value', 'Mumbai');

    var dateSelector = signUpGetters.dateFormatDropdown;
    dateSelector.selectByAttribute('value', '%m/%d/%Y');

    var startSelector = signUpGetters.startWeekDropdown;
    startSelector.selectByAttribute('value', 'Monday');
    signUpGetters.primaryButton.click();

    signUpGetters.organizationNameInput.waitForVisible(3000);
    const createOrgHeader = signUpGetters.pageHeader.getText();
    assert.equal(createOrgHeader, 'Add New Organization');
  });

  it('Creates an organization', () => {
    signUpGetters.organizationNameInput.waitForVisible(3000);
    signUpGetters.organizationNameInput.setValue('WebdriverIO');
    signUpGetters.organizationEmailInput.setValue('test2@webdriverio.com');
    signUpGetters.primaryButton.click();
    signUpGetters.card.waitForVisible(3000);

    const name = signUpGetters.card.getText();
    assert.equal(name, 'Ace Invoice will be sending emails to your client and your team members once you start using this application. If they reply to those emails this is where the replied emails will come.');

    const url = browser.getUrl();
    assert.include(url, '/team/active');
  });
});
```

In the next chapter, we will look into how to define page elements and how to use them.
