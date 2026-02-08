# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What

Svelte is an Obsidian theme. This repository contains the theme files and automated release workflow for distribution through the Obsidian community themes gallery.

## Project Structure

- `src/scss/` - SCSS source files (development)
  - `index.scss` - Main entry point for SCSS compilation
- `theme.css` - Compiled CSS output (generated from SCSS)
- `manifest.json` - Theme metadata (name, version, author)
- `versions.json` - Maps theme versions to minimum Obsidian versions
- `version-bump.mjs` - Automated version bumping script
- `.github/workflows/release-version.yml` - GitHub Actions workflow for automated releases

## SCSS Development

The theme is built using SCSS for better organization and maintainability. All development happens in `src/scss/`, which compiles to `theme.css`.

### Compilation Commands

- `npm run dev` - Watch mode with source maps (for development)
- `npm run build` - Production mode with compressed output (for releases)

### Making Theme Changes

1. Edit SCSS files in `src/scss/`
2. Run `npm run dev` to watch for changes
3. Files automatically compile to `theme.css`
4. Use Obsidian's Hot-Reload plugin for automatic theme reloading

**Do not edit `theme.css` directly** - it's auto-generated from SCSS sources.

### Suggested SCSS Structure

Organize SCSS using feature-based partials (example pattern, not prescriptive):

```scss
// src/scss/index.scss
@use 'variables';
@use 'base';
@use 'typography';
@use 'editor';
@use 'sidebars';
@use 'modals';
@use 'mobile';
```

**Recommended organization:**
- `_variables.scss` - SCSS variables (compile-time constants: breakpoints, z-index)
- `_base.scss` - CSS custom properties, resets, global styles
- `_typography.scss` - Font stacks, headings, text styles
- `_editor.scss` - Editor pane, markdown syntax highlighting
- `_sidebars.scss` - Left/right sidebar panels, file explorer
- `_modals.scss` - Dialogs, settings modal, command palette
- `_mobile.scss` - Mobile-specific responsive styles

**CSS Variables vs SCSS Variables:**
- Use CSS custom properties (`--var-name`) for runtime theming (light/dark mode)
- Use SCSS variables (`$var-name`) for compile-time constants
- Follow Obsidian's CSS variable conventions for compatibility

### Version Management

When ready to release a new version:

```bash
npm version patch  # or minor, major
```

This command automatically:
- Builds production CSS (compressed, no source maps)
- Updates `manifest.json` with the new version
- Adds an entry to `versions.json` mapping the theme version to the minimum compatible Obsidian version
- Stages `manifest.json`, `versions.json`, and `theme.css` for commit

### Release Process

Releases are automated via GitHub Actions:

1. Commit and push all changes
2. Create and push a version tag:
   ```bash
   git tag 1.0.1
   git push origin 1.0.1
   ```
3. GitHub Actions automatically:
   - Installs dependencies
   - Compiles SCSS to CSS (`npm run build`)
   - Creates a GitHub release
   - Uploads `manifest.json` and `theme.css`

The workflow (`.github/workflows/release-version.yml`) triggers on any tag push and bundles the necessary files for Obsidian theme distribution.

### CI/CD Integration

The GitHub Actions workflow includes Node.js setup and SCSS compilation:
- Uses Node.js 20 with npm caching
- Runs `npm ci` for clean dependency install
- Runs `npm run build` to compile SCSS before bundling
- Ensures `theme.css` is always up-to-date in releases

## Obsidian Theme Requirements

- `theme.css` must be at the repository root
- `manifest.json` must contain: name, version, minAppVersion, author
- For community theme gallery submission, include a 16:9 screenshot (recommended 512x288px)
- Submit via pull request to `obsidianmd/obsidian-releases` repository

## Version Compatibility

The `versions.json` file ensures users on older Obsidian versions don't update to incompatible theme versions. Format:

```json
{
  "1.0.0": "1.0.0",  // theme version: minimum Obsidian version
  "1.0.1": "1.0.0"
}
```
