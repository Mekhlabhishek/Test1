# Moving to page objects

In the last chapter, we added a complete test suite for the signup flow, but while achieving this we wrote a lot of duplicate code. Page objects help us to get rid of the duplicate code. The page object is a very interesting topic but at the same time is also complex to understand. So we will not jump onto page objects here. First, we will take the first step towards it in this chapter.

## 9.1 Browser element

So far, we are calling a method on the browser object. This does not look good, webdriverio gives us a way to get an element from the browser and then call a method to it. We can get a hold of the element using `element()` method. This method accepts the selector as a parameter.

With this method in place, we can rewrite our old code

```
browser.click('//strong[contains(text(),'Sign Up')]');
```

as follows

```
browser.element('//strong[contains(text(),'Sign Up')]').click();
```

Webdriverio provides a handy operator to define this as

```
$('//strong[contains(text(),'Sign Up')]').click();
```

Isn't this look like a JQuery code and also this makes the code more readable. Also, we can define a selector as a constant at the top of the code as

```
const signUpButtonSelector = '//strong[contains(text(),'Sign Up')]';
```

and then replace the code for same as

```
$(signUpButtonSelector).click();
```

## 9.2 Introduction to getters

Sometimes we unknowingly define an element declaration even before it is present on the page. In such cases, you might not get a result that you expected. To overcome this `getter` are pretty much handy

## 9.3 Declaring getter

Creating a getter is very simple. Let's take a look at simple getter to start with.

```
const getter = {
  get element() { return $('#password_input') }
}
```

This is similar to defining a hash object with a few twists. We are declaring key with name `element` and this has to be a function. With the above getter defined we can now call it like


```
getter.element.click();
```

Simple right? With this let's change our code to use the getters. Make sure all of your test cases are passing.

```
const assert = require('chai').assert;

const signUpButtonSelector = "//strong[contains(text(),'Sign Up')]";
const emailInputSelector = "input[name='email']";
const primaryButtonSelector = ".btn.btn-primary";
const passwordInputSelector = "input[name='password']";
const confirmPasswordInputSelector = "input[name='password_confirmation']";
const firstNameInputSelector = "input[name='user[first_name]']";
const lastNameInputSelector = "input[name='user[last_name]']";
const timeZoneDropdownSelector = "select[name='user[time_zone]']";
const dateFormatSelector = "//div[contains(text(),'MM/DD/YYYY')]";
const startWeekSelector = "//div[contains(text(),'Monday')]";
const pageHeaderSelector = ".page-header-left";
const organizationNameInputSelector = "input[name='name']";
const organizationEmailInputSelector = "input[name='email']";
const cardSelector = ".card";

const signUp = {
  get signUpLink() { return $(signUpButtonSelector) },
  get emailInput() { return $(emailInputSelector); },
  get primaryButton() { return $(primaryButtonSelector); },
  get passwordInput() { return $(passwordInputSelector); },
  get confirmPasswordInput() { return $(confirmPasswordInputSelector); },
  get firstNameInput() { return $(firstNameInputSelector); },
  get lastNameInput() { return $(lastNameInputSelector); },
  get timeZoneDropdown() { return $(timeZoneDropdownSelector); },
  get dateFormatDropdown() { return $(dateFormatSelector); },
  get startWeekDropdown() { return $(startWeekSelector); },
  get pageHeader() { return $(pageHeaderSelector); },
  get organizationNameInput() { return $(organizationNameInputSelector); },
  get organizationEmailInput() { return $(organizationEmailInputSelector); },
  get card() { return $(cardSelector); }
}

describe('AceInvoice Signup', () => {
  it('URL has sign_up and qa.aceinvoice.com as a server address', () => {
    browser.url('./');
    signUp.signUpLink.click();

    var url = browser.getUrl();
    assert.equal(url, 'https://qa.aceinvoice.com/sign_up');
  });

  it('Navigates to password page after adding an valid email', () => {
    signUp.emailInput.setValue(`test${Math.random()}@webdriverio.com`);
    signUp.primaryButton.click();

    var passwordInputHeight = signUp.passwordInput.getCssProperty('height');
    assert.notEqual(passwordInputHeight.parsed.value, 0);
  });

  it('Creates a user with the email id and password', () => {
    signUp.passwordInput.setValue('welcome');
    signUp.confirmPasswordInput.setValue('welcome');
    signUp.primaryButton.click();
    browser.pause(500);
    var headerText = signUp.pageHeader.getText();
    assert.equal(headerText, 'Basic details\nCreate your profile by adding your personal details and setting some of the app preferences');
  });

  it('Create preferences', () => {
    signUp.firstNameInput.waitForVisible(5000);
    signUp.firstNameInput.setValue('test');
    signUp.lastNameInput.setValue('webdriverio');


    signUp.timeZoneDropdown.waitForVisible(2000)
    browser.waitUntil(() => signUp.timeZoneDropdown.getText().length > 1000, 3000);
    var timezoneSelector = signUp.timeZoneDropdown;
    timezoneSelector.selectByAttribute('value', 'Mumbai');

    var dateSelector = signUp.dateFormatDropdown;
    dateSelector.selectByAttribute('value', '%m/%d/%Y');

    var startSelector = signUp.startWeekDropdown;
    startSelector.selectByAttribute('value', 'Monday');
    signUp.primaryButton.click();

    signUp.organizationNameInput.waitForVisible(3000);
    const createOrgHeader = signUp.pageHeader.getText();
    assert.equal(createOrgHeader, 'Add New Organization');
  });

  it('Creates an organization', () => {
    signUp.organizationNameInput.waitForVisible(3000);
    signUp.organizationNameInput.setValue('WebdriverIO');
    signUp.organizationEmailInput.setValue('test2@webdriverio.com');
    signUp.primaryButton.click();
    signUp.card.waitForVisible(3000);

    const name = signUp.card.getText();
    assert.equal(name, 'Ace Invoice will be sending emails to your client and to your team members once you start using this application. If they reply to those emails this is where the replied emails will come.');

    const url = browser.getUrl();
    assert.include(url, '/team/active');
  });
});
```
