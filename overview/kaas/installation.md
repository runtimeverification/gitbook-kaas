# Installation

Follow these step-by-step instructions to set up **KaaS** components, which include the web interface and command-line interface (CLI) tool.

## **System Requirements**

Your system will need the following: `python >= 3.10`, `pip >= 20.0.2`, `poetry >= 1.3.2`

## **Sign in - Web Interface** (_Optional)_

You can sign in or sign up using your GitHub account [here](https://kaas.runtimeverification.com/). This will make it easier for you to access your projects.

## **Installation - CLI Tool**

Follow these steps to install the CLI tool:

### Clone the repository

To clone the repository, run the following command:

```
git clone https://github.com/runtimeverification/kaas.git
```

Alternatively, you can use SSH to clone with the command below:

```
git clone git@github.com:runtimeverification/kaas.git
```

Next, you will need to navigate to the project directory. You can do so with the following command:

```
cd kaas/kaas-cli
```

Now that you are in the project directory, you can build the CLI by running the following command:

```
make build
```

### Install  dependencies

You will need to install some dependencies. The installation commands are provided below.&#x20;

Install pip by running:

```
pip install dist/*.whl
```

Install poetry by running:&#x20;

```
poetry install
```

### Activate the Virtual Environment

With the dependencies installed, you can now activate the Virtual Environment by running the following:

```
poetry shell
```

### Test installation

To make sure everything is installed run

```
kaas-cli hello
```

If the installation is successful, you should see the message `Hello stranger!`&#x20;

### **Additional Information**

For detailed guidance on using `kaas-cli`, refer to the [CLI documentation](https://github.com/runtimeverification/kaas/blob/develop/kaas-cli/README.md) on GitHub. Use `kaas-cli --help` to list all commands. For live feedback when using the web interface, use the **Feedback** button located on the right side of the screen. Contact us via our official Telegram or Discord channels for support or inquiries.

