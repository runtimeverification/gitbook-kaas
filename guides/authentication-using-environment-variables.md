# Authentication Using Environment Variables

**Using Environment Variables for Authentication**

To simplify the authentication process, the KaaS CLI tool allows using environment variables to store authentication credentials. Alternatively, you can use Device login. Below are the steps to configure and use environment variables for authentication:

1.  **Setting Up Environment Variables:**

    You need to set up the following environment variables in your operating system:

    * `KAAS_SERVER_URL`: The URL of the KaaS server (for development and testing).
    * `KAAS_DIRECTORY`: The directory path for KaaS configurations. Default is `/out`.
    * `KAAS_KEY`: The key for accessing KaaS services. Can be obtained [here](https://kaas.runtimeverification.com/app/profile/keys).
    * `KAAS_ORG_VAULT`: The organization vault identifier for KaaS in the format `orgname/vaultname`.

    **Example:**

    **On Windows:**

    ```powershell
    setx KAAS_DIRECTORY "your_directory"
    setx KAAS_KEY "your_key"
    setx KAAS_ORG_VAULT "your_org_vault"
    ```

    **On Linux/macOS:**

    ```bash
    export KAAS_DIRECTORY="your_directory"
    export KAAS_KEY="your_key"
    export KAAS_ORG_VAULT="your_org_vault"
    ```
2.  **Using Environment Variables in the CLI Tool:**

    The CLI tool will automatically read the values of `KAAS_SERVER_URL`, `KAAS_DIRECTORY`, `KAAS_KEY`, and `KAAS_ORG_VAULT` from the environment variables. You do not need to pass these values explicitly each time you run a command.

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
    echo %KAAS_KEY%
    echo %KAAS_ORG_VAULT%
    ```

    **On Linux/macOS:**

    ```bash
    echo $KAAS_DIRECTORY
    echo $KAAS_KEY
    echo $KAAS_ORG_VAULT
    ```
