# Using KaaS CLI

This guide will walk you through updating  setting up your CI to upload and download proofs using the **`kaas-cli`** tool.

Once set up in your CI pipeline, new proofs will be uploaded automatically with each build, and the latest set of proofs will be downloaded for use in your verification jobs.

First, make sure you have **`kaas-cli`** installed. You can find instructions here: [kaas-cli_installation.md](/overview/kaas/kaas-cli_installation.md "mention").&#x20;

### Know Your Vault Name

To list your vaults you will run the following command:

```bash
kaas-cli list-vaults
```

### Upload Proofs

To upload proofs in CI:

```bash
 kaas-cli upload --token $KAAS_TOKEN --vault $KAAS_VAULT_ID
```

This will upload **all** files from the current directory to your **KaaS** vault.

### Download Proofs

To download proofs in CI:

```bash
kaas-cli download --token $KAAS_TOKEN --vault $KAAS_VAULT_ID
```

You can also specify a subdirectory to upload/download to/from using the `--directory` or `-d`  flag. See the example below:

```bash
kaas-cli upload -d ./kout 
kaas-cli download -d ./kout
```

{% hint style="info" %}
Specifying a subdirectory is optional.
{% endhint %}

### Authentication

\
To verify authentication is working before uploading/downloading you can run the following command:

```bash
kaas-cli check-auth
```

{% hint style="success" %}
Confirm the message:&#x20;

`You are currently authenticated.`
{% endhint %}

### Tips:

* Keep your `vault token` secure. Set it as a secret environment variable in your CI system.
* Run the upload/download commands from the directory containing your `./kout` folder.
* Check for non-zero exit codes in CI and fail the build if needed.
* Reach out to the **KaaS** team if you have any issues with authentication or need assistance.
