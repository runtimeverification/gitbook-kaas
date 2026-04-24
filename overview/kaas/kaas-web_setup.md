---
title: Organizations, Vaults & GitHub App
description: KaaS web app setup, organizations, vaults, and GitHub App installation
---

# KaaS Web: Organizations, Vaults & GitHub App

This guide describes what the [KaaS web application](https://kaas.runtimeverification.com) does today, how **organizations** and **vaults** fit together, and the **recommended GitHub App** setup for private repositories and remote compute.

---

## What you can do in KaaS

| Area | What it is | Typical use |
|------|------------|-------------|
| **Vaults & caches** | A vault is a workspace tied to a project (often a GitHub repo). **Caches** store versioned proof artifacts (KCFG and related outputs) with tags. | Browse results in the UI, upload/download via [`kaas-cli`]({% link overview/kaas/kaas-cli_installation.md %}). |
| **Remote compute** | Jobs run on Runtime Verification infrastructure (e.g. Kontrol proofs, **Go/Rust fuzzing**). | Submit from the **Compute** tab or from the CLI (`kaas-cli run`, `kaas go test`). Requires a **vault spec** (`org/vault`) and **access token**. |
| **Local / container runs** | Proof workflows on your machine or in Docker. | [`kaas-cli run`]({% link guides/kaas-cli_run_command.md %}) in `local` or `container` mode; no GitHub App required for basic local use. |

**GitHub integration** is **required** for KaaS to clone **private** repositories when running **remote** jobs. It is **not** required only to use the CLI to **upload or download** cached artifacts to a vault you can already access.

---

## Step 1: Sign up and sign in

1. Open [KaaS](https://kaas.runtimeverification.com).
2. Choose **Login**.
3. **Sign in with GitHub (recommended)** — you are redirected to GitHub and back to the KaaS app. This is the smoothest path if you will use the GitHub App and remote compute.
4. **Email and password** — register, verify your email, then sign in.

*Sign-in options on the KaaS homepage: **Login**, then **Sign in with GitHub** or email/password as described above.*

---

## Step 2: Recommended setup — install the GitHub App

For **private** repos and **remote** Kontrol or fuzz jobs, install Runtime Verification’s GitHub App on the GitHub **organization or user account** that owns those repositories.

**Install the app:** [github.com/apps/runtime-verification-inc](https://github.com/apps/runtime-verification-inc)

During installation on GitHub you will:

1. Choose which **GitHub account or organization** the app is installed on.
2. Choose **all repositories** or only **selected** ones (you can change this later in GitHub’s app settings).

After installation, return to KaaS. Newly linked installations are picked up when you open the app; if an organization does not appear yet, use **Refresh Organization List** on the Organizations page (or close and reopen the **Create Organization** / onboarding flow and complete the GitHub steps again).

**Onboarding in the app (summary):**

1. From **Organizations**, choose **Get Started** or **Create Organization**.
2. Under **GitHub Integration Options**, choose **Install GitHub App** (connect GitHub).
3. Use **Install GitHub App** to open GitHub, complete installation, then **Continue** in the modal.
4. When GitHub organizations are linked to your user, they appear as **GitHub organizations** in the list (see below).

> **Info.** Log into the **same GitHub account** that has access to the repositories you need. The onboarding copy in the app reminds you of this before you leave for GitHub.

---

## Step 3: Organizations

### Types of organizations

| Type | How it appears | Name pattern |
|------|----------------|--------------|
| **GitHub-linked** | Label: *GitHub Organization* | Matches the GitHub **login** (user or org), e.g. `mycompany`. **Does not** start with `@`. |
| **User-managed** | Label: *User-managed Organization* | **Must start with `@`**, e.g. `@my-team`. Use this if you want an organization **without** tying it to a GitHub org first. |

### Managing the organization list

Open **Organizations** in the app (`/app`). You can:

- **Search** organizations by name.
- **Filter** by *All*, *GitHub Organizations*, or *User-managed Organizations*.
- Switch **grid** or **list** view.
- **Refresh** to reload organizations and GitHub installation data.

To add another **user-managed** organization: **Create Organization** → choose **Without GitHub Integration** → enter a name that **starts with `@`** (letters, digits, `_`, `-`, `.`).

If you already installed the GitHub App but a GitHub org is missing, confirm the app is installed for that org on GitHub, grant access to the right repositories, then **Refresh Organization List** in KaaS.

*Organizations list empty state: prompts you to connect GitHub or create an organization.*

#### Tutorial video (GitHub-connected organization)

[Open video on Screencast](https://app.screencast.com/JpRDbeTHhgRqs/e)

<iframe scrolling="no" frameborder="0" style="width:100%;max-width:944px;height:717px;border:0;" src="https://app.screencast.com/JpRDbeTHhgRqs/e" allowfullscreen title="GitHub organization tutorial"></iframe>

---

## Step 4: Vaults

A **vault** belongs to exactly one organization. The CLI and API refer to it as **`organization-name/vault-name`** (vault spec).

### Opening the Vaults tab

1. Click an **organization**.
2. Open the **Vaults** tab.

*Inside an organization, open the **Vaults** tab to see vaults for that org.*

### Creating a vault (current UI)

Use the control to **create or add a vault** (for example **Add vault** / **Create a New Vault**). A modal offers:

1. **Connect GitHub Repository** (shown for **GitHub-linked** organizations)  
   - Uses your **GitHub App** installation.  
   - Pick from repositories the app can access (**private** and **public**).  
   - **Best for** your own orgs/repos where the app is installed.

2. **Connect Public Repository** (always available)  
   - Enter a **public** GitHub repository URL.  
   - **Best for** public repos that are **not** in your app installation.

For **user-managed** organizations (`@my-org`), the modal only offers **Connect Public Repository** — private repos need a **GitHub-linked** organization with the GitHub App installed.

### Vault naming rules (important)

| Organization type | Vault name rule |
|-------------------|-----------------|
| **GitHub-linked** | Vault name **must start with `@`**, followed by allowed characters (letters, digits, `_`, `-`, `.`). |
| **User-managed** (`@org`) | Vault name **must not** start with `@`; use letters, digits, `_`, `-`, `.` only. |

These rules match the forms in the KaaS web app. Use the exact **`org/vault`** pair shown in the UI when you pass `--vault-spec` to the CLI.

### If a repository does not appear

- Confirm the **GitHub App** is installed on the correct GitHub org/user and that the repo is included (all or selected).
- Read any **banner** at the top of the page for next steps.

*If a repository is missing from the picker, check the GitHub App installation and repository access; the UI may show a banner with next steps.*

#### Tutorial video (vault + compute)

[Open video on Screencast](https://app.screencast.com/w6Bh2tD4hvu4E/e)

<iframe scrolling="no" frameborder="0" style="width:100%;max-width:944px;height:717px;border:0;" src="https://app.screencast.com/w6Bh2tD4hvu4E/e" allowfullscreen title="Vault and compute tutorial"></iframe>

---

## Step 5: Inside an organization

Organization pages use tabs (some depend on your **plan** or **role**):

| Tab | Purpose |
|-----|---------|
| **Overview** | Summary of the organization. |
| **Vaults** | List vaults; open a vault for caches and settings. |
| **Users** | Members and access. |
| **Compute** | Remote jobs for this org (when available for your account). |
| **Notifications** | Notification settings (eligible accounts). |
| **Credits / Subscription / Usage** | Billing and usage (typically **admin** views). |

---

## Step 6: Inside a vault

| Tab | Purpose |
|-----|---------|
| **Caches** | Versioned cached proof runs; open a cache for reports and KCFG views. |
| **Compute** | Jobs tied to this vault (when your account has access). |
| **Collaborators** | Who can access this vault. |

---

## Step 7: Access tokens (CLI and API)

1. Open your **Profile** (avatar menu).
2. Go to **Access Tokens** (or [Profile → keys](https://kaas.runtimeverification.com/app/profile#tokens)).
3. Create a token and store it securely (e.g. `KAAS_TOKEN`).

**Admin keys** (where applicable) are managed under the separate **Admin** / admin-token section of the profile ([`#admin_tokens`](https://kaas.runtimeverification.com/app/profile#admin_tokens)).

For CLI usage patterns, see [Connecting using tokens]({% link guides/kaas-cli_connecting-using-tokens.md %}) and [Device flow]({% link guides/kaas-cli_connecting-using-device-flow.md %}).

---

## Related documentation

- [KaaS CLI installation]({% link overview/kaas/kaas-cli_installation.md %})
- [`kaas-cli run` command]({% link guides/kaas-cli_run_command.md %})
- [Remote fuzzing]({% link guides/kaas-cli_remote_fuzzing.md %})
- [CI / GitHub Actions]({% link guides/kaas_setting-up-ci.md %})
- [KCFG tagging]({% link guides/kaas-cli_tagging-best-practices.md %})

---

## Questions?

[contact@runtimeverification.com](mailto:contact@runtimeverification.com)  
[Discord](https://discord.gg/CurfmXNtbN)  
[Telegram](https://t.me/rv_kontrol)
