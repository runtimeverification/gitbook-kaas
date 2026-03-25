# KAAS Fuzzing API Reference

Base URL: `https://kaas.runtimeverification.com`

All endpoints require a Bearer token in the `Authorization` header.

## Create a Fuzz Job

```
POST /api/orgs/{organizationName}/vaults/{vaultName}/jobs
```

### Go Fuzzing Payload

```json
{
  "kind": "go",
  "branch": "main",
  "profiles": [],
  "workflowBranch": "main",
  "kaasServerUrl": "https://kaas.runtimeverification.com/",
  "kaasTestRoot": "./path/to/pkg/",
  "executionTimeout": 480,
  "commitHash": "abc123def456...",
  "goFuzzTarget": "^FuzzMyFunction$",
  "goVersion": "1.23.8",
  "goBuildDirectory": "./..."
}
```

### Rust Fuzzing Payload

```json
{
  "kind": "rust",
  "branch": "main",
  "profiles": [],
  "workflowBranch": "main",
  "kaasServerUrl": "https://kaas.runtimeverification.com/",
  "kaasTestRoot": ".",
  "executionTimeout": 480,
  "rustFuzzTarget": "fuzz_transfer",
  "rustVersion": "stable",
  "rustBuildDirectory": ".",
  "rustFuzzEngine": "libfuzzer",
  "rustFuzzArgs": ""
}
```

### Response

```json
{
  "jobId": "unique-job-id",
  "status": "pending"
}
```

## List Vault Jobs

```
GET /api/orgs/{organizationName}/vaults/{vaultName}/jobs
```

Query parameters:
- `page` (int): Page number, default 1
- `per_page` (int): Results per page, default 20
- `status` (string): Filter by status (`pending`, `running`, `success`, `failure`, `cancelled`, `error`)
- `commit` (string): Filter by commit SHA
- `sort` (string): Sort field
- `direction` (string): Sort direction (`asc`, `desc`)

## Get Job Details

```
GET /api/jobs/{jobId}
```

Returns full job object including status, configuration, timing, and results.

## Cancel a Job

```
POST /api/jobs/{jobId}/cancel
```

Cancels a pending or running job.

## Get Job Reports

```
GET /api/jobs/{jobId}/json-report
POST /api/jobs/{jobId}/pdf-report
```

## Job Kind Values

| Kind | Description |
|------|-------------|
| `kontrol` | Kontrol formal verification |
| `foundry` | Forge test suite |
| `go` | Go fuzz testing |
| `rust` | Rust fuzz testing |

## Job Status Values

| Status | Description |
|--------|-------------|
| `pending` | Job created, waiting for runner |
| `running` | Job is executing |
| `success` | Job completed successfully |
| `failure` | Job completed with test failures |
| `cancelled` | Job was cancelled |
| `error` | Job encountered an infrastructure error |
| `processed` | Results have been processed |
| `processing_failed` | Post-processing of results failed |

## Dashboard URL Pattern

```
https://kaas.runtimeverification.com/app/organization/{org}/{vault}/job/{jobId}/dashboard
```
