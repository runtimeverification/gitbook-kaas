# Using KaaS CLI

This guide will walk you through setting up your CI to upload and download proofs using the **`kaas-cli`** tool.

Once set up in your CI pipeline, new proofs will be uploaded automatically with each build, and the latest set of proofs will be downloaded for use in your verification jobs.

First, make sure you have **`kaas-cli`** installed. You can find instructions here: [installation.md](../overview/kaas/installation.md "mention").&#x20;

### Obtain your Vault ID and Token

To list your vaults you will run the following command:

```bash
kaas-cli list-vaults
```

The output will include your `Vaults`, which will be the `id` value. Your `vault key` will be provided separately and it is important to keep that secure.

To obtain a token use `Vault ID` from previous step&#x20;

```
kaas-cli list-keys xxxx-xxxx-xxxx-xxxx-xxxx
```

### Set Environment Variables

Use the following command to set the environment variables in your CI system each time you run CLI (make sure you add your `vault ID` and `vault token`):

{% code overflow="wrap" %}
```bash
SERVER_URL=<kaas server url> DEFAULT_VAULT_ID=<your vault id> DEFAULT_KEY=<your vault key token> kaas-cli <command>

```
{% endcode %}

To persist environment variables run the export command:

{% hint style="info" %}
`SERVER_URL - is optional and used mainly for development.`
{% endhint %}

```bash
export DEFAULT_VAULT_ID=<your vault id>
export DEFAULT_KEY=<your vault key token>
export DEFAULT_DIRECTORY=<your default folder for proofs>

export SERVER_URL=<kaas server url> # optional, for development
```

After `export` you can run:

```bash
kaas-cli <command>
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
