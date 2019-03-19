### /test/pages/create_organization.page.js

```
import {
  pageHeaderSelector,
  organizationNameInputSelector,
  organizationEmailInputSelector,
  sortingIconSelector,
  primaryButtonSelector
} from '../selectors/sign_up.selectors';

class CreateOrganization {
  get pageHeader() { return $(pageHeaderSelector); }
  get organizationNameInput() { return $(organizationNameInputSelector); }
  get organizationEmailInput() { return $(organizationEmailInputSelector); }
  get sortingIcon() { return $(sortingIconSelector); }
  get primaryButton() { return $(primaryButtonSelector); }
  get pageHeader() { return $(pageHeaderSelector); }

  enterOrganizationData(name, email) {
    this.organizationNameInput.waitForVisible(3000);
    this.organizationNameInput.setValue(name);
    this.organizationEmailInput.setValue(email);
  }

  submit() {
    this.primaryButton.click();
  }

  getPageHeader() {
    this.organizationEmailInput.waitForVisible(5000);
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
  dateFormatDropdownSelector,
  startWeekDropdownSelector,
  primaryButtonSelector,
  pageHeaderSelector
} from '../selectors/sign_up.selectors';

class PreferencePage {
  get firstNameInput() { return $(firstNameInputSelector); }
  get lastNameInput() { return $(lastNameInputSelector); }
  get timeZoneDropdown() { return $(timeZoneDropdownSelector); }
  get dateFormatDropdown() { return $(dateFormatDropdownSelector); }
  get startWeekDropdown() { return $(startWeekDropdownSelector); }
  get primaryButton() { return $(primaryButtonSelector); }
  get pageHeader() { return $(pageHeaderSelector); }

  enterFullName(firstName, lastName) {
    this.firstNameInput.waitForVisible(5000);
    this.firstNameInput.setValue(firstName);
    this.lastNameInput.setValue(lastName);
  }

  setPreferences(timeZone, dateFormat, day) {
    this.timeZoneDropdown.selectByAttribute('value', timeZone);

    this.dateFormatDropdown.selectByAttribute('value', dateFormat);

    this.startWeekDropdown.selectByAttribute('value', day);
  }

  submit() {
    this.primaryButton.click();
  }

  getPageHeader() {
    this.pageHeader.waitForVisible(5000);
    return this.pageHeader.getText()
  }
}

export default new PreferencePage();
```

### /test/pages/sign_in.page.js

```
import {
  signUpButtonSelector,
  primaryButtonSelector
} from '../selectors/sign_up.selectors';

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
} from "../selectors/sign_up.selectors";

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
import { sortingIconSelector } from '../selectors/sign_up.selectors';

class TeamIndexPage {
  get sortingIcon() { return $(sortingIconSelector); }

  getSortingIconText() {
    this.sortingIcon.waitForVisible(5000);
    return this.sortingIcon.getText();
  }
}

export default new TeamIndexPage();
```

### /team/selectors/sign_up.selectors.js

```
export const signUpButtonSelector = ".signup-button.border-radius-lg";
export const emailInputSelector = "input[name='email']";
export const primaryButtonSelector = ".btn.btn-primary";
export const passwordInputSelector = "input[name='password']";
export const confirmPasswordInputSelector = "input[name='password_confirmation']";
export const firstNameInputSelector = "input[name='user[first_name]']";
export const lastNameInputSelector = "input[name='user[last_name]']";
export const timeZoneDropdownSelector = "select[name='user[time_zone]']";
export const dateFormatDropdownSelector = "select[name='user[date_format]']";
export const startWeekDropdownSelector = "select[name='user[start_of_week]']";
export const pageHeaderSelector = ".page-header-left";
export const organizationNameInputSelector = "input[name='name']";
export const organizationEmailInputSelector = "input[name='email']";
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
  it('URL has sign_up and staging.aceinvoice.com as a server address', () => {
    browser.url('./');
    signInPage.clickSignUpLink();
    var url = browser.getUrl();

    assert.equal(url, 'https://staging.aceinvoice.com/sign_up');
  });

  it('Navigates to password page after adding an valid email', () => {
    signUpPage.enterEmail(`test${Math.random()}@webdriverio.com`);
    var passwordInputHeight = signUpPage.passwordInput.getCssProperty('height');

    assert.notEqual(passwordInputHeight.parsed.value, 0);
  });

  it('Creates a user with the email id and password', () => {
    signUpPage.enterPassword('welcome');
    var headerText = preferencePage.getPageHeader();

    assert.equal(headerText, 'Enter your Preferences');
  });

  it('Create preferences', () => {
    preferencePage.enterFullName('test', 'webdriverio');
    browser.waitUntil(() => preferencePage.timeZoneDropdown.getText().length > 1000, 3000);
    preferencePage.setPreferences('Mumbai', '%m/%d/%Y', 'monday');
    preferencePage.submit();
    const createOrgHeader = createOrgPage.getPageHeader();

    assert.equal(createOrgHeader, 'Add New Organization');
  });

  it('Creates an organization', () => {
    createOrgPage.enterOrganizationData('WebdriverIO', 'test@webdriverio.com');
    createOrgPage.submit();
    const name = teamIndexPage.getSortingIconText();
    const url = browser.getUrl();

    assert.equal(name, 'test webdriverio');
    assert.include(url, '/team/active');
  });
});

```
