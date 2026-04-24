---
title: Authentication (device flow)
description: Sign in to kaas-cli with GitHub device flow
---

# Authentication with kaas-cli (device flow)

### **Login**

To login, run the following command:&#x20;

```
kaas-cli login
```

You will be prompted with this message:

```
Your user code: A111-B333
Open the link and type your code: https://github.com/login/device
Then hit 'Enter'.
Press Enter to continue or type 'q' to quit: 
```

Follow the link in the browser. On GitHub’s **device activation** page, paste the user code from the terminal, choose **Continue**, then approve the **runtimeverification** OAuth application when prompted (**Authorize runtimeverification** or equivalent).

When activation succeeds, GitHub shows a short confirmation that the device is connected and you can close the browser tab.

You can now return to the terminal where the `cli` is open and click "Enter." You will see this message:

```
You pressed Enter. The application continues...
Access token received. We store it in the cache folder.
```

To confirm that you are logged in, run the following command:

```
kaas-cli check-auth
```

If you are logged in you will see the following output in your terminal:

```
You are currently authenticated.
```
