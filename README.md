# GitHub Developer Day — Perth (companion site)

Companion website for **GitHub Developer Day Perth**, built with
[MkDocs](https://www.mkdocs.org/) and the
[Material for MkDocs](https://squidfunk.github.io/mkdocs-material/) theme.
It turns the day's agenda into readable, accessible pages — one per session —
with hero images, deeper write-ups than fit on a slide, and a resources page
to keep exploring afterwards.

The site is responsive, keyboard accessible, and supports a light/dark mode
toggle (via Material's built-in palette switcher).

## Local development

```bash
pip install -r requirements.txt
mkdocs serve
```

Then open <http://127.0.0.1:8000>.

## Project structure

```
mkdocs.yml            # site configuration, nav, theme
docs/
  index.md            # home page
  keynote.md           # Keynote — Agentic Coding
  github-app-cli.md     # Getting hands on with the GitHub App and CLI
  context-management.md # Context management and optimization
  sdk-foundry.md        # GitHub SDK and Microsoft Foundry
  resources.md           # Links: Awesome Copilot, Copilot SDK, Copilot App, Spec Kit, and more
  img/                   # hero images and placeholders
  stylesheets/extra.css   # site-specific styling on top of Material
assets/                   # original source slide exports
.github/workflows/deploy.yml  # builds and deploys to GitHub Pages on push to main
```

Pages without a source slide yet (the Keynote and Context Management
sessions) use `docs/img/placeholder-hero.svg` as a hero placeholder — swap in
the real slide image and update the `<img src>` in the corresponding page
once it's available.

## Deployment

Pushes to `main` build the site with `mkdocs build --strict` and publish it
to GitHub Pages via GitHub Actions (`.github/workflows/deploy.yml`). In the
repository settings, set **Pages → Build and deployment → Source** to
**GitHub Actions**.
