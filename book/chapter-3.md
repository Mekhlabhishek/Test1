We ran our first program successfully in the last chapter and saw webdriverio in action.

In this chapter, we will be setting up our test runner to run test cases. 

## Install wdio-cli

In order to run our test suite we need wdio cli, let's install it.
`--save` is an option for npm to add package as a project dependency.

```bash
$ npm install --save @wdio/cli
```


## Creating a config file

When we installed the package, it got saved under `node_modules/.bin/`. So, in order to create a config file we
will use the wdio package installed under the `node_modules`.

```bash
$ ./node_modules/.bin/wdio config
```

It will ask a bunch of [questions](https://webdriver.io/docs/clioptions.html).
Just hit enter to accept default answer for all the questions except for the one which says

```bash
$ Do you want to add a service to your test setup?
```

Select `selenium-standalone` as an answer for this as we will be running the test cases using the selenium server.

## Correct the config file

After completing those questions dependencies, other packages will get installed on the system. Let's fix the config file that the system just created.
Open `wdio.conf.js` in your favorite text editor and inspect & modify the following to appropriate values.

1. Specs folder

```msg
specs: ['./test/specs/**/*.js']
```

Here we are specifying where wdio should look for the executable files.

2. Browser configuration

```msg
browserName: 'chrome'
```

It says 'firefox' by default, let's change it to 'chrome' as we are using Chrome browser in this course.


3. Settings for log level

```msg
logLevel: 'info'
```

4. Base URL

```msg
baseUrl: 'https://qa.aceinvoice.com'
```

It shows as 'http://localhost' by default, let's change it to 'https://qa.aceinvoice.com'.

Every time when browser instance gets created, it will call out for base URL.

5. Services

```msg
services: ['selenium-standalone']
```

We need to make sure `selenium-standalone` is selected as a test runner service.


6. Path

```msg
path: '\',
```

Delete the path variable if it exists.

_What about other settings? Let's focus on these right now to get started with test runner. We will cover the rest of the settings in detail, in the later part of the course_


## Use selenium as a service

The last part of this chapter is to verify our `package.json` file. At this moment our `package.json` file should look like this (but version of dependencies might differ).

```js
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

```bash
./node_modules/.bin/wdio wdio.conf.js
```

we will see an error on the terminal as

```bash
pattern ./test/specs/**/*.js did not match any file
```

In addition, if we are getting any errors other than the above, may it be while starting selenium server or getting an error for a local runner, install following packages and try again

```bash
npm install --save wdio-selenium-standalone-service selenium-standalone wdio-mocha-framework wdio-local-runner
```

_Report to us if you are still facing issues with the test runner. We would love to help you with issues and improving the quality of this book_

## Creating script command

In our `package.json` file, we will see scripts set as follows

```js
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  }
```

Change it to look like:

```js
"scripts": {
    "test": "./node_modules/.bin/wdio wdio.conf.js"
  },
```

After doing this we can run our tests with command on terminal

```bash
npm test
```

Go ahead and paste command on a terminal and hit enter. We will see an error on a terminal as previous this is because we haven't created our first program/test under `test/specs/`. Hop on to the next chapter where we will run our program with a test runner.
