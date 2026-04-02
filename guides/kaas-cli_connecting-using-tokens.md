---
title: Using KaaS CLI (tokens)
description: Tokens, upload/download, and environment variables
---

# Getting started

This guide provides step-by-step instructions on how to use **KaaS**. You can use **KaaS** in two ways: as an authorized user or by providing a token. As an authorized user, you can simplify command execution and avoid specifying a token flag. To access someone else's project, you'll need to be a part of the same KaaS organization or Vault.

## **Using the CLI**

### **Login**

You can refer to [Authentication (device flow)]({% link guides/kaas-cli_connecting-using-device-flow.md %}) for the detailed guide on how to authenticate using the CLI and GitHub device flow.

### **Creating and Using a Token**

Providing token allows you to skip authentication flow and have same level of access as authenticated users. To create and use a token for accessing the KaaS CLI, follow these steps:

1. **Create a Token**:
   * Go to the [KaaS dashboard](https://kaas.runtimeverification.com/app).
   * Click on `Profile` picture in right top corner
   * Click on the `Access Tokens` button in the drop-down menu.
   * Create a new token and copy the `Key`.
2. **Obtain organization and vault (vault spec):**
   * Open the [KaaS app](https://kaas.runtimeverification.com/app) and select an **organization**, then a **vault** (or read the names from the URL: `/app/organization/{org}/{vault}`).
   * The **vault spec** passed to the CLI is always `organization-name/vault-name` (two path segments, not the optional cache tag). GitHub-linked orgs use the GitHub login as the first segment; user-managed org names include the leading `@`. See [Organizations, Vaults & GitHub App]({% link overview/kaas/kaas-web_setup.md %}) for how orgs and vaults are created.
3. **Use the Token**:
   * Replace `xxx` with your actual token from previous steps.
   * Replace `organization-name/vault-name` with names from the previous step
   * Replace `tag` - with short identifier
   *   Use the following command for uploading:

       ```bash
           kaas-cli upload organization-name/vault-name:tag --token xxx
       ```
   *   For downloading, use:

       ```bash
       kaas-cli download organization-name/vault-name:tag --token xxx
       ```


> **Info.** Use `kaas-cli upload --help` and `kaas-cli download --help` to see all available flags.


# Authentication Using Environment Variables

**Using Environment Variables for Authentication**

To simplify the authentication process, the KaaS CLI tool allows using environment variables to store authentication credentials. Alternatively, you can use Device login. Below are the steps to configure and use environment variables for authentication:

1.  **Setting Up Environment Variables:**

    You need to set up the following environment variables in your operating system:

    * `KAAS_SERVER_URL`: The URL of the KaaS server (for development and testing).
    * `KAAS_DIRECTORY`: The directory path for KaaS configurations. Default is `/out`.
    * `KAAS_TOKEN`: The Token for accessing KaaS services. Can be obtained [here](https://kaas.runtimeverification.com/app/profile/keys).
    * `KAAS_ORG_VAULT`: The organization vault identifier for KaaS in the format `orgname/vaultname`.

    **Example:**

    **On Windows:**

    ```powershell
    setx KAAS_DIRECTORY "your_directory"
    setx KAAS_TOKEN "your_token"
    setx KAAS_ORG_VAULT "your_org_vault"
    ```

    **On Linux/macOS:**

    ```bash
    export KAAS_DIRECTORY="your_directory"
    export KAAS_TOKEN="your_token"
    export KAAS_ORG_VAULT="your_org_vault"
    ```
2.  **Using Environment Variables in the CLI Tool:**

    The CLI tool will automatically read the values of `KAAS_SERVER_URL`, `KAAS_DIRECTORY`, `KAAS_TOKEN`, and `KAAS_ORG_VAULT` from the environment variables. You do not need to pass these values explicitly each time you run a command.

    **Example Command:**

    ```bash
    kaas-cli check-auth
    ```

    In this example, the CLI tool will authenticate using the credentials stored in the environment variables.
3.  **Verifying Environment Variable Setup:**

    You can verify that the environment variables are set correctly by running the following commands:

    **On Windows:**

    ```powershell
    echo %KAAS_DIRECTORY%
    echo %KAAS_TOKEN%
    echo %KAAS_ORG_VAULT%
    ```

    **On Linux/macOS:**

    ```bash
    echo $KAAS_DIRECTORY
    echo $KAAS_TOKEN
    echo $KAAS_ORG_VAULT
    ```
