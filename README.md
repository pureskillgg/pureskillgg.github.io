# PureSkill.gg GitHub Pages Site

[![GitHub Pages](https://img.shields.io/badge/GitHub%20Pages-dev.pureskill.gg-53ac67)](https://dev.pureskill.gg)

A minimal static GitHub Pages site for the `dev.pureskill.gg` custom domain — a hand-written redirect landing page, a custom 404 page, and the configuration that binds them to the domain.

## What it does

This repo is a static GitHub Pages site with **no build system, no dependencies, and no backend code**. It contains exactly two content pages plus configuration:

- **`index.html`** — The landing page. It renders the PureSkill.gg logo and immediately redirects every visitor to the PureSkill.gg GitHub organization via a meta-refresh tag:

  ```html
  <meta http-equiv="Refresh" content="0; url='https://github.com/pureskillgg'" />
  ```

  Its `<body>` uses the class `cdn-error`, and all styling and icons are loaded from the production webapp CDN at `csgo.cdn.pureskill.app` (logo SVG, favicons, and the main CSS chunk).

- **`404.html`** — The custom GitHub Pages not-found page. It shows the same branding with the CS-themed message **"Ran through smoke! We couldn't find this page."** and a link back to the docs site, `https://docs.pureskill.gg`. It is byte-for-byte identical to `index.html` except it carries the error copy instead of the meta-refresh redirect.

Configuration completes the setup:

- **`CNAME`** — Contains `dev.pureskill.gg`, telling GitHub Pages to serve this repo at that custom domain.
- **`.nojekyll`** — An empty marker file that disables GitHub's default Jekyll build so the raw files are served verbatim.
- **`LICENSE.txt`** — A one-line copyright notice for FPS Critic, Inc. (2019–2022), the company behind PureSkill.gg.

There are no Lambdas, handlers, Step Functions, queues, Serverless/Terraform configs, package manifests, GitHub Actions workflows, or source directories. The entire repository's behavior is: **serve two static pages and redirect.**

## Pipeline role

This repo is **not part of the CS2 data or coaching pipeline** — it produces and consumes no match, replay, csds, or assessment data. It is the org's GitHub Pages repo whose name (`<org>.github.io`) is what makes GitHub serve it, and the `CNAME` binds it to the custom domain `dev.pureskill.gg`.

It is purely web-presence / infrastructure glue. It **borrows** branding assets (logo, CSS, favicons) from the production webapp CDN at `csgo.cdn.pureskill.app` — it does not own or provision that CDN — and points visitors at:

- the GitHub org, `https://github.com/pureskillgg` (from `index.html`), and
- the docs site, `https://docs.pureskill.gg` (from `404.html`).

If the CDN is unreachable, the pages degrade gracefully to the `cdn-error` body styling.

## Files

Only the files actually tracked in this repo are listed.

| File          | Role                                                                                                          |
| ------------- | ----------------------------------------------------------------------------------------------------------- |
| `index.html`  | Landing page: shows the logo and meta-refresh-redirects to `https://github.com/pureskillgg`. Body class `cdn-error`. |
| `404.html`    | Custom GitHub Pages not-found page ("Ran through smoke!") linking to `https://docs.pureskill.gg`.            |
| `CNAME`       | Binds the GitHub Pages site to the custom domain `dev.pureskill.gg`.                                          |
| `.nojekyll`   | Empty marker that disables Jekyll so files are served verbatim.                                              |
| `LICENSE.txt` | Copyright notice for FPS Critic, Inc.                                                                         |

## Local preview

There is nothing to build. To preview the pages locally, serve the repo root with any static file server, for example:

```
$ python3 -m http.server 8080
```

…then open <http://localhost:8080/index.html> or <http://localhost:8080/404.html>. The logo, favicons, and CSS load over the network from `csgo.cdn.pureskill.app`.

## Deployment

Deployment is handled entirely by **GitHub Pages** — there is no CI workflow in this repo.

- GitHub serves this repo because of its name, `pureskillgg.github.io`.
- The site is published from the `master` branch root; pushing to `master` publishes the change.
- `.nojekyll` disables Jekyll, so the raw HTML is served exactly as committed.
- `CNAME` maps the published site to the custom domain **`dev.pureskill.gg`**, and GitHub Pages provisions and renews the TLS certificate.

## Logs / Observability

This repo owns no AWS resources — there are no CloudWatch log groups, DLQs, Step Functions, Sentry integration, or `LOG_LEVEL` config. Hosting, TLS, and availability are GitHub Pages' responsibility, so deployment status surfaces through GitHub rather than AWS:

- **Build / deploy status** — the repo's **Deployments** view and the **github-pages** environment on GitHub show each Pages publish and whether it succeeded.
- **Custom domain / DNS / TLS** — **Settings → Pages** shows the `dev.pureskill.gg` custom-domain state and certificate provisioning.
- **Runtime "errors"** — the only failure mode in the pages themselves is the production CDN (`csgo.cdn.pureskill.app`) being unreachable, in which case the pages fall back to the `cdn-error` body styling. This repo does not provision or monitor that CDN.

## Contributing

Edits are just edits to the two HTML files or the configuration. Open a pull request; merging to `master` deploys to `dev.pureskill.gg` via GitHub Pages. Keep the assets pointed at the production CDN host and preserve the `cdn-error` body class so the fallback styling continues to work.

## License

Copyright (c) 2019-2022 FPS Critic, Inc.

## Warranty

This software is provided by the copyright holders and contributors "as is" and any express or implied warranties, including, but not limited to, the implied warranties of merchantability and fitness for a particular purpose are disclaimed.
