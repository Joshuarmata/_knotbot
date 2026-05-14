# KnotBot

Project site for **KnotBot**: an autonomous rope knot-tying system using a UR7e arm, Intel RealSense, and ROS2, built for **EE 106A** (Introduction to Robotics) at UC Berkeley, Spring 2026.

The site is built with [Hugo Blox](https://github.com/HugoBlox/kit) (developer portfolio template) and is configured for **GitHub Pages**.

## Live site

After you enable Pages (below), the site is served at:

**https://joshuarmata.github.io/_knotbot/**

If you rename the repository or use a custom domain, update `baseURL` in `config/_default/hugo.yaml` to match.

## Local development

Requirements: [Hugo Extended](https://gohugo.io/installation/) (see `hugoblox.yaml` for the pinned version), [Go](https://go.dev/dl/) (for Hugo modules), [Node.js](https://nodejs.org/) 22+, and [pnpm](https://pnpm.io/).

```bash
pnpm install
pnpm run dev
```

Then open the URL Hugo prints (usually `http://localhost:1313/`).

## Deploy to GitHub Pages

1. In the GitHub repo, go to **Settings → Pages**.
2. Under **Build and deployment**, set **Source** to **GitHub Actions** (not “Deploy from a branch”).
3. Push to **`main`**. The workflow [.github/workflows/deploy.yml](.github/workflows/deploy.yml) builds the site (including Hugo modules and Pagefind search) and publishes the `public/` output to Pages.

The build uses [actions/configure-pages](https://github.com/actions/configure-pages) so `hugo --baseURL` matches your real Pages URL (including project subpaths).

### Files that exist specifically for GitHub Pages

| Item | Purpose |
|------|--------|
| [`static/.nojekyll`](static/.nojekyll) | Tells GitHub Pages not to run Jekyll (avoids problems with underscored paths and static assets). |
| [`hugoblox.yaml`](hugoblox.yaml) `deploy.host: github-pages` | Tells Hugo Blox tooling the intended host. |
| Correct `baseURL` in [`config/_default/hugo.yaml`](config/_default/hugo.yaml) | Canonical URLs, RSS, and sitemap. CI still passes an explicit `--baseURL` at build time. |

## Optional: Netlify

[`netlify.toml`](netlify.toml) is kept if you want a Netlify mirror; this repo’s primary deployment path is GitHub Pages.

## License

See [LICENSE.md](LICENSE.md).
