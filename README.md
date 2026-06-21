# Personal Site

**Status: [Personal Site]**

Source repository for the Hugo-powered personal site for Md Imrul Hassan.

## Current Scope

- Hugo static site using the PaperMod theme.
- Profile-mode home page configured in `config.toml`.
- About page content under `content/about-me.md`.
- Custom domain configured through `CNAME`.

## Project Structure

```text
config.toml      # Hugo site configuration
content/         # Markdown content pages
assets/          # Profile and banner assets
static/          # Static files copied as-is
themes/          # Hugo theme submodule(s)
.github/         # GitHub Pages workflow/config, if present
```

## Quick Start

```bash
hugo server
```

## Verification

```bash
hugo --minify
```

Manual smoke check:

1. Run the Hugo development server.
2. Open the local site URL shown by Hugo.
3. Confirm the home page and `/about-me/` page render.

## Limitations

- The canonical domain should be kept consistent across `CNAME`, `config.toml`, DNS, and GitHub Pages settings.
- This README documents the repository purpose only; it does not change Hugo source behavior.
