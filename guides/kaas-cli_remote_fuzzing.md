---
title: Remote fuzzing
description: kaas go test and kaas-cli run for Go and Rust fuzzing on KaaS
---

# Remote Fuzzing on KaaS

KaaS supports remote fuzzing for **Go** and **Rust** projects. Fuzz jobs run on cloud compute infrastructure, allowing long-duration fuzzing without blocking your local machine.

Each fuzz target is submitted as a separate remote job, with up to 5 targets per invocation. Results are viewable on the [KaaS dashboard](https://kaas.runtimeverification.com/app).

## Prerequisites

- **`kaas-cli`** installed (`pip install kaas-cli` or `uv pip install kaas-cli`). The same package provides both the **`kaas-cli`** command and the **`kaas`** command group (e.g. `kaas go test`); you do not install them separately.
- A valid KaaS token ([create one here](https://kaas.runtimeverification.com/app/profile/keys))
- A vault connected to your GitHub repository ([setup guide]({% link overview/kaas/kaas-web_setup.md %}))

## `kaas go test` Command

The `kaas go test` command provides a shorthand that mirrors the familiar `go test -fuzz` syntax. It submits Go fuzz tests to run remotely on KaaS infrastructure.

### Basic Usage

```bash
kaas go test -fuzz='^FuzzMyFunction$' ./path/to/pkg/
```

### Multiple Targets

Comma-separated targets each start a separate parallel job:

```bash
kaas go test -fuzz='^FuzzTransfer$,^FuzzMint$' ./path/to/pkg/
```

### Available Flags

| Flag | Description | Default |
|------|-------------|---------|
| `--fuzz`, `-fuzz` | Comma-separated fuzz target patterns (required) | — |
| `--commit` | Pin a specific git commit hash | HEAD |
| `--fuzztime` | Go-style duration of at least `1m` (`'1m'`, `'5m'`, `'1h'`), converted to execution timeout | — |
| `--execution-timeout` | Execution timeout in minutes (mutually exclusive with `--fuzztime`) | 480 |
| `--vault-spec`, `-vs` | Vault specification in `org/vault` format | from config |
| `--token`, `-t` | Personal access key | from config |
| `--branch`, `-b` | Repository branch | `main` |
| `--go-version` | Go version | `latest` |
| `--go-build-directory` | Go build directory/pattern | `./...` |
| `--go-module-root` | Repo-relative directory containing `go.mod` (e.g. `src` for `golang/go`); optional, matches web **Go Module Root** | — |
| `--url`, `-u` | Server URL | `https://kaas.runtimeverification.com/` |
| `--watch`, `-w` | Watch job execution status | `false` |

> **Info.** **`-fuzz` and `--fuzz` are equivalent** (short and long flag for the same option), matching Go’s usual `-flag` style and Click’s long options. Examples below use `-fuzz`; either form works.
>
> **`--fuzztime` and `--execution-timeout` are mutually exclusive.** Use `--fuzztime` for Go-style durations of at least `1m` (e.g. `5m`) or `--execution-timeout` for raw minutes.

### Example with All Options

```bash
kaas go test \
  -fuzz='^FuzzParseTransaction$' \
  ./pkg/parser/ \
  --commit abc123def \
  --fuzztime 2h \
  --vault-spec myorg/myrepo \
  --token "$KAAS_TOKEN" \
  --branch feature/my-branch \
  --go-version 1.23.8
```

For a **`golang/go`** checkout, the main module is under **`src/`**. Use **`--go-module-root src`** and paths **relative to `src/`** for **`./pkg/parser`** and **`--go-build-directory`** (e.g. **`./html/template`**, not **`./src/html/template`**).

## `kaas-cli run` with Go/Rust Fuzzing

For more explicit control or Rust fuzzing, use the full `kaas-cli run` command.

### Go Fuzzing

`kaas-cli run` uses **`--test-root`** / **`-tr`** for the package path (relative to the repository root), **`--fuzz-targets`** (comma-separated, up to five remote jobs per invocation), and **`--execution-timeout`** in **minutes** (there is no `--fuzztime` on this command; use `kaas go test` if you prefer Go-style durations).

```bash
kaas-cli run \
  --mode remote \
  --test-mode go \
  --vault-spec org/vault \
  --token "$KAAS_TOKEN" \
  --branch main \
  --test-root ./pkg/parser \
  --fuzz-targets "^FuzzTransfer$,^FuzzMint$" \
  --go-version 1.23.8 \
  --go-build-directory "./pkg/parser" \
  --execution-timeout 120
```

### Rust Fuzzing

Rust fuzzing supports `libfuzzer`, `afl`, and `hongfuzz` engines.

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

### Rust-Specific Flags

| Flag | Description | Default |
|------|-------------|---------|
| `--rust-version` | Rust toolchain version | `latest` |
| `--rust-build-directory` | Project directory containing `Cargo.toml` | `.` |
| `--rust-fuzz-engine` | Fuzzing engine: `libfuzzer`, `afl`, `hongfuzz` | `libfuzzer` |
| `--rust-fuzz-args` | Extra arguments passed to the fuzz engine | — |
| `--execution-timeout` | Timeout in minutes | `480` |

## Configuration File (`.kaas-cli.toml`)

The `kaas go test` command supports persistent configuration via a `.kaas-cli.toml` file in your project root. If no config exists, the CLI will interactively prompt you to create one.

CLI flags always override config values. The file is automatically added to `.gitignore`.

```toml
[default]
vault_spec = "org/vault"
token = "your-token"
branch = "main"
url = "https://kaas.runtimeverification.com/"

[go]
go_version = "latest"
go_build_directory = "./..."
go_module_root = "" # optional, e.g. "src" for golang/go
execution_timeout = 480
```

> **Warning.** The `.kaas-cli.toml` file contains your API token. It is automatically added to `.gitignore` to prevent accidental commits.

## Viewing Results

After submitting a fuzz job, the CLI outputs a dashboard URL for each job:

```
Started 2 go fuzz job(s):
  [1] target: ^FuzzTransfer$
      jobId:  abc123
      url:    https://kaas.runtimeverification.com/app/organization/org/vault/job/abc123/dashboard
      commit: def456
  [2] target: ^FuzzMint$
      jobId:  xyz789
      url:    https://kaas.runtimeverification.com/app/organization/org/vault/job/xyz789/dashboard
      commit: def456
```

Job statuses: `pending` → `running` → `success` / `failure` / `error`

You can also view all jobs for a vault on the KaaS dashboard under your organization's compute tab.
