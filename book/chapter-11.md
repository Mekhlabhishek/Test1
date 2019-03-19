# Page Elements and Page Objects

In the last chapter, we saw how to define a browser element. Page objects are similar to browser elements with few tweaks.
In page objects, we can define a class and custom method.

## 11.1 Creating a page

Create a folder named `pages` at the level of `specs` folder. Inside that folder create two files with name `sign_in.page.js` and `sign_up.page.js`.
Now, let's define a class in each of the files as follows

```
# sign_in.page.js

class SignInPage {

}

# sign_up.page.js

class SignUpPage {

}
```

Now, let's move code related to the sign-in page to `signInPage` class first. To do this we need to import signUpSelectors into the file. After importing the selector in file move all the getters related to the sign-in page in class as follows

```
import signUpSelectors from '../selectors/sign_up_selectors';

class SignInPage {
  get signUpLink() { return $(signUpSelectors.signUpButtonSelector) }
  get primaryButton() { return $(signUpSelectors.primaryButtonSelector); }
}
```

As you can see we haven't exported our class yet, we need to export the object of the class in order to use it directly in test cases. Add export statement at the end of the file as

```
export default new signInPage();
```

## 11.2 Page actions

Now, on the sign-in page in order to redirect to the sign-up page, we need to take action and click on a link on a page. Page action is nothing but a method in a class, by calling that method we can perform our actions. Let's define our click action first as follows

```
clickSignUpLink() {
    this.signUpLink.click();
  }
```

Final code in `sign_in.page.js` will look as follows.

```
import signUpSelectors from '../selectors/sign_up_selectors';

class SignInPage {
  get signUpLink() { return $(signUpSelectors.signUpButtonSelector) }
  get primaryButton() { return $(signUpSelectors.primaryButtonSelector); }

  clickSignUpLink() {
    this.signUpLink.click();
  }
}

export default new SignInPage();
```

## 11.3 Use case in test cases

Import class object that we exported as

```
import signInPage from '../pages/sign_in.page';
```

Now, with this import in place, our very first test case will look like

```
it('URL has sign_up and staging.aceinvoice.com as a server address', () => {
  browser.url('./');
  signInPage.clickSignUpLink();

  var url = browser.getUrl();
  assert.equal(url, 'https://staging.aceinvoice.com/sign_up');
});
```

## 11.4 Miscellaneous

Now page for sign-in is in place let's create a page for sign-up as follows

```
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
  }

  enterPassword(password) {
    if (password) {
      this.passwordInput.setValue(password);
      this.confirmPasswordInput.setValue(password);
    }
    this.primaryButton.click();
  }
}

export default new SignUpPage();
```

With sign-up page object is in place let's use it in test cases. Our very next test case will look like as follows

```
it('Navigates to password page after adding an valid email', () => {
    signUpPage.enterEmail(`test${Math.random()}@webdriverio.com`);
    var passwordInputHeight = signUpPage.passwordInput.getCssProperty('height');
    assert.notEqual(passwordInputHeight.parsed.value, 0);
  });

  it('Creates a user with the email id and password', () => {
    signUpPage.enterPassword('welcome');
    signUpGetter.pageHeader.waitForVisible(5000);
    var headerText = signUpGetter.pageHeader.getText();
    assert.equal(headerText, 'Enter your Preferences');
  });
```

At this point, you will see we are still using `signUpGetter` at some places. Give it a try and convert all test cases to use page objects. You can check your code if you get stuck [here](https://github.com/bigbinary/learn-webdriverio-book/blob/21-content-about-page-pbjects/book/miscellaneous.md).
