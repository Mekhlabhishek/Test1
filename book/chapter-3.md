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

Now we have the wdio config file.
However we need to make some changes to the config file.
Open `wdio.conf.js` and let's look at values.

1. Specs folder

The values should be as shown below.
If the value is different then change the value to what is shown below.

```msg
specs: ['./test/specs/**/*.js']
```

2. Browser configuration

If the value is 'firefox' then change it to 'chrome' as we are using Chrome browser in this course.

```msg
browserName: 'chrome'
```

3. Settings for log level

```msg
logLevel: 'silent'
```

If it shows as 'info', then let's change it to 'silent'.

4. Base URL


It shows as 'http://localhost' by default, let's change it to 'https://qa.aceinvoice.com'.
Every time when browser instance gets created, it will call out for base URL.

```msg
baseUrl: 'https://qa.aceinvoice.com'
```

5. Services

We need to make sure `selenium-standalone` is selected as a test runner service.

```msg
services: ['selenium-standalone']
```


6. Path

```msg
path: '\',
```

Delete the path variable if it exists.

What about other settings? Let's focus on these right now to get started with test runner. We will cover the rest of the settings in detail, in the later part of the course.


## Using selenium as a service

The last part of this chapter is to verify our `package.json` file. 
At this moment our `package.json` file should look like this (but version of dependencies might differ).

```js
{
  "name": "aceinvoice-selenium-tests",
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
    "@wdio/cli": "^6.1.3",
    "selenium-standalone": "^6.17.0",
    "webdriverio": "^4.14.4"
  },
  "devDependencies": {
    "@wdio/local-runner": "^6.1.3",
    "@wdio/mocha-framework": "^6.1.0",
    "@wdio/spec-reporter": "^6.0.16",
    "@wdio/sync": "^6.1.0",
    "chromedriver": "^81.0.0",
    "wdio-chromedriver-service": "^6.0.2"
  }
}

```

Execute test by running following command.

```bash
$ ./node_modules/.bin/wdio wdio.conf.js
```

We will see an error in the terminal.

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
