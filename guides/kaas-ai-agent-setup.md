# AI Agent Setup for KAAS Fuzzing

KAAS ships a knowledge file that teaches AI coding assistants how to write fuzz tests and submit them to KAAS. This guide explains how to install it for your preferred tool.

The knowledge file covers:
- When and why to write fuzz tests
- Go and Rust fuzz test patterns
- Using the `kaas-cli` and `kaas go test` commands
- KAAS API endpoints for programmatic access
- `.kaas-cli.toml` configuration

## Option 1: AGENTS.md (Universal — All Tools)

`AGENTS.md` is an open standard recognized by Cursor, Claude Code, GitHub Copilot, Codex, and most AI coding agents. Place it in your project root and it will be loaded automatically.

**Install:**

Copy the KAAS fuzzing `AGENTS.md` into the root of any project where you want AI-assisted fuzzing:

```bash
curl -o AGENTS.md https://raw.githubusercontent.com/runtimeverification/gitbook-kaas/main/AGENTS.md
```

Or copy it manually from the [gitbook-kaas repository](https://github.com/runtimeverification/gitbook-kaas/blob/main/AGENTS.md).

The file is picked up automatically — no additional configuration needed.

## Option 2: Cursor Skills

Cursor supports project-level and personal skills via `.cursor/skills/` directories.

**Project-level (shared with your team via git):**

```bash
mkdir -p .cursor/skills/kaas-fuzzing
curl -o .cursor/skills/kaas-fuzzing/SKILL.md \
  https://raw.githubusercontent.com/runtimeverification/gitbook-kaas/main/.cursor/skills/kaas-fuzzing/SKILL.md
curl -o .cursor/skills/kaas-fuzzing/api-reference.md \
  https://raw.githubusercontent.com/runtimeverification/gitbook-kaas/main/.cursor/skills/kaas-fuzzing/api-reference.md
```

**Personal (available across all your projects):**

```bash
mkdir -p ~/.cursor/skills/kaas-fuzzing
curl -o ~/.cursor/skills/kaas-fuzzing/SKILL.md \
  https://raw.githubusercontent.com/runtimeverification/gitbook-kaas/main/.cursor/skills/kaas-fuzzing/SKILL.md
curl -o ~/.cursor/skills/kaas-fuzzing/api-reference.md \
  https://raw.githubusercontent.com/runtimeverification/gitbook-kaas/main/.cursor/skills/kaas-fuzzing/api-reference.md
```

{% hint style="warning" %}
Do **not** place skills in `~/.cursor/skills-cursor/` — that directory is reserved for Cursor's internal built-in skills.
{% endhint %}

## Option 3: Claude Code (CLAUDE.md)

Claude Code loads `CLAUDE.md` files automatically from your project root or `~/.claude/CLAUDE.md` for global preferences.

**Project-level:**

```bash
curl -o CLAUDE.md https://raw.githubusercontent.com/runtimeverification/gitbook-kaas/main/CLAUDE.md
```

**Global (personal, all projects):**

```bash
curl -o ~/.claude/CLAUDE.md https://raw.githubusercontent.com/runtimeverification/gitbook-kaas/main/CLAUDE.md
```

If you already have a `CLAUDE.md`, append the KAAS content or use `@import` to reference it.

## Option 4: ChatGPT (Custom GPT / Projects)

ChatGPT supports knowledge files through Custom GPTs and Projects. Since there's no filesystem integration, you upload the knowledge file manually.

**Using ChatGPT Projects:**

1. Open [ChatGPT](https://chatgpt.com) and go to **Projects**
2. Create or open a project
3. Click **Add files** and upload the `SKILL.md` file from [gitbook-kaas](https://github.com/runtimeverification/gitbook-kaas/blob/main/.cursor/skills/kaas-fuzzing/SKILL.md)
4. Optionally upload `api-reference.md` for the full API details
5. Add a custom instruction: *"Refer to the KAAS fuzzing knowledge file when helping with fuzz testing or KAAS CLI usage."*

**Using a Custom GPT:**

1. Go to **Explore GPTs** → **Create**
2. In the **Knowledge** section, upload `SKILL.md` and `api-reference.md`
3. In **Instructions**, add: *"You are a KAAS fuzzing assistant. Use the uploaded knowledge files to help users write fuzz tests and run them on KAAS infrastructure."*

{% hint style="info" %}
Custom GPTs require a ChatGPT Plus, Team, or Enterprise subscription.
{% endhint %}

## Verifying Installation

After installation, test that your AI agent has the knowledge by asking:

> "How do I run a Go fuzz test on KAAS?"

It should know about `kaas go test`, the `--fuzz` flag, vault specs, `.kaas-cli.toml` configuration, and the KAAS API.
