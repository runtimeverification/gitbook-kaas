# KAAS Fuzzing Knowledge

KAAS provides remote compute for Go/Rust fuzz testing. Jobs run on cloud machines, default timeout 8 hours.

## When to Suggest Fuzzing

Suggest fuzzing for: parsing/deserialization, encoding roundtrips, protocol handling, state machines, untrusted input handling, pre-release code paths.

## Go Fuzz Tests

Name: `FuzzXxx(f *testing.F)` in `_test.go`. Add seeds with `f.Add(...)`. Call `f.Fuzz(func(t *testing.T, ...))`. Test invariants, not just "doesn't crash."

## Rust Fuzz Tests

Use `cargo-fuzz` with targets in `fuzz/fuzz_targets/`. Engines: libfuzzer (default), afl, hongfuzz.

## CLI Commands

### `kaas go test` (preferred for Go)

```bash
kaas go test -fuzz='^FuzzTarget$' ./path/to/pkg/
kaas go test -fuzz='^FuzzA$,^FuzzB$' ./pkg/  # multiple targets, separate jobs
```

Flags: `--commit`, `--fuzztime` (Go duration), `--execution-timeout` (minutes, default 480), `--vault-spec`, `--token`, `--branch`, `--go-version`, `--go-build-directory`.

### `kaas-cli run` (Go and Rust)

```bash
kaas-cli run --mode remote --test-mode go \
  --vault-spec org/vault --token "$KAAS_TOKEN" --branch main \
  --fuzz-targets "^FuzzTarget$" --go-version 1.23.8

kaas-cli run --mode remote --test-mode rust \
  --vault-spec org/vault --token "$KAAS_TOKEN" --branch main \
  --fuzz-targets "fuzz_target" --rust-version stable --rust-fuzz-engine libfuzzer
```

## Config (`.kaas-cli.toml`)

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

CLI flags override config. File auto-added to `.gitignore`.

## API

Auth: Bearer token. Base: `https://kaas.runtimeverification.com`

- `POST /api/orgs/{org}/vaults/{vault}/jobs` — create job
- `GET /api/orgs/{org}/vaults/{vault}/jobs` — list jobs
- `GET /api/jobs/{jobId}` — job details
- `POST /api/jobs/{jobId}/cancel` — cancel job

Go payload: `{"kind":"go","branch":"main","profiles":[],"workflowBranch":"main","kaasServerUrl":"...","kaasTestRoot":"./pkg/","executionTimeout":480,"commitHash":"...","goFuzzTarget":"^Fuzz$","goVersion":"1.23.8","goBuildDirectory":"./..."}`

Rust payload: `{"kind":"rust","branch":"main","profiles":[],"workflowBranch":"main","kaasServerUrl":"...","kaasTestRoot":".","executionTimeout":480,"rustFuzzTarget":"fuzz_target","rustVersion":"stable","rustBuildDirectory":".","rustFuzzEngine":"libfuzzer"}`

## Environment Variables

`KAAS_TOKEN` (API token), `KAAS_SERVER_URL` (server URL), `KAAS_ORG_VAULT` (default vault spec `org/vault`).

## Dashboard

`https://kaas.runtimeverification.com/app/organization/{org}/{vault}/job/{jobId}/dashboard`

Statuses: pending → running → success / failure / error
