# KaaS documentation (source)

User-facing documentation for [KaaS](https://kaas.runtimeverification.com), published as a static site with **Jekyll** and **GitHub Pages**.

## Local preview

```bash
bundle install
bundle exec jekyll serve
```

Open <http://localhost:4000>. If you use a non-empty `baseurl` (project Pages URL), run:

```bash
bundle exec jekyll serve --baseurl /your-repo-name
```

## GitHub Pages

1. Repository **Settings** → **Pages** → **Build and deployment**: source **GitHub Actions**.
2. Push to `main`; the workflow [`.github/workflows/jekyll-gh-pages.yml`](.github/workflows/jekyll-gh-pages.yml) builds and deploys the `jekyll build` output.
3. Optional: set a **custom domain** (e.g. `docs.kaas.runtimeverification.com`) in Pages settings and add the DNS records GitHub shows.

`jekyll build --baseurl` is supplied automatically in CI so the site works both at `https://<org>.github.io/<repo>/` and behind a custom domain (with `baseurl` empty in `_config.yml` once the domain is configured—GitHub still passes the correct base path for subdirectory installs).

## Content

- Markdown pages live under [`guides/`](guides/) and [`overview/kaas/`](overview/kaas/).
- Navigation is [`_data/nav.yml`](_data/nav.yml).
- `AGENTS.md` / `CLAUDE.md` are for AI assistants and are **not** published on the site (see `_config.yml` `exclude`).

### Images

Screenshots previously referenced as `/.gitbook/assets/...` should be placed under `assets/images/` (same filenames). Add files there to fix broken images until they are migrated.
