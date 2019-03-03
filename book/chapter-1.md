

# 1 Installation

Welcome to the first chapter of learning webdriverio. In this chapter we will get ready with our
platform. So let's fasten our belts and start testing with WebdriverIO

## Check pre-requisite

### 1.1 Check Node is installed

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

### 1.2 Installing selenium-standalone and WebdriverIO

Once you have Node in place let's go ahead and install tools that we will be using to test application.

Create a separate directory by

```
mkdir aceinvoice_web_selenium_tests && cd aceinvoice_web_selenium_tests
```

Initialize npm using following command.

```
npm init -y
```

`-y` is yes for all questions that npm init will ask.


Next, install `webdriverio` by typing

```
npm install --save webdriverio@4
```

_`--save` will add these modules as a project dependencies and `xx@4` is the version number for the package._

We will be installing `selenium-standalone` globally by running following command.

```
npm install selenium-standalone -g
```

Let's install dependencies for selenium by executing following command.
This command will install three things 
1. `selenium-server`
2. `chromewebdriver`, for Chrome
3. `geckodriver`, for firefox

```
selenium-standalone install --version=3.4.0
```


Verify that you installed the selenium correctly by executing following command.

```
selenium-standalone start --version=3.4.0
```

This will start the selenium server on port `4444`. Try visiting `http://localhost:4444` and you will see selenium webpage.

Hola! That's it. Now you are now ready to write your first program.
