# Making code generic

In the last chapter, we created a page object and used it in our test cases. If you have still not converted your code, then please take some time and convert all your test cases to use page object and not getters. Sample code is [here](https://github.com/bigbinary/learn-webdriverio-book/blob/master/book/miscellaneous.md)

## 12.1 Creating components

As you can see we are creating a getter for `primaryButton` in almost every page class. At this point, we will take a glimpse of the components.
We will take out the `primaryButton` getter declaration and action related around that.

Create a new folder called `components` and create a file in that folder with the name `form.component.js` and paste the following code into it.

```
import { primaryButtonSelector } from '../selectors/sign_up.selector';

export default class FormComponent {
  get primaryButton() { return $(primaryButtonSelector); }

  submit() {
    this.primaryButton.click();
  }
}
```

Back to our `sign_up.page.js` import this component and inherent in class as follows

```
import FormComponent from '../components/form.component';

class SignUpPage extends FormComponent {
  ...
}
```

Next, remove getter definition for a primary button and `click` action around that getter and instead use `submit` method from FormComponent class. Your final `sign_up.page.js` file should look like

```
import {
  emailInputSelector,
  passwordInputSelector,
  confirmPasswordInputSelector
} from "../selectors/sign_up.selectors";

import FormComponent from '../components/form.component';

class SignUpPage extends FormComponent {
  get emailInput() { return $(emailInputSelector); }
  get passwordInput() { return $(passwordInputSelector); }
  get confirmPasswordInput() { return $(confirmPasswordInputSelector); }

  enterEmail(email) {
    if (email) {
      this.emailInput.setValue(email);
    }
    this.submit();
  }

  enterPassword(password) {
    if (password) {
      this.passwordInput.setValue(password);
      this.confirmPasswordInput.setValue(password);
    }
    this.submit();
  }
}

export default new SignUpPage();
```

Take time and separate all the components from pages.

At this moment you are ready to write test cases for an entire application. For getting started, try writing test cases for sign-in flow. In the next chapter, we will take an overview of how WebdriverIO and Selenium work hand-in-hand.
