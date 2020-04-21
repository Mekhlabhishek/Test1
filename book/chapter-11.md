In the last chapter, we saw how to define a browser element. Page objects are similar to browser elements with few tweaks.
In page objects, we can define a class and custom method.

## Creating a page

Create a folder named `pages` at the level of `specs` folder. Inside that folder create two files with names `sign_in.page.js` and `sign_up.page.js`.
Now, let's define a class in each of the files as follows

```js
# sign_in.page.js

class SignInPage {

}

# sign_up.page.js

class SignUpPage {

}
```

Now, let's move code related to the sign-in page to `signInPage` class first. To do this we need to import signUpSelectors into the file. After importing the selector in file move all the getters related to the sign-in page in class as follows

```js
import signUpSelectors from '../selectors/sign_up_selectors';

class SignInPage {
  get signUpLink() { return $(signUpSelectors.signUpButtonSelector) }
  get primaryButton() { return $(signUpSelectors.primaryButtonSelector); }
}
```

As you can see we haven't exported our class yet, we need to export the object of the class in order to use it directly in test cases. Add export statement at the end of the file as

```js
export default new SignInPage();
```

## Page actions

Now, on the sign-in page in order to redirect to the sign-up page, we need to take action and click on a link on a page. Page action is nothing but a method in a class, by calling that method we can perform our actions. Let's define our click action first as follows

```js
clickSignUpLink() {
    this.signUpLink.click();
  }
```

Final code in `sign_in.page.js` will look as follows.

```js
import signUpSelectors from '../selectors/sign_up_selectors';

class SignInPage {
  get signUpLink() { return $(signUpSelectors.signUpButtonSelector) }
  get primaryButton() { return $(signUpSelectors.primaryButtonSelector); }

  clickSignUpLink() {
    browser.moveToObject(this.signUpLink);
    browser.pause(100);
    this.signUpLink.click();
  }
}

export default new SignInPage();
```

## Use case in test cases

Import class object that we exported as

```js
import signInPage from '../pages/sign_in.page';
```

Now, with this import in place, our very first test case will look like

```js
it('URL has sign_up and qa.aceinvoice.com as a server address', () => {
  browser.url('./');
  signInPage.clickSignUpLink();

  var url = browser.getUrl();
  assert.equal(url, 'https://qa.aceinvoice.com/sign_up');
});
```

## Miscellaneous

Now page for sign-in is in place, let's create a page for sign-up as follows

```js
import signUpSelectors from "../selectors/sign_up_selectors";

class SignUpPage {
  get emailInput() { return $(signUpSelectors.emailInputSelector); }
  get primaryButton() { return $(signUpSelectors.primaryButtonSelector); }
  get passwordInput() { return $(signUpSelectors.passwordInputSelector); }
  get confirmPasswordInput() { return $(signUpSelectors.confirmPasswordInputSelector); }

  enterEmail(email) {
    if (email) {
      this.emailInput.setValue(email);
    }
    this.primaryButton.click();
    browser.pause(500);
  }

  enterPassword(password) {
    if (password) {
      this.passwordInput.setValue(password);
      this.confirmPasswordInput.setValue(password);
    }
    this.primaryButton.click();
    browser.pause(500);
  }
}

export default new SignUpPage();
```

With sign-up page object is in place, let's use it in test cases. Our very next test case will look like as follows

```js
  it('Navigates to password page after adding an valid email', () => {
    signUpPage.enterEmail(`test${Math.random()}@webdriverio.com`);
    var passwordInputHeight = signUpPage.passwordInput.getCssProperty('height');
    assert.notEqual(passwordInputHeight.parsed.value, 0);
  });

  it('Creates a user with the email id and password', () => {
    signUpPage.enterPassword('welcome');
    signUpGetters.pageHeader.waitForVisible(5000);
    var headerText = signUpGetters.pageHeader.getText();
    assert.equal(headerText, 'Basic details\nAdd your details and preferences.');
  });
```

At this point, you will see we are still using `signUpGetter` at some places. Give it a try and convert all test cases to use page objects. You can check your code if you get stuck [here](https://github.com/bigbinary/learn-webdriverio-book/blob/master/book/miscellaneous.md).
