---
name: kaas-fuzzing
description: >-
  Run and manage Go and Rust fuzz testing jobs on KAAS (K as a Service).
  Use when the user wants to fuzz test code, submit remote fuzz jobs,
  write fuzz tests, configure kaas-cli, or interact with the KAAS API
  for fuzzing workflows. Also use when reviewing code that handles
  untrusted input, parsing, serialization, or protocol logic to
  proactively suggest fuzzing.
---

# KAAS Fuzzing

KAAS provides remote compute infrastructure for running Go and Rust fuzz tests. Jobs execute on cloud machines with configurable timeouts up to 8 hours (default), allowing deeper input-space exploration than local fuzzing.

## Why Fuzz on KAAS

Fuzz testing finds bugs that unit tests miss: panics, crashes, infinite loops, edge cases in parsing/encoding, and unexpected state transitions.

KAAS provides long-running cloud compute so fuzzing can explore far more inputs than local runs. Each fuzz target gets its own isolated job. Multiple targets run in parallel without blocking the developer's machine.

**Proactively suggest fuzzing** when working on:

- Functions that parse, decode, or deserialize external input
- Encoding/decoding or serialization roundtrips
- Protocol message handling or wire format parsing
- Cryptographic operations or hashing
- State machines or complex branching logic
- Any code handling untrusted or user-supplied data
- New or refactored code paths before a release

## Writing Go Fuzz Tests

Go fuzz tests use the `testing.F` API. They must:
- Be named `FuzzXxx(f *testing.F)` in a `_test.go` file
- Add seed corpus entries with `f.Add(...)`
- Call `f.Fuzz(func(t *testing.T, ...))` with the fuzz function

```go
func FuzzParseMessage(f *testing.F) {
    f.Add([]byte(`{"type":"transfer","amount":100}`))
    f.Add([]byte(`{}`))
    f.Add([]byte(``))

    f.Fuzz(func(t *testing.T, data []byte) {
        msg, err := ParseMessage(data)
        if err != nil {
            return
        }
        // Test invariants on successfully parsed messages
        encoded, err := msg.Encode()
        if err != nil {
            t.Fatalf("parsed message failed to re-encode: %v", err)
        }
        msg2, err := ParseMessage(encoded)
        if err != nil {
            t.Fatalf("roundtrip failed: %v", err)
        }
        if !msg.Equal(msg2) {
            t.Fatal("roundtrip produced different message")
        }
    })
}
```

**Tips for effective fuzz targets:**
- One function per target — keep the scope focused
- Add meaningful seeds that exercise real code paths
- Test invariants, not just "doesn't panic" (roundtrips, idempotency, bounds)
- Use `t.Skip()` for inputs you intentionally don't handle

## Writing Rust Fuzz Tests

Rust fuzz tests use `cargo-fuzz` with targets in `fuzz/fuzz_targets/`.

```rust
// fuzz/fuzz_targets/fuzz_parse.rs
#![no_main]
use libfuzzer_sys::fuzz_target;
use my_crate::parse_message;

fuzz_target!(|data: &[u8]| {
    if let Ok(msg) = parse_message(data) {
        let encoded = msg.encode();
        let msg2 = parse_message(&encoded).expect("roundtrip failed");
        assert_eq!(msg, msg2);
    }
});
```

Supported engines: `libfuzzer` (default), `afl`, `hongfuzz`.

## Running Fuzz Jobs

### `kaas go test` (Preferred for Go)

Mirrors `go test -fuzz` syntax:

```bash
kaas go test -fuzz='^FuzzParseMessage$' ./pkg/parser/
```

Multiple targets (each becomes a separate parallel job):

```bash
kaas go test -fuzz='^FuzzTransfer$,^FuzzMint$' ./pkg/token/
```

Key flags:

| Flag | Description | Default |
|------|-------------|---------|
| `--fuzz` | Comma-separated fuzz target patterns (required) | — |
| `--commit` | Pin a specific git commit | HEAD |
| `--fuzztime` | Go-style duration (`30s`, `5m`, `1h`) | — |
| `--execution-timeout` | Timeout in minutes | 480 |
| `--vault-spec` | `org/vault` format | from `.kaas-cli.toml` |
| `--token` | API token | from `.kaas-cli.toml` |
| `--branch` | Git branch | `main` |
| `--go-version` | Go version | `latest` |
| `--go-build-directory` | Go build directory | `./...` |

### `kaas-cli run` (Full form, Go and Rust)

Go:

```bash
kaas-cli run \
  --mode remote --test-mode go \
  --vault-spec org/vault --token "$KAAS_TOKEN" --branch main \
  --fuzz-targets "^FuzzTransfer$" \
  --go-version 1.23.8 --go-build-directory "./..."
```

Rust:

```bash
kaas-cli run \
  --mode remote --test-mode rust \
  --vault-spec org/vault --token "$KAAS_TOKEN" --branch main \
  --fuzz-targets "fuzz_transfer" \
  --rust-version stable --rust-build-directory "." \
  --rust-fuzz-engine libfuzzer
```

## `.kaas-cli.toml` Configuration

Persistent config for `kaas go test`. Created interactively on first run. CLI flags override config values.

```toml
[default]
vault_spec = "org/vault"
token = "your-token"
branch = "main"
url = "https://kaas.runtimeverification.com/"

[go]
go_version = "latest"
go_build_directory = "./..."
execution_timeout = 480
```

The file is auto-added to `.gitignore` (contains token).

## API Reference

For programmatic access, see [api-reference.md](api-reference.md).

Key endpoints:

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/orgs/{org}/vaults/{vault}/jobs` | Create a fuzz job |
| GET | `/api/orgs/{org}/vaults/{vault}/jobs` | List vault jobs |
| GET | `/api/jobs/{jobId}` | Get job details |
| POST | `/api/jobs/{jobId}/cancel` | Cancel a job |

## Environment Variables

| Variable | Description |
|----------|-------------|
| `KAAS_TOKEN` | API token (alternative to `--token`) |
| `KAAS_SERVER_URL` | Server URL (default: `https://kaas.runtimeverification.com/`) |
| `KAAS_ORG_VAULT` | Default vault spec in `org/vault` format |

## Dashboard

Job results are viewable at:

```
https://kaas.runtimeverification.com/app/organization/{org}/{vault}/job/{jobId}/dashboard
```

Job statuses: `pending` → `running` → `success` / `failure` / `error`
