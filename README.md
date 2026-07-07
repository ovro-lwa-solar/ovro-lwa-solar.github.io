# OVRO-LWA Solar Observation Knowledge-base

Documentation site for OVRO-LWA solar radio observations, built with [MkDocs](https://www.mkdocs.org/) and [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/).

**Live site:** <https://ovro-lwa-solar.github.io/>

## Local development

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
mkdocs serve
```

Open <http://127.0.0.1:8000> to preview changes.

## Deployment

Pushes to `main` trigger the [GitHub Actions workflow](.github/workflows/deploy.yml), which builds and deploys the site.

**One-time setup (required):** open [Settings → Pages](https://github.com/ovro-lwa-solar/ovro-lwa-solar.github.io/settings/pages) and set **Build and deployment → Source** to **GitHub Actions**.

## Adding content

Add Markdown files under `docs/` and register them in the `nav` section of `mkdocs.yml`.
