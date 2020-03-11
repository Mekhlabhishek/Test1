

# Installation

Welcome to the first chapter of learning webdriverio. In this chapter, we will ensure that we have the requisite software to start writing our test.


### 1.1 Check Node is installed

Open `terminal` on your machine and check the version of the node using the following command.

```
node -v
```

_This should print node version >= 8.15.0_. 

### 1.2 Installing nodejs


Download the desired node version from the [official](https://nodejs.org/en/download) site.

#### Mac, Linux users

```
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.34.0/install.sh | bash
```

#### Windows users

NVM is not available for Windows directly you need to download the installer from [here](https://github.com/coreybutler/nvm-windows/releases).
At the end of installation you may see following error

```
The system cannot find the file specified
```

To overcome this error, follow steps

1. Go to c:\Users\{username}\AppData\Roaming\nvm directory,
2. copy settings.txt and
3. paste it to c:\ .


After completion of the above command, restart your terminal.

_For windows users, you need to open a terminal with admin access. To open a terminal using admin access, right click on cmd and select `Run as administrator` option._

Next, verify that NVM is installed on your machine by typing

```
nvm --version
```

_This should print version >= 0.33_

Now that NVM is installed, let's install NodeJS as the next step. Always keep in mind to install the LTS(Long Term Support) version of the node. You can find the latest LTS version from the website. Let's install node by

```
nvm install 10.15.1 # 10.15.1 here is the version of node.
```

After finishing the installation, check which version you are using currently and which versions are installed on your local machine by typing

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

_To get more idea on how to use NVM, you can always type `nvm` on the command line which will print available options to use with NVM._

Installing Node also installs `npm` along with it. Verify that npm is installed on your local machine by

```
npm -v
```
_This should print version >= 6.4.1_

Windows user may face error here stating that `npm command not found`. In order to make it go away try following steps.

1. Open control panel
2. Click on `User accounts`
3. click on `Change my environment variables`
4. At the end of the variable add `C:\Program Files\nodejs` and click Ok.
5. Restart command prompt as admin and try again. This time it it should show correct version.

### 1.2 Installing selenium-standalone and WebdriverIO

Once you have Node in place, let's go ahead and install tools that we will be using to test application.


```
mkdir aceinvoice_web_selenium_tests
cd aceinvoice_web_selenium_tests
```

Initialize npm using the following command.

```
npm init -y
```

`-y` is yes for all questions that npm init will ask.


Next, install `webdriverio` by typing

```
npm install --save webdriverio@4
```

_`--save` will add these modules as project dependencies and `xx@4` is the version number for the package._

We will be installing `selenium-standalone` globally by running the following command.

```
npm install selenium-standalone -g
```

Let's install dependencies for selenium by executing the following command.
This will install "selenium-server", "chrome webdriver" for chrome browser and "geckodriver" for Firefox.

```
selenium-standalone install --version=3.4.0
```

Verify that you installed the selenium correctly by executing the following command.

```
selenium-standalone start --version=3.4.0
```

This will start the selenium server on port `4444`.
Open your browser and try to visit [http://localhost:4444](http://localhost:4444)
and you will see the selenium webpage.

Hola! That's it. Now, you are ready to write your first program.
