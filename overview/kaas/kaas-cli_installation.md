---
title: KaaS CLI Installation
description: Install kaas-cli and local requirements
---

# KaaS CLI Tool Installation

Follow these step-by-step instructions to set up **KaaS** components, which include the web interface and command-line interface (CLI) tool.

## **System Requirements**

Your system will need the following: `python >= 3.10`, `pip >= 20.0.2`, `poetry >= 1.3.2`

## **Dependency Requirements:**
To run your tests you will need:
- Kontrol must be installed locally. See [Kontrol documentation](https://docs.runtimeverification.com/kontrol/overview/readme/installations) for installation instructions.
OR 
- Docker must be installed locally. See [Docker documentation](https://docs.docker.com/get-docker/) for installation instructions.
OR
- For remote runs you must have a valid internet connection and license with the KaaS online platform at [more info on the website.](https://kaas.runtimeverification.com)

- You must have a minimum a `foundry.toml` file at the root of you TEST directory.
- A `kontrol.toml` file is optional, but if present, it will be used to run the tests using specific configuraiton options passed to kontrol. 
  - `kontrol.toml` is a kontrol specific configuration file that can be used to specify additional options for the proof run, and manage what kontrol does or does not do.


## **Installation - CLI Tool**

To install the CLI tool run:

`pip install --user kaas-cli`&#x20;

or:

`sudo pip install kaas-cli`

for uv use 

`uv pip install kaas-cli`

### Test installation

To make sure everything is installed run

```
kaas-cli hello
```

If the installation is successful, you should see the message `Hello stranger!`&#x20;

### **Additional Information**

Use `kaas-cli --help` to list all commands. For live feedback when using the web interface, use the **Feedback** button located on the right side of the screen. Contact us via our official Telegram or Discord channels for support or inquiries.

### Install Latest Version

Uninstall the current version of the CLI tool:

`pip uninstall kaas-cli`

To install the latest version of the CLI tool run:

`pip install --user kaas-cli`
