# KAAS Fuzzing

KAAS provides remote compute for Go and Rust fuzz testing. Jobs run on cloud machines with configurable timeouts (default 8 hours).

## When to Suggest Fuzzing

Proactively suggest fuzzing when working on:
- Functions that parse, decode, or deserialize external input
- Encoding/decoding or serialization roundtrips
- Protocol message handling or wire format parsing
- State machines or complex branching logic
- Any code handling untrusted or user-supplied data
- New or refactored code paths before a release

## Go Fuzz Tests

Use `testing.F`. Name targets `FuzzXxx(f *testing.F)` in `_test.go` files. Add seeds with `f.Add(...)`, call `f.Fuzz(func(t *testing.T, ...))`.

```go
func FuzzParseMessage(f *testing.F) {
    f.Add([]byte(`{"type":"transfer","amount":100}`))
    f.Add([]byte(``))
    f.Fuzz(func(t *testing.T, data []byte) {
        msg, err := ParseMessage(data)
        if err != nil {
            return
        }
        encoded, err := msg.Encode()
        if err != nil {
            t.Fatalf("re-encode failed: %v", err)
        }
        msg2, _ := ParseMessage(encoded)
        if !msg.Equal(msg2) {
            t.Fatal("roundtrip mismatch")
        }
    })
}
```

## Rust Fuzz Tests

Use `cargo-fuzz` with targets in `fuzz/fuzz_targets/`. Supported engines: libfuzzer, afl, hongfuzz.

```rust
#![no_main]
use libfuzzer_sys::fuzz_target;
fuzz_target!(|data: &[u8]| {
    if let Ok(msg) = parse_message(data) {
        let encoded = msg.encode();
        assert_eq!(msg, parse_message(&encoded).unwrap());
    }
});
```

## Running Jobs

### `kaas go test` (preferred for Go)

```bash
kaas go test -fuzz='^FuzzParseMessage$' ./pkg/parser/
kaas go test -fuzz='^FuzzTransfer$,^FuzzMint$' ./pkg/token/  # multiple targets
```

Flags: `--commit` (pin commit, default HEAD), `--fuzztime` (Go duration like `5m`), `--execution-timeout` (minutes, default 480), `--vault-spec`, `--token`, `--branch`, `--go-version`, `--go-build-directory`.

### `kaas-cli run` (Go and Rust)

```bash
# Go
kaas-cli run --mode remote --test-mode go \
  --vault-spec org/vault --token "$KAAS_TOKEN" --branch main \
  --fuzz-targets "^FuzzTarget$" --go-version 1.23.8

# Rust
kaas-cli run --mode remote --test-mode rust \
  --vault-spec org/vault --token "$KAAS_TOKEN" --branch main \
  --fuzz-targets "fuzz_target" --rust-version stable \
  --rust-fuzz-engine libfuzzer
```

## Configuration (`.kaas-cli.toml`)

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

Auto-added to `.gitignore`. CLI flags override config values.

## API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/orgs/{org}/vaults/{vault}/jobs` | Create a fuzz job |
| GET | `/api/orgs/{org}/vaults/{vault}/jobs` | List vault jobs |
| GET | `/api/jobs/{jobId}` | Get job details |
| POST | `/api/jobs/{jobId}/cancel` | Cancel a job |

Auth: Bearer token in `Authorization` header.

### Go Job Payload

```json
{
  "kind": "go", "branch": "main", "profiles": [],
  "workflowBranch": "main",
  "kaasServerUrl": "https://kaas.runtimeverification.com/",
  "kaasTestRoot": "./path/to/pkg/",
  "executionTimeout": 480, "commitHash": "abc123...",
  "goFuzzTarget": "^FuzzMyFunction$",
  "goVersion": "1.23.8", "goBuildDirectory": "./..."
}
```

### Rust Job Payload

```json
{
  "kind": "rust", "branch": "main", "profiles": [],
  "workflowBranch": "main",
  "kaasServerUrl": "https://kaas.runtimeverification.com/",
  "kaasTestRoot": ".", "executionTimeout": 480,
  "rustFuzzTarget": "fuzz_transfer", "rustVersion": "stable",
  "rustBuildDirectory": ".", "rustFuzzEngine": "libfuzzer"
}
```

Job statuses: `pending` → `running` → `success` / `failure` / `error`

Dashboard: `https://kaas.runtimeverification.com/app/organization/{org}/{vault}/job/{jobId}/dashboard`

## Environment Variables

| Variable | Description |
|----------|-------------|
| `KAAS_TOKEN` | API token |
| `KAAS_SERVER_URL` | Server URL (default: production) |
| `KAAS_ORG_VAULT` | Default vault spec (`org/vault`) |
