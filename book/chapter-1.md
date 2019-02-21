

# 1.1 Installation

Welcome to the first chapter of learning webdriverio. In this chapter we will get ready with our
platform. So let's fasten our belts and start testing with WebdriverIO

## Check pre-requisite

### 1. Check Node is installed

Open `terminal` on your machine and check the version of the node using following command.

```
node -v
```

_This should pring node version >= 8.15.0_

If Node is not present on your machine then this is for you, let's install node on your machine.
There are two ways for installing NodeJS.
First one is to download the desired node version from [download](https://nodejs.org/en/download) site. 
Second one is to install with NVM(Node Version Manager). We recommend installing Node using NVM. This will
allow you to manage the different Node versions on your local machine. Open terminal on you machine and download
NVM and install it by following command.

```
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.34.0/install.sh | bash
```


After completion of the command above you will have to restart your terminal, so go on close your terminal and open it again.

Next verify that NVM is installed on your machine by typing 

```
nvm --version
```

_This should print version >= 0.33_

Good, that you have NVM installed on your machine let's go and install NodeJS as a next step. Keep in mind you should be
always installing the LTS(Long Term Support) version of node. You can find latest LTS version from website. Let's install node by

```
nvm install 10.15.1 # 10.15.1 here is the version of node.
```

After finishing installation check which version you are using currently and which versions are installed on your local machine by typing

```
nvm list
```

This will print output as follows

```
->      v8.15.0
       v10.15.1
        v11.4.0
default -> 8.15.0 (-> v8.15.0)
node -> stable (-> v11.4.0) (default)
stable -> 11.4 (-> v11.4.0) (default)
iojs -> N/A (default)
lts/* -> lts/dubnium (-> v10.15.1)
lts/carbon -> v8.15.0
lts/dubnium -> v10.15.1
```

_To get more idea on how to use NVM, you can always type `nvm` on command line which will print available options to use with NVM._

Intsalling Node also installs `npm` along with it. Verify that npm is installed on your local machine by

```
npm -v
```
_This should print version >= 6.4.1_

### 2. Installing selenium-standalone and WebdriverIO

Once you have Node in place let's go ahead and install tools that we will be using to test application.

Create a separate directory by

```
mkdir learn-webdriverio && cd learn-webdriverio
```

Initialize npm using `npm init -y`
_`-y` is yes for all questions that npm init will ask._

Next, install `webdriverio` by typing

```
npm install --save webdriverio@4
```

_`--save` will add these modules as a project dependencies and `xx@4` is the version number for the package_

For this time we will be installing `selenium-standalone` globally by running `npm install selenium-standalone -g`. Lets install 
dependencies for selenium by `selenium-standalone install --version=3.4.0`

This will install three things 
1. `selenium-server`
2. `chromewebdriver`, for Chrome
3. `geckodriver`, for firefox

Verify that you installed the selenium correctly by command

```
selenium_standalone start --version=3.4.0
```

This will start the selenium server on port `4444`. Try visiting `http://localhost:4444` and you will see selenium webpage.

Hola! That's it. Now you are now ready to write your first test case.


# 1.2 Writing your first test case

Create a file using `touch first_test.js` and paste a following code into it

```
const webdriverio = require('webdriverio');

webdriverio
  .remote({ desiredCapabilities: { browserName: 'firefox' } })
  .init()
  .url('https://www.duckduckgo.com')
  .setValue('#search_form_input_homepage', 'BigBinary')
  .click('#search_button_homepage')
  .getTitle().then(title => { console.log('Title is : ', title) })
  .end();
```

Let's take a quick look at a code and understand it.

We are importing a `webdriverio` first from the node package that we installed earlier by calling 
`const webdriverio = require('webdriverio')`

Next we are creating remote client with some basic options like browser that we want to use.
Take a look at all options [here](https://webdriver.io/docs/options.html). We will be covering those in later part of the book
so you don't have to worry about it now.

After that we are initializing the remote client by calling `init()` method, which will assign the session to the client.

After that we are navingating to the DuckDuckGo search engine by calling `url('https://duckduckgo.com')`.

On the page there will input element with the id `search_form_input_homepage`, we are setting the value to that input field by
`setValue('#search_form_input_homepage', 'BigBinary')` function. This function will actually send the keystrokes to the input field.
If you want to take quick look at what other selectors are available, [visit](https://webdriver.io/docs/selectors.html). Don't worry about them now, 
as said earlier we will taking look at selectors in later part of the course.

After adding a value to the input field we are clicking button on page by function `click('#search_button_homepage')`, and once we hit the button we
are checking the title of the page by `getTitle()` function.

_`then()` is way to resolve a promise you can get a basic idea of a promise in JS [here](https://javascript.info/promise-basics)._
_`title => console.log('Title is : ', title)` is called an arrow function, learn more about arrow functions [here](https://codeburst.io/javascript-arrow-functions-for-beginners-926947fc0cdc)_

After getting title for the webpage we are terminating our session by calling `end()`.

Now this is the time to run our first test case.

Start the `selenium-standalone` server by command in the terminal

```
selenium-standalone start --version=3.4.0
```

Once server starts, open up a new terminal window and navigate to the directory and start a test case by

```
node first_test.js
```

You will see FireFox window popping up navingation to DuckDuckGo, then searching for BigBinary and then closing FireFox window.
And on terminal you will see the output as

```
Title is :  BigBinary at DuckDuckGo
```

That's it, you ran your first test case successfully. It's time to get more serious. In the next chapter we will run test case with the `wdio` testrunner, till that time you can play with the code above and try some permutations and combinations.
