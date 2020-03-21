# Introduction to the WebdriverIO test runner

We ran our first program successfully in the last chapter and saw webdriverio in action.

In this chapter, we will be setting up our test runner to run test cases. Follow along to know how.

## 3.1 Install wdio-cli

In order to run our test suite we need wdio cli, let's install it.

```
npm install --save @wdio/cli
```

`--save` is an option for npm to add package as a project dependency.


## 3.2 Creating a config file

When you install the package, it will get saved under `node_modules/.bin/`. So, in order to create a config file we
will use the wdio package installed under the `node_modules`.

Create a configuration file by command

```
./node_modules/.bin/wdio config
```

It will ask a bunch of [questions](https://webdriver.io/docs/gettingstarted.html#generate-configuration-file).
Just hit enter to accept default answer for all the questions except for the one which says

```
Do you want to add a service to your test setup?
```

Select `selenium-standalone` as an answer for this as we will be running the test cases using the selenium server.

## 3.3 Review config file

After completing those questions dependencies, other packages will get installed on the system. Let's review the config file that the system just created.
Open `wdio.conf.js` in your favorite text editor and inspect & modify the following to appropriate values

1. Specs folder

```
specs: ['./test/specs/**/*.js']
```

Here we are specifying where wdio should look for the executable files.

2. Browser configuration

```
browserName: 'chrome'
```

For this course, we will be using the Chrome browser not Firefox.

3. Setting for running specs synchronously

```
sync: true
```

We will be running our test in a synchronous manner, for two reasons. First, it is easier to understand and write test cases in a synchronous manner and second we won't have to check output using `then` callback.

4. Settings for log level

```
logLevel: 'silent'
```

5. Base URL

```
baseUrl: 'https://qa.aceinvoice.com'
```

Every time when browser instance gets created, it will call out for base URL

6. Services

```
services: ['selenium-standalone']
```

Make sure you have `selenium-standalone` as a test runner service for you


7. Path

```
path: '\',
```

Delete the path variable if it exists.

_What about other settings? Let's focus on these right now to get started with test runner. We will cover the rest of the settings in detail, in the later part of the course_


## 3.4 Use selenium as a service

The last part of this chapter is to make sure that your `package.json` file contains correct values. At this moment your `package.json` file should look like this (but version of dependencies might differ).

```
{
  "name": "aceinvoice_web_selenium_tests",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
     "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "@wdio/cli": "^^5.8.3",
    "webdriverio": "^4.6.0"
  },
  "devDependencies": {
    "wdio-dot-reporter": "0.0.10",
    "wdio-mocha-framework": "^0.6.4",
    "wdio-selenium-standalone-service": "0.0.12",
    "wdio-spec-reporter": "~0.1.5"
  }
}

```

Execute test runner by command

```
./node_modules/.bin/wdio wdio.conf.js
```

you will see an error on the terminal as

```
pattern ./test/specs/**/*.js did not match any file
```

In addition, if you are getting any errors other than the above, may it be while starting selenium server or getting an error for a local runner, install following packages and try again

```
npm install --save wdio-selenium-standalone-service selenium-standalone wdio-mocha-framework wdio-local-runner
```

_Report to us if you are still facing issues with the test runner. We would love to help you with issues and improving the quality of this book_

## 3.5 Creating script command

In your `package.json` file you will see scripts set as follows

```
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  }
```

Change it to look like as

```
"scripts": {
    "test": "./node_modules/.bin/wdio wdio.conf.js"
  },
```

After doing this you can run your tests with command on terminal

```
npm test
```

Go ahead and paste command on a terminal and hit enter. You will see an error on a terminal as previous this is because we haven't created our first program/test under `test/specs/`. Hop on to the next chapter where we will run our program with a test runner.
