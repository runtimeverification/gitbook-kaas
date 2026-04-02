---
title: KaaS
description: K as a Service — documentation home
permalink: /
---

### Get started

# KaaS

Introducing **KaaS** (**K** as a Service), a cloud-based solution designed to enhance the [**K** framework](https://kframework.org/) experience. **KaaS** is engineered to introduce new features, streamline operations, and foster collaboration among teams. By leveraging caching proofs and remote computation, it eliminates redundant processes, saving your team precious time.

**KaaS** integrates seamlessly with continuous integration (CI) systems, allowing developers to pull the latest cached results and bypass repetitive computations. It is a strong fit for internal teams that want centralized, shared computational results.

For data management, **KaaS** uses secure storage. User access is protected through project keys and organization membership. The **KaaS CLI** supports cache management and local, containerized, or remote proof execution.

Cloud compute lets you offload heavy work to Runtime Verification infrastructure (including remote Kontrol and fuzzing workflows).

With **KaaS**, the goal is to make the **K** framework more approachable and to streamline verification workflows for teams.

## Next steps

- [Organizations, Vaults & GitHub App]({% link overview/kaas/kaas-web_setup.md %}) — sign in, install the [GitHub App](https://github.com/apps/runtime-verification-inc), manage orgs and vaults, tokens
- [KaaS CLI installation]({% link overview/kaas/kaas-cli_installation.md %})

## Publish

This site is built with **Jekyll** and deployed with **GitHub Actions** to **GitHub Pages**. After you enable Pages (source: GitHub Actions) and add a [custom domain](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site) if needed, the workflow on `main` will publish automatically.
