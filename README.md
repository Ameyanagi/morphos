# Morphous

<!-- yarkr:badges:start -->
[![Indexed on Yarkr](https://api.yarkr.com/functions/v1/api/api/v1/badges/repos/Ameyanagi/morphos/version.svg)](https://yarkr.com/arks/ameyanagi/morphos)
<!-- yarkr:badges:end -->

Morphous is a static TanStack Start catalog for nature-inspired design systems. Each system starts from an animal, plant, mineral, weather, or landscape motif and ships a transparent motif asset, light and dark design-system boards, recorded prompts, and shadcn/tweakcn-compatible theme exports.

The published site is configured for:

```text
morphos.ameyanagi.com
```

## Stack

- Bun for package management and scripts
- TanStack Start, Vite, React, and Tailwind CSS
- shadcn-style component primitives and CSS variable theme exports
- Static prerender output from `.output/public`
- GitHub Actions deployment to GitHub Pages

## Commands

```bash
bun install
bun run dev
bun run check
bun run test
bun run ci
bun run build
bun run generate:previews
bun run pages:prepare
```

`bun run check` runs lint and typecheck. `bun run ci` runs lint, typecheck, tests, and the production build. `bun run generate:previews` creates lightweight WebP preview derivatives for gallery rendering. `bun run pages:prepare` validates the static build output and adds the GitHub Pages files needed for deployment.

## Development

Start the local dev server:

```bash
bun run dev
```

The app uses the generated catalog data in `src/data/systems.json` and static bundle assets under `public/systems`.

## Catalog Bundles

Each system bundle lives at:

```text
public/systems/{slug}/
```

Core bundle files:

```text
prompts.json
system.json
theme.css
theme.json
```

Most current systems also include `motif.png`, `design-system-light.png`, and `design-system-dark.png`. Older bundles may use equivalent asset names recorded in `system.json`, such as `animal.png` or `design-system.png`. Some systems also keep source or generated-image records in nested folders. The catalog UI reads the public bundle files and exposes prompts and theme exports from the individual system directory.

## Generation

The project-specific Codex skill lives at:

```text
.codex/skills/morphous-catalog/SKILL.md
```

Use it when creating or refreshing motif assets, design-system boards, prompt records, and theme exports. Image generation is intentionally handled by the Codex image tool, then copied into each system bundle.

After generated assets or system metadata changes, refresh and validate the catalog:

```bash
bun run generate:catalog
bun run generate:previews
bun run validate:catalog
```

## Pre-Commit Hook

Husky installs a local pre-commit hook through `bun install` via the `prepare` script. The hook runs:

```bash
bun run precommit
```

That runs `bun run generate:previews` first, then `bun run check`, so commits are gated by generated previews, lint, and typecheck without running the full production build locally. Current previews are skipped by timestamp; if generation updates preview files, stage those generated files and retry the commit.

## GitHub Pages

GitHub Pages is deployed by [`.github/workflows/pages.yml`](.github/workflows/pages.yml). The workflow runs on pushes to `main` and can also be started manually from GitHub Actions.

Deployment flow:

1. Install dependencies with Bun.
2. Run `bun run ci`.
3. Upload `.output/public` as the Pages artifact.
4. Deploy with `actions/deploy-pages`.

The custom domain is committed in `public/CNAME`, and `.nojekyll` is included so GitHub Pages serves generated assets exactly as built.

Pull requests to `main` run [`.github/workflows/ci.yml`](.github/workflows/ci.yml), which executes the same CI script without deploying.

## Publishing Checklist

Before pushing a catalog update:

```bash
bun run validate:catalog
bun run generate:previews
bun run ci
bun run pages:prepare
```

`pages:prepare` creates a static `404.html` fallback from `index.html` so deep links work when served by GitHub Pages.

<!-- yarkr:install:start -->
## Install with Yarkr

This repo is indexed on [Yarkr](https://yarkr.com/arks/ameyanagi/morphos) as the ark [ameyanagi/morphos](https://yarkr.com/arks/ameyanagi/morphos), so its skills install into Claude Code, Codex, Cursor and other agent harnesses with one command.

### CLI

Install the whole ark, which Yarkr keeps equal to this repo:

```bash
npx yarkr-cli sync ameyanagi/morphos
```

Or add individual skills:

```bash
npx yarkr-cli add skill:ameyanagi/morphos/morphous-catalog
```

No Node.js? `curl -fsSL https://yarkr.com/install.sh | sh` (macOS, Linux) or `irm https://yarkr.com/install.ps1 | iex` (Windows PowerShell) installs `yarkr` as a standalone binary; then type `yarkr` in place of `npx yarkr-cli`.

### MCP

Connect your agent to the ark over MCP:

```bash
npx yarkr-cli mcp add ameyanagi/morphos
```

Or point any MCP client at `https://api.yarkr.com/functions/v1/api/mcp/o/ameyanagi/morphos`.
<!-- yarkr:install:end -->

## License

Licensed under either of:

- Apache License, Version 2.0 ([LICENSE-APACHE](LICENSE-APACHE) or http://www.apache.org/licenses/LICENSE-2.0)
- MIT license ([LICENSE-MIT](LICENSE-MIT) or http://opensource.org/licenses/MIT)

at your option.

Unless you explicitly state otherwise, any contribution intentionally submitted for inclusion in the work by you, as defined in the Apache-2.0 license, shall be dual licensed as above, without any additional terms or conditions.
