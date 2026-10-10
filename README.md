# PureSkill.gg GitHub Pages Site

> [!IMPORTANT]
> **This repo is retired.** PureSkill.gg's developer documentation now lives in one place: the [docs](https://github.com/pureskillgg/docs) repo, published at [docs.pureskill.gg](https://docs.pureskill.gg). Make documentation changes there.
>
> This repo is kept only so `dev.pureskill.gg` keeps serving its one page, which points visitors to docs.pureskill.gg. It may be archived; GitHub Pages keeps serving an archived repo's site.

[![GitHub Pages](https://img.shields.io/badge/GitHub%20Pages-dev.pureskill.gg-53ac67)](https://dev.pureskill.gg)

A one-page static GitHub Pages site for the `dev.pureskill.gg` custom domain. It points visitors to the PureSkill.gg docs at `https://docs.pureskill.gg`.

## What it does

This repo is a static GitHub Pages site with **no build system, no dependencies, and no backend code**. Every address on `dev.pureskill.gg` shows the same page:

- **`index.html`**: a card with the PureSkill.gg logo, the heading "Looking for the docs?", one line saying the developer docs (including the CSDS data format) live at docs.pureskill.gg, and a "Go to the docs" button. It does not redirect on its own; the visitor clicks through.
- **`404.html`**: an exact copy of `index.html`. GitHub Pages serves it for any path that doesn't exist, so `dev.pureskill.gg/anything` shows the same card. Edit both files together.

The page carries everything it needs: the logo, the Archive title font and the favicon are files in this repo, and the styles are inline. Assets use root-relative paths (`/logo.svg`) so they also load from the 404 page at any depth. The only outside request is the Assistant body font from Google Fonts, which falls back to the system font.

Configuration:

- **`CNAME`**: contains `dev.pureskill.gg`, telling GitHub Pages to serve this repo at that custom domain.
- **`.nojekyll`**: an empty marker file that disables GitHub's default Jekyll build so the raw files are served verbatim.
- **`LICENSE.txt`**: a one-line copyright notice for FPS Critic, Inc. (2019–2022), the company behind PureSkill.gg.

## Pipeline role

This repo is **not part of the CS2 data or coaching pipeline**; it produces and consumes no match, replay, csds, or assessment data. It is the org's GitHub Pages repo, whose name (`<org>.github.io`) is what makes GitHub serve it, and the `CNAME` binds it to the custom domain `dev.pureskill.gg`.

## Files

| File           | Role                                                                    |
| -------------- | ----------------------------------------------------------------------- |
| `index.html`   | The page: logo, one line and a button to `https://docs.pureskill.gg`.   |
| `404.html`     | Exact copy of `index.html`, served for every other path.                |
| `logo.svg`     | The PureSkill.gg wordmark, from csgo-webapp's `src/assets/logo.svg`.    |
| `Archive.woff` | The title and button font the webapp uses.                              |
| `favicon.ico`  | The webapp's favicon, from csgo-webapp's `public/favicon.ico`.          |
| `CNAME`        | Binds the GitHub Pages site to the custom domain `dev.pureskill.gg`.    |
| `.nojekyll`    | Empty marker that disables Jekyll so files are served verbatim.         |
| `LICENSE.txt`  | Copyright notice for FPS Critic, Inc.                                   |

## Local preview

There is nothing to build. Serve the repo root with any static file server, for example:

```
$ python3 -m http.server 8080
```

…then open <http://localhost:8080/>. (A plain static server shows its own 404 for other paths; open <http://localhost:8080/404.html> to see the not-found copy.)

## Deployment

Deployment is handled entirely by **GitHub Pages**; there is no CI workflow in this repo.

- GitHub serves this repo because of its name, `pureskillgg.github.io`.
- The site is published from the `master` branch root; merging to `master` publishes the change.
- `.nojekyll` disables Jekyll, so the raw files are served exactly as committed.
- `CNAME` maps the published site to the custom domain **`dev.pureskill.gg`**, and GitHub Pages provisions and renews the TLS certificate.

## Logs / Observability

This repo owns no AWS resources. Hosting, TLS, and availability are GitHub Pages' responsibility, so deployment status surfaces through GitHub:

- **Build / deploy status**: the repo's **Deployments** view and the **github-pages** environment on GitHub show each Pages publish and whether it succeeded.
- **Custom domain / DNS / TLS**: **Settings → Pages** shows the `dev.pureskill.gg` custom-domain state and certificate provisioning.

## Contributing

Edits are just edits to the HTML files, the assets or the configuration. Keep `404.html` identical to `index.html`. Open a pull request; merging to `master` deploys to `dev.pureskill.gg` via GitHub Pages.

## License

Copyright (c) 2019-2022 FPS Critic, Inc.

## Warranty

This software is provided by the copyright holders and contributors "as is" and any express or implied warranties, including, but not limited to, the implied warranties of merchantability and fitness for a particular purpose are disclaimed.
