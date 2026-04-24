---
title: "Foundry (`forge test`) with kaas-cli"
description: Run `forge build` and `forge test` via `--test-mode forge` (local or Docker)
---

# Foundry (`forge test`) with `kaas-cli`

Use **`kaas-cli run --test-mode forge`** to run a standard Foundry workflow: **`forge build`**, then **`forge test`**. This is separate from **`--test-mode kontrol`** (symbolic proofs). The CLI test mode name is **`forge`**; some KaaS APIs and compute jobs refer to the same work as **Foundry** / **`foundry`**.

## What gets executed

| Step | Local mode (`--mode local`) | Container mode (`--mode container`) |
|------|-----------------------------|--------------------------------------|
| Build | `forge build` (+ `--extra-build-args`) | Same inside the container |
| Test | `forge test` (+ `--extra-test-args`) | `forge test --json` (+ `--extra-test-args`), streamed to **`foundry_test_report.json`** in the container, then copied back to the host |

**Remote mode:** **`kaas-cli run --mode remote --test-mode forge` is not implemented.** Remote jobs on KaaS today use this Foundry path through **managed compute / CI workers** that clone your repo and invoke the CLI with **`--mode container`** (see [KaaS in CI]({% link guides/kaas_setting-up-ci.md %})).

## Prerequisites

- A **Foundry project** (typically **`foundry.toml`** at the directory you run from, or under **`--test-root`**).
- **`--mode local`:** **`forge`** on your **`PATH`**.
- **`--mode container`:** **Docker** available to the CLI, with permission to pull and run images.

## Basic usage

From the repository root (or subdirectory) that contains your Foundry config:

```bash
kaas-cli run --mode local --test-mode forge
```

Run inside a **Foundry Docker image** (no local `forge` install required):

```bash
kaas-cli run --mode container --test-mode forge
```

If tests live under a subdirectory (monorepo layout):

```bash
kaas-cli run --mode container --test-mode forge --test-root packages/contracts
```

## Useful flags (`forge` mode)

| Flag | Purpose |
|------|---------|
| `--foundry-version` | Image tag when **not** using `--foundry-docker-image` (default image: `ghcr.io/foundry-rs/foundry:<tag>`; tag defaults to **`stable`** if unset). |
| `--foundry-docker-image` | Full image reference (overrides version-based default). |
| `--extra-build-args` / `-eb` | Extra arguments appended to **`forge build`**. |
| `--extra-test-args` / `-et` | Extra arguments appended to **`forge test`**. |
| `--verbose` / `-v` | More CLI logging. |

**Foundry profile:** set **`FOUNDRY_PROFILE`** in your environment before running. In **container** mode, the CLI passes **`FOUNDRY_PROFILE`** into the container (default **`default`** if unset).

**Examples:**

```bash
# Match a subset of tests (extra args are appended to forge test)
kaas-cli run --mode local --test-mode forge \
  --extra-test-args "--match-contract MyContractTest"

# Pin a Foundry image tag
kaas-cli run --mode container --test-mode forge --foundry-version nightly

# Custom image
kaas-cli run --mode container --test-mode forge \
  --foundry-docker-image ghcr.io/foundry-rs/foundry:latest
```

## Machine-readable report (container mode)

After a successful **container** run, the CLI tries to copy **`foundry_test_report.json`** (JSON test output from **`forge test --json`**) and **`forge_version.txt`** to the host and merges Forge version metadata into **`foundry_test_report.json`**. Use this for CI dashboards or uploads to KaaS job artifacts.

## Relationship to Kontrol

- **`--test-mode kontrol`:** **`kontrol build`** / **`kontrol prove`** (and optional **`kontrol.toml`**).
- **`--test-mode forge`:** plain Foundry only — **`forge build`** / **`forge test`**.

You can use **`forge`** mode to validate that contracts and tests pass in a **clean Foundry image** before investing time in Kontrol proofs.

## Further reading

- [`` `kaas-cli run` `` command]({% link guides/kaas-cli_run_command.md %}) — all modes and shared flags
- [Foundry Book — `forge test`](https://book.getfoundry.sh/reference/forge/forge-test)
