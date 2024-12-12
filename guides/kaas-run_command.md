# **`kaas-cli run` Command Documentation**

## **Description:**  
The `kaas-cli run` command executes Kontrol proofs either locally, within a Docker container, or remotely on KaaS infrastructure. Kontrol is an open-source formal verification tool for Ethereum smart contracts, maintained by Runtime Verification ([Kontrol GitHub repository](https://github.com/runtimeverification/kontrol)).

## **Usage:**

```bash
kaas-cli run [OPTIONS]
```

**Key Options:**

- `--mode, -m`: Specifies the execution mode. Accepted values are `local`, `remote`, or `container`.
    
    - **local**: Runs Kontrol proofs directly on the host machine. Requires Kontrol to be installed locally, see [Kontrol documentation]( https://docs.runtimeverification.com/kontrol/overview/readme/installations) for installation instructions.
    - **container**: Executes proofs inside a Docker container on the host machine, ensuring a consistent and isolated environment.
    - **remote**: Submits proofs to KaaS remote compute infrastructure, requiring authentication and a valid vault specification.
- `--watch, -w`: When set, and if running in `remote` mode, waits until the remote proof run completes, periodically checking the job status.
    
- `--kontrol-version, -kv`: Specifies the Kontrol version. If not provided, the CLI uses the latest available release ([Kontrol releases on GitHub](https://github.com/runtimeverification/kontrol/releases)).
    
- `--url, -u`: The API server URL. Defaults to the configured server URL and is typically used for development or testing environments.
    
- `--vault-spec, -vs`: Required in `remote` mode. Specifies the vault in `<ORG_NAME>/<VAULT_NAME>` format.
    
- `--token, -t`: A personal access key for authentication against the remote server. If not provided, you must authenticate using [environment variables](https://docs.runtimeverification.com/kaas/guides/kaas-cli_connecting-using-tokens#authentication-using-environment-variables) or [device flow authentication](https://docs.runtimeverification.com/kaas/guides/kaas-cli_connecting-using-device-flow).
    
- `--branch, -b`: (Optional) Specifies a Git branch for remote runs, aiding in reproducible CI/CD workflows.
    
- `--extra-build-args, -eb` and `--extra-prove-args, -ep`: Additional arguments passed directly to `kontrol build` and `kontrol prove`. For available options, consult the [Kontrol Prove Flags](https://docs.runtimeverification.com/kontrol/glossary/kontrol-prove-flags) and [Kontrol Build Options](https://docs.runtimeverification.com/kontrol/glossary/kontrol-build-options) documentation.
    

## **Execution Flow:**

1. **Local Mode:**  
    Calls `kontrol build` and `kontrol prove` directly on the host. Kontrol must be installed beforehand. If Kontrol is not found, the CLI will provide installation guidance ([Kontrol Documentation]( https://docs.runtimeverification.com/kontrol/overview/readme/installations)).
    
2. **Container Mode:**  
    Runs on the host machine. Uses a Docker container to run `kontrol build` and `kontrol prove`. This ensures an isolated environment with Kontrol installed without modifying the host system.
    
3. **Remote Mode:**  
    Uses KaasClient to run proofs on the remote KaaS infrastructure. Requires `--vault-spec` and `--token`. If `--watch` is set, the CLI monitors the job until completion.
    

## **Error Handling and Output:**

- `--verbose, -v` enables additional output useful for troubleshooting.

___
**Example (Local Mode):**

```bash
kaas-cli run --mode local --extra-build-args "--verbose" --extra-prove-args "--xml-test-report"
```

**Example (Remote Mode):**

```bash
kaas-cli run --mode remote --vault-spec <YOUR_ORGANIZATION>/<YOUR_VAULT>:<OPTIONAL_TAG> --token <YOUR_PERSONAL_ACCESS_KEY> --watch
```
