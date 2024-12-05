# Getting Started with KaaS
---

**Welcome to KaaS!**  
This guide will help you navigate through the initial setup process, including signing up, creating your first organization, and setting up a vault. Follow these steps to get started:

## Step 1: Sign Up

1. **Access the KaaS Web App:**

   Open your web browser and navigate to the [KaaS web application](https://kaas.runtimeverification.com).

2. **Create an Account:**

   - Click on the **Login** button on the homepage.

   **Two Login Options:**
   - **Login with Github (Recommended)**
     - Click on the **Login with Github** button.
     - You will be redirected to the Github login page.
     - After logging in, you will be redirected back to KaaS Dashboard.

   - **Email/Password:**
     - Fill in your details, including your email address and a secure password.
     - Submit the form to create your account.
     - Check your email inbox for a verification email from KaaS.
   
   ![Signin Page](/.gitbook/assets/SignInPage.png)  
   *Screenshot of the sign-in page with fields for email and password.*

## Step 2: Create Your First Organization

1. **Log In to Your Account:**

   Use your credentials to log in to the KaaS web app.

2. **Navigate to Organizations:**
   On first login you will be presented with a blank canvas. 

   ![Blank Canvas](/.gitbook/assets/BlankCanvas.png)

   If someone in your github organization has already imported an organization from Github you are part of.
   It will be listed on login.

### After first login

   #### Add a Github Connected Organization.

   Watch a brief tutorial video adding an organization using the Github integration feature.  
   
   <iframe scrolling='no' frameborder='0' style='width: 944px; height: 717px; border:0;' src='https://app.screencast.com/JpRDbeTHhgRqs/e' allowfullscreen></iframe>  

   **Additional Notes on Github Connected Organizations:**
   - Github integration is required for remote compute execution.
   - Github integration is NOT required for storing kcfgs to a vault.
   - When selecting 'Add Organization' if your organization does not show up within a few seconds, Click "Install Github App" and make sure the app is installed for the organization.
   - If your repository is not showing up in the list of repositories, Check the to baner message and follow the instructions.

   **Troubleshooting Github Repo not showing:**
   - If your repository is not showing up in the list of repositories, Check the to banner message and follow the instructions.  
    ![Github Repo not showing](/.gitbook/assets/AddVault_MissingGithubRepo.png)

   #### Create a Non Github Organization:

   - Click on the **Create New Organization** button. Top Right of screen.
   - Enter the name of your organization. Must Start with '@'
   - Click **Create**

## Step 3: Set Up Your First Vault

1. **Access the Vaults Tab:**

   Click on an Organization. Navigate to the **Vaults** tab.  
   ![Vaults Tab](/.gitbook/assets/VaultsTab.png)   
   *Screenshot of the Vaults tab.*

2. **Connect a Github Repository as a KaaS Vault:**

   - Click on the **Connect Vault** button.
   - Select the repository you want to use for the vault. And click **Connect Icon**.  
   ![Connect Vault Icon](/.gitbook/assets/ConnectVaultIcon.png)  
   *Screenshot of the Connect Vault Icon.*  
   <br>
   <br>
   **Troubleshooting Github Repo not showing:**
   - If your repository is not showing up in the list of repositories, Check the to banner message and follow the instructions.  
    ![Github Repo not showing](/.gitbook/assets/AddVault_MissingGithubRepo.png)



3. **Adding a New Vault by Connecting a Github Repository:**

   A Tutorial on adding a new Github Connected Repository as a KaaS Vault and running a Kontrol Compute Job
   
   <iframe scrolling='no' frameborder='0' style='width: 944px; height: 717px; border:0;' src='https://app.screencast.com/w6Bh2tD4hvu4E/e' allowfullscreen></iframe>

## Questions?

If you have any questions or need further assistance, please refer to our support documentation or contact our support team.
[contact@runtimeverification.com](mailto:contact@runtimeverification.com)  
[Join our Discord](https://discord.gg/UBq4J8NE)  
[Find us on Twitter](https://twitter.com/rv_inc)  


# Next Steps
[Tagging Best Practices](/guides/tagging-best-practices.md)

You are now ready to start using KaaS to manage your projects efficiently. If you have any questions or need further assistance, please refer to our support documentation or contact our support team.