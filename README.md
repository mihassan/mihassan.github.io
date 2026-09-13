# mihassan.com

Source for **Md Imrul Hassan’s** personal portfolio: a small Hugo site for selected software, technical notes, wireless-network research, Bangla poetry, music and photography.

The site is static, has no analytics or external runtime dependencies, and keeps substantive content available when JavaScript is disabled. `Md` is part of the public name—not an honorific—and remains explicit in all textual branding. The non-letter **Confluence** symbol is the favicon and supporting identity mark.

## Requirements

- Hugo **0.165.0** (extended or standard; the tested local binary is extended)
- Python 3.11+ for the dependency-free static checks
- Node 22+ and a Chromium/Chrome executable for browser smoke tests

The pinned Hugo value is also recorded in `.hugo-version`.

## Run locally

```sh
hugo server --bind 127.0.0.1 --port 1313 --disableFastRender
```

Open <http://127.0.0.1:1313/>. The explicit loopback bind avoids exposing the development server to the local network. Stop it with `Ctrl-C`.

## Build

```sh
hugo --gc --minify --cleanDestinationDir
```

Hugo writes generated output to `public/`. Do not edit or commit that directory; change `content/`, `data/`, `layouts/`, or `static/` instead.

## Verify

Static build, routes, links, fragments, metadata, name contract, poem checksums, SVG/PNG/ICO files and payload:

```sh
./scripts/check-site.py
```

Responsive browser scenarios, dark mode, reduced motion, keyboard focus, mobile navigation, no JavaScript, blocked JavaScript, hover contrast and designed 404:

```sh
CHROME_PATH=/path/to/chrome-or-chromium node scripts/browser-smoke.mjs
```

On the machine used for the implementation, `CHROME_PATH` defaults to the locally cached Chromium executable. The browser check starts temporary loopback-only Hugo/Chromium processes, writes evidence to `/tmp/homepage-final-qa`, and stops both processes when finished. Override `QA_DIR`, `HUGO_TEST_PORT`, or `CHROME_DEBUG_PORT` if needed.

Run both before publishing:

```sh
./scripts/verify.sh
```

The checks do not validate external destinations, production DNS/TLS, social-platform cache behaviour, Safari, or a full screen-reader/WCAG audit. Those require post-deployment or manual verification; they are not implied by a passing local build.

## Content maintenance

- Homepage headings and calls to action: `data/home.yaml`
- Listed projects and preserved legacy project routes: `content/work/*.md`
- Technical notes: `content/notes/*.md`
- Publications: `data/publications.yaml`
- Confirmed public profiles: `data/profiles.yaml`
- Claim/source maintenance notes: `docs/content-sources.md`
- Identity proof: `docs/identity-proof.html`
- Poem body checksums: `docs/poem-checksums.json`

Both Bangla poem bodies are intentionally preserved. Presentation and front matter may change, but run the checks before modifying their body text.

## License

Software source code is available under the MIT License. Original articles, project copy, poems and portfolio artwork are © Md Imrul Hassan, all rights reserved unless a file explicitly states otherwise. Third-party material remains under its original terms. See `LICENSE` for the complete scope.

## Staged publication

The existing public repository is `mihassan/mihassan.github.io`, whose `main` branch serves the earlier site. Publication of this redesign is deliberately staged:

1. Preserve the existing `main` branch and its live GitHub Pages site.
2. Copy this verified source into a separate `portfolio-redesign` branch.
3. Review the branch and an isolated Cloudflare Pages preview.
4. Merge or change the custom-domain hosting only after a separate final approval.

Review staged files before every commit. Never add private research archives, `.env` files, alternate contact details, downloaded audio, or generated `public/` output.

## Cloudflare Pages (Git integration)

For a future production deployment after the staging branch and preview are approved:

1. In Cloudflare Workers & Pages, choose **Create application → Pages → Import an existing Git repository** and select the GitHub repository.
2. Choose `main` as the production branch (other branches become preview deployments).
3. Use these build settings:
   - Framework preset: Hugo, or none with the values below
   - Root directory: `/` (repository root)
   - Build command:
     ```sh
     if [ "${CF_PAGES_BRANCH:-}" = "main" ]; then BASE_URL="https://mihassan.com/"; else BASE_URL="${CF_PAGES_URL:?}"; fi; hugo --gc --minify -b "$BASE_URL"
     ```
   - Build output directory: `public`
4. Set `HUGO_VERSION` to `0.165.0` for both **Production** and **Preview** environments.
5. Save and deploy only after reviewing the preview. Custom-domain/DNS/TLS changes are separate operations.

Preview branches use their generated `CF_PAGES_URL`, so canonical and absolute URLs remain inside the isolated preview. Production uses `https://mihassan.com/`, matching `hugo.toml` and avoiding a `pages.dev` canonical after a future custom-domain cutover. Official references:

- <https://developers.cloudflare.com/pages/framework-guides/deploy-a-hugo-site/>
- <https://developers.cloudflare.com/pages/get-started/git-integration/>
- <https://developers.cloudflare.com/pages/configuration/build-configuration/>
- <https://gohugo.io/commands/hugo/>

The staging plan does not itself authorise a `main` merge, custom-domain attachment, DNS change or production cutover. Those remain separate actions.
