### /test/pages/create_organization.page.js

```
import {
  pageHeaderSelector,
  organizationNameInputSelector,
  sortingIconSelector,
  primaryButtonSelector
} from '../selectors/sign_up_selectors';

class CreateOrganization {
  get pageHeader() { return $(pageHeaderSelector); }
  get organizationNameInput() { return $(organizationNameInputSelector); }
  get sortingIcon() { return $(sortingIconSelector); }
  get primaryButton() { return $(primaryButtonSelector); }
  get pageHeader() { return $(pageHeaderSelector); }

  enterOrganizationData(name) {
    this.organizationNameInput.waitForVisible(3000);
    this.organizationNameInput.setValue(name);
  }

  submit() {
    this.primaryButton.click();
  }

  getPageHeader() {
    this.pageHeader.waitForVisible(5000);
    return this.pageHeader.getText();
  }
}

export default new CreateOrganization();
```

### test/pages/preference.page.js

```
import {
  firstNameInputSelector,
  lastNameInputSelector,
  timeZoneDropdownSelector,
  dateFormatSelector,
  startWeekSelector,
  agreeToTermsCheckBox,
  primaryButtonSelector,
  pageHeaderSelector
} from '../selectors/sign_up_selectors';

class PreferencePage {
  get firstNameInput() { return $(firstNameInputSelector); }
  get lastNameInput() { return $(lastNameInputSelector); }
  get timeZoneDropdown() { return $(timeZoneDropdownSelector); }
  get dateFormatDropdown() { return $(dateFormatSelector); }
  get startWeekDropdown() { return $(startWeekSelector); }
  get agreeToTermsCheckBox() { return $(agreeToTermsCheckBox); }
  get primaryButton() { return $(primaryButtonSelector); }
  get pageHeader() { return $(pageHeaderSelector); }

  enterFullName(firstName, lastName) {
    this.firstNameInput.waitForVisible(5000);
    this.firstNameInput.setValue(firstName);
    this.lastNameInput.setValue(lastName);
  }

  setPreferences(browser, timeZone, dateFormat, day) {
    this.timeZoneDropdown.waitForVisible(5000);
    browser.waitUntil(() => this.timeZoneDropdown.getText().length > 1000, 3000);
    this.timeZoneDropdown.selectByAttribute('value', timeZone);

    this.dateFormatDropdown.selectByAttribute('value', dateFormat);

    this.startWeekDropdown.selectByAttribute('value', day);

    this.agreeToTermsCheckBox.click();
  }

  submit() {
    this.primaryButton.click();
  }

  getPageHeader() {
    this.pageHeader.waitForVisible(5000);
    return this.pageHeader.getText();
  }
}

export default new PreferencePage();
```

### /test/pages/sign_in.page.js

```
import {
  signUpButtonSelector,
  primaryButtonSelector
} from '../selectors/sign_up_selectors';

class SignInPage {
  get signUpLink() { return $(signUpButtonSelector) }
  get primaryButton() { return $(primaryButtonSelector); }

  clickSignUpLink() {
    this.signUpLink.click();
  }
}

export default new SignInPage();
```

### /test/pages/sign_up.page.js

```
import {
  emailInputSelector,
  primaryButtonSelector,
  passwordInputSelector,
  confirmPasswordInputSelector
} from "../selectors/sign_up_selectors";

class SignUpPage {
  get emailInput() { return $(emailInputSelector); }
  get primaryButton() { return $(primaryButtonSelector); }
  get passwordInput() { return $(passwordInputSelector); }
  get confirmPasswordInput() { return $(confirmPasswordInputSelector); }

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

### /test/pages/team_index.page.js

```
import { sortingIconSelector } from '../selectors/sign_up_selectors';

class TeamIndexPage {
  get sortingIcon() { return $(sortingIconSelector); }

  getSortingIconText() {
    this.sortingIcon.waitForVisible(5000);
    return this.sortingIcon.getText();
  }
}

export default new TeamIndexPage();
```

### /test/selectors/sign_up_selectors.js

```
export const signUpButtonSelector = "//strong[contains(text(),'Sign Up')]";
export const emailInputSelector = "input[name='email']";
export const primaryButtonSelector = ".btn.btn-primary";
export const passwordInputSelector = "input[name='password']";
export const confirmPasswordInputSelector = "input[name='password_confirmation']";
export const firstNameInputSelector = "input[name='user[first_name]']";
export const lastNameInputSelector = "input[name='user[last_name]']";
export const timeZoneDropdownSelector = "select[name='user[time_zone]']";
export const dateFormatSelector: "//div[contains(text(),'MM/DD/YYYY')]";
export const startWeekSelector: "//div[contains(text(),'Monday')]";
export const agreeToTermsCheckBox: "//input[@name='user[terms_of_service_accepted]']/../div[1]",
export const basicDetails: "//h1[contains(text(),'Basic details')]";
export const addYourPreference: "//h1[contains(text(),'Add your details and preferences.')]";
export const organizationNameInputSelector = "input[name='name']";
export const sortingIconSelector = ".sorting_1";
```

### /test/specs/sign_up.spec.js

```
import { assert } from 'chai';

import signInPage from '../pages/sign_in.page';
import signUpPage from '../pages/sign_up.page';
import preferencePage from '../pages/preference.page';
import createOrgPage from '../pages/create_organization.page';
import teamIndexPage from '../pages/team_index.page';

describe('AceInvoice Signup', () => {
  it('URL has sign_up and qa.aceinvoice.com as a server address', () => {
    browser.url('./');
    signInPage.clickSignUpLink();
    var url = browser.getUrl();

    assert.equal(url, 'https://qa.aceinvoice.com/sign_up');
  });

  it('Navigates to password page after adding an valid email', () => {
    signUpPage.enterEmail(`test${Math.random()}@webdriverio.com`);
    var passwordInputHeight = signUpPage.passwordInput.getCssProperty('height');

    assert.notEqual(passwordInputHeight.parsed.value, 0);
  });

  it('Creates a user with the email id and password', () => {
    signUpPage.enterPassword('welcome');
    var headerText = preferencePage.getPageHeader();

    assert.equal(headerText, 'Basic details\nAdd your details and preferences.');
  });

  it('Create preferences', () => {
    preferencePage.enterFullName('test', 'webdriverio');
    preferencePage.setPreferences(browser, 'Mumbai', '%m/%d/%Y', 'Monday');
    preferencePage.submit();
    const createOrgHeader = createOrgPage.getPageHeader();

    assert.equal(createOrgHeader, 'Create your organization\nTo continue please enter your organization details, you can also create multiple organizations.');
  });

  it('Creates an organization', () => {
    createOrgPage.enterOrganizationData('WebdriverIO');
    createOrgPage.submit();
    const name = teamIndexPage.getSortingIconText();
    const url = browser.getUrl();

    assert.equal(name, 'test webdriverio');
    assert.include(url, '/team/active');
  });
});

```
