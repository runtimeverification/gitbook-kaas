# **`kaas-cli run` Command Documentation**

## **Description:**  
The `kaas-cli run` command executes tests either locally, within a Docker container, or remotely on KaaS infrastructure. It supports multiple test modes including Kontrol (formal verification), Forge, Go fuzzing, and Rust fuzzing.

## **Requirements:**
To run your tests you will need:
- For remote runs you must have a valid internet connection and license with the KaaS online platform at [more info on the website.](https://kaas.runtimeverification.com)
OR
- Kontrol must be installed locally. See [Kontrol documentation](https://docs.runtimeverification.com/kontrol/overview/readme/installations) for installation instructions.
OR 
- Docker must be installed locally. See [Docker documentation](https://docs.docker.com/get-docker/) for installation instructions.

**For Kontrol/Forge modes:**
- You must have at minimum a `foundry.toml` file at the root of you TEST directory.
- A `kontrol.toml` file is optional, but if present, it will be used to run the tests using specific configuration options passed to kontrol. 

**For Go/Rust fuzzing modes:**
- A valid vault specification, token, and branch are required (remote only).
- Go fuzz tests must follow Go's `testing.F` convention (`FuzzXxx(f *testing.F)`).
- Rust fuzz tests require `cargo-fuzz` with a supported engine (libfuzzer, afl, hongfuzz).

## **Test Modes:**

`kaas-cli run` supports the following test modes via `--test-mode`:

- **kontrol** (default): local, container, or remote execution
- **forge**: local or container execution
- **go**: remote execution only — runs Go fuzz tests
- **rust**: remote execution only — runs Rust fuzz tests

Mode-specific flags:

| Mode | Flags |
|------|-------|
| kontrol | `--build-only`, `--prove-only-profile`, `--kontrol-version`, `--kontrol-docker-image`, `--extra-build-args`, `--extra-prove-args` |
| forge | `--foundry-version`, `--foundry-docker-image`, `--extra-build-args`, `--extra-test-args` |
| go | `--fuzz-targets`, `--go-version`, `--go-build-directory`, `--execution-timeout` |
| rust | `--fuzz-targets`, `--rust-version`, `--rust-build-directory`, `--rust-fuzz-engine`, `--rust-fuzz-args`, `--execution-timeout` |

## **Usage:**

```bash
kaas-cli run [OPTIONS]
```
Kaas CLI will attempt to run the tests within the current working directory. So long as a foundry.toml file is present, it will be used to run the tests.
If no foundry.toml file is present, it will exit. 
If a kontrol.toml file is present, it will be used to run the tests using specific configuration options passed to kontrol. 

If your execution directory for tests are within a subdirectory, either `cd` to that directory or use the `--test-root` flag to specify the path to the tests.

**Key Environment Variables:**
- `FOUNDRY_PROFILE`:   
   Set the preferred profile for your kontrol tests.  
   Example: `export FOUNDRY_PROFILE=kprove`  
   
   Otherwise, the default profile will be used. And if no 'out' definition is provided here kaas-cli will fallback to using 'out' for dumping all kcfg proof files. 

**Key Options:**

- `--mode, -m`: Specifies the execution mode. Accepted values are `local`, `remote`, or `container`.
    
    - **local**: Runs Kontrol proofs directly on the host machine. Requires Kontrol to be installed locally, see [Kontrol documentation](https://docs.runtimeverification.com/kontrol/overview/readme/installations) for installation instructions.
    - **container**: Executes proofs inside a Docker container on the host machine, ensuring a consistent and isolated environment.
    - **remote**: Submits proofs to KaaS remote compute infrastructure, requiring authentication and a valid vault specification.

- `--test-mode, -tm`: Specifies the test runtime. Accepted values are `kontrol`, `forge`, `go`, `rust`. Default is `kontrol`.
    
- `--watch, -w`: Monitors the job execution status. If set, waits until the job is completed, applicable in all modes.
    
- `--build-only, -bo`: Only runs `kontrol build` without executing `kontrol prove`.
    
- `--prove-only-profile, -pop`: Runs `kontrol prove` on a previously built proof, without executing `kontrol build`. Requires a profile name.
    
- `--kontrol-version, -kv`: Specifies the Kontrol version. Defaults to "v0.0.0" if not provided ([Kontrol releases on GitHub](https://github.com/runtimeverification/kontrol/releases)).
    
- `--kontrol-docker-image, -kdi`: Specifies the Docker image for running Kontrol in container mode.
    
- `--url, -u`: The API server URL. Defaults to the configured server URL and is typically used for development or testing environments.
    
- `--vault-spec, -vs`: Required in `remote` mode. Specifies the vault in `<ORG_NAME>/<VAULT_NAME>` format.
    
- `--token, -t`: A personal access key for authentication against the remote server. If not provided, you must authenticate using [environment variables](https://docs.runtimeverification.com/kaas/guides/kaas-cli_connecting-using-tokens#authentication-using-environment-variables) or [device flow authentication](https://docs.runtimeverification.com/kaas/guides/kaas-cli_connecting-using-device-flow).
    
- `--branch, -b`: (Optional) Specifies a Git branch for remote runs, aiding in reproducible CI/CD workflows.
    
- `--test-root, -tr`: Specifies the test folder root path relative to the project root.
    
- `--extra-build-args, -eb`: Additional arguments passed directly to `kontrol build`. For available options, consult the [Kontrol Build Options](https://docs.runtimeverification.com/kontrol/glossary/kontrol-build-options) documentation.

- `--fuzz-targets`: (go/rust) Comma-separated list of fuzz targets (max 5). Each target starts a separate remote job.

- `--execution-timeout`: (go/rust) Execution timeout in minutes. Default is 480 (8 hours).


## **Execution Flow:**

1. **Local Mode:**  
    Calls `kontrol build` and `kontrol prove` directly on the host. Kontrol must be installed beforehand. If Kontrol is not found, the CLI will provide installation guidance ([Kontrol Documentation]( https://docs.runtimeverification.com/kontrol/overview/readme/installations)).
    
2. **Container Mode:**  
    Runs on the host machine. Uses a Docker container to run `kontrol build` and `kontrol prove`. This ensures an isolated environment with Kontrol installed without modifying the host system.
    
3. **Remote Mode:**  
    Uses KaasClient to run proofs on the remote KaaS infrastructure. Requires `--vault-spec` and `--token`. If `--watch` is set, the CLI monitors the job until completion. For Go and Rust fuzzing modes, the CLI checks available fuzzing capacity before submitting jobs.
    

## **Error Handling and Output:**

- `--verbose, -v` enables additional output useful for troubleshooting.

___
**Example (Local Mode):**

```bash
kaas-cli run --mode local --extra-build-args "--verbose" --extra-prove-args "--xml-test-report"
```

**Example (Remote Mode - Kontrol):**

```bash
kaas-cli run --mode remote --vault-spec <YOUR_ORGANIZATION>/<YOUR_VAULT>:<OPTIONAL_TAG> --token <YOUR_PERSONAL_ACCESS_KEY> --watch
```

**Example (Remote Mode - Go Fuzzing):**

```bash
kaas-cli run \
  --mode remote \
  --test-mode go \
  --vault-spec org/vault \
  --token "$KAAS_TOKEN" \
  --branch main \
  --fuzz-targets "^FuzzTransfer$,^FuzzMint$" \
  --go-version 1.23.8 \
  --go-build-directory "./..."
```

**Example (Remote Mode - Rust Fuzzing):**

```bash
kaas-cli run \
  --mode remote \
  --test-mode rust \
  --vault-spec org/vault \
  --token "$KAAS_TOKEN" \
  --branch main \
  --fuzz-targets "fuzz_transfer,fuzz_mint" \
  --rust-version stable \
  --rust-build-directory "." \
  --rust-fuzz-engine libfuzzer
```

For the `kaas go test` shorthand and Go fuzzing details, see [Remote Fuzzing](/guides/kaas-cli_remote_fuzzing.md).
