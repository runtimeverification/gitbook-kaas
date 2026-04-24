---
title: KCFG tagging
description: Tags, list-caches, download, and the web UI
---

# KCFG Tagging Best Practices

After successfully [setting up and logging into the CLI]({% link guides/kaas-cli_connecting-using-tokens.md %}), you can view and download all previous versions of artifacts.

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

*In the Dashboard, open a project and use the **eye** icon next to it to see the list of cached versions for that vault.*

You will be redirected to a page with all available caches within the vault.

*That page lists each tagged cache; pick the version you need.*

Click the **Download** button to copy the suggested `kaas-cli download …` command or download the artifact in the browser.

*The download control shows the CLI command and any in-browser download option.*

## Options for Tagging

Tagging is user driven and best pratices can be defined by the user or business needs. 
The tagging system is intended to be used as a way to identify different versions of KCFG files. 
Named for sharing proofs and proof inspection between team members.


## Additional Information
- If the same tag is used for multiple uploads, the latest version will be tagged. And the old version will be untagged but not deleted.
- Tags are not required to be unique. But it is recommended to keep them descriptive of the contents.
