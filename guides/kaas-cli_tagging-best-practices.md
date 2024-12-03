---
description: Best Practices for Tagging KCFG Files
---

# KCFG Tagging Best Practices

After successfully [setting up and logging into the CLI](/guides/kaas-cli_connecting-using-tokens.md), you can view and download all previous versions of artifacts.&#x20;

### CLI

To view a list of available versions for specific `Vault ID` use this command:

```
 kaas-cli list-caches <ORG_NAME>/<VAULT_NAME>
```

The output will be similar to this:


You can run a simple `curl` command or download it in the browser or download the cached files using the CLI tool.  
To do this, run the following command:

```
kaas-cli download <ORG_NAME>/<VAULT_NAME>:<TAG>
```

### Web view

The webpage provides a list of available versions. To access this information, navigate to your "Dashboard" and click the eye button next to your preferred project.

<figure><img src="/.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

You will be redirected to a page with all available caches within the vault.

<figure><img src="/.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

Click the "Download" button to see the CLI command or download it in the browser.

<figure><img src="/.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

## Options for Tagging

Tagging is user driven and best pratices can be defined by the user or business needs. 
The tagging system is intended to be used as a way to identify different versions of KCFG files. 
Named for sharing proofs and proof inspection between team members.


## Additional Information
- If the same tag is used for multiple uploads, the latest version will be tagged. And the old version will be untagged but not deleted.
- Tags are not required to be unique. But it is recommended to keep them descriptive of the contents.
