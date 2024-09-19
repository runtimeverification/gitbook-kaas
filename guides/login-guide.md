# Login Guide

### **Login**

To login run the following command:&#x20;

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

Follow the link in the browser, and you will see the following window:

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

Copy the code from the terminal and paste it into the browser. Then click "Continue." After the code is confirmed, click the "Authorize runtimeverification" button.

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

You will then see the message below.

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

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
