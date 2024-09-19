---
description: Step-by-step Guidance on Using the Tool
---

# Getting started

This guide provides step-by-step instructions on how to use **KaaS**. You can use **KaaS** in two ways: as an authorized user or by providing a token. As an authorized user, you can simplify command execution and avoid specifying a token flag. To access someone else's project, you'll need to be a part of the same GitHub Organisation.

## **Using the CLI**

### **Login**

You can refer to [authentification-using-device-flow.md](authentification-using-device-flow.md "mention") for the detailed guide on how to log on to the CLI.&#x20;

### **Creating and Using a Token**

Providing token allows you to skip authentication flow and have same level of access as authenticated users. To create and use a token for accessing the KaaS CLI, follow these steps:

1. **Create a Token**:
   * Go to the [KaaS dashboard](https://kaas.runtimeverification.com/app).
   * Click on `Profile` picture in right top corner
   * Click on the `Manage access` button in the drop-down menu.
   * Create a new token and copy the `Key`.
2. **Obtain Organisation Name and Vault Name:**
   * Go to the [KaaS dashboard](https://kaas.runtimeverification.com/app).
   * Click `View` next to the selected organization.
   * And click on selected Vault.
   * Copy the vault name with organisation name. It has format:
     * Vault: `organisation-name/vault-name`
3. **Use the Token**:
   * Replace `xxx` with your actual token from previous steps.
   * Replace `tag` - with short identifier
   *   Use the following command for uploading:

       ```
           kaas-cli upload organisation-name/vault-name:tag --token xxx --directory ./kout
       ```
   *   For downloading, use:

       ```
       kaas-cli download organisation-name/vault-name:tag --token xxx --directory ./kout
       ```







{% hint style="info" %}
Use the command `kaas-cli upload --help` and `kaas-cli download --help` to see all available flags.
{% endhint %}
