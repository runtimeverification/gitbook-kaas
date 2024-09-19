---
description: Step-by-step Guidance on Using the Tool
---

# Getting started

This guide provides step-by-step instructions on how to use **KaaS**. You can use **KaaS** in two ways: as an authorized user or by providing a token. As an authorized user, you can simplify command execution and avoid specifying flags for the vault and token. To access someone else's project, you'll need to provide a key and a unique project identifier.

## **Using the CLI**

### **Login**

You can refer to [login-guide.md](login-guide.md "mention") for the detailed guide on how to log on to the CLI.&#x20;

### **Configuring the CLI**

Configuring the CLI, while not mandatory, can enhance your user experience. Here are the methods:

* **Directly Edit** [**.flaskenv**](https://github.com/runtimeverification/kaas/blob/1eda7067b7910d62856fb79dab10fa3ad1cf357a/kaas-cli/.flaskenv)**:** Modify this file to set your preferences.
* **Setting Environment Variables:** Configure environment variables in your terminal for temporary settings.

### **Uploading and Downloading Proofs within the Default Vault**

**Use Cases:**

* **CI Optimization:** Minimize redundant computations in your CI pipeline by executing them remotely and downloading results as needed.&#x20;
* **Enhanced Collaboration:** Sharing of computation results among team members.
* **Simplified Debugging:** Facilitate storage and sharing of execution results to help troubleshoot failures.

**CLI Commands:**

The main commands for these tasks are `kaas-cli upload` and `kaas-cli download`.&#x20;

{% hint style="info" %}
You will need to be logged in to use these commands. For more information, see the [login-guide.md](login-guide.md "mention") page.
{% endhint %}

**`kaas-cli upload`:**

1. Verifies login status.
2. Uploads files from your default vault to the corresponding S3 location.

**`kaas-cli download`:**

1. Verifies login status.
2. Download updates from S3 to your local machine.

### **Uploading and Downloading Proofs within the Vault ID and Token**

_This section does not require configuration or login, so the previous steps can be disregarded._&#x20;

We need the `Token ID` and `Vault ID` associated with the vault to upload and download project artifacts. \


To obtain `Vault ID` run:  \


```
kaas-cli list-vaults
```

The `id` from the output is your Vault ID. The expected output is:

```
{'id': 'ed8f795e-9d9f-4314-a2cc-8b823be2bcba', 'name': 'Default Vault', 'userId': '19416734', 'createdAt': '2024-05-01T18:13:11.529Z', 'updatedAt': '2024-05-01T18:13:11.529Z', 'deletedAt': None, 'user': {owner data}}
```

We can use a dashboard to view, create, and revoke keys. Go to the [kaas dashboard](https://kaas.runtimeverification.com/app) and click `Manage vault access`  button next to a selected vault.&#x20;



<figure><img src="../.gitbook/assets/image (18).png" alt=""><figcaption></figcaption></figure>

Once you have them, replace `xxx` with your actual `Vault ID` and `Token ID`. Use the following command for uploading:

```bash
kaas-cli upload --token xxx --vault xxx --directory ./kout
```

For downloading, use:

```bash
kaas-cli download --token xxx --vault xxx --directory ./kout
```

{% hint style="info" %}
Use the command `kaas-cli --help` to see all available flags.
{% endhint %}
