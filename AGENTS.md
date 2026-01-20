# AGENTS.md

Guidance for AI agents working with this repository.

## Overview

- Configuration templates for [Recyclarr](https://github.com/recyclarr/recyclarr), a CLI tool that
  syncs TRaSH Guides recommendations to Radarr/Sonarr
- Templates provide ready-to-use configurations users can customize
- Issue tracking via Linear (internal only)

## Branching

Version-aware branches track Recyclarr major versions. Recyclarr auto-selects: `v{major}` then
`master` then `main`.

- **Before any work**: Verify checked-out branch matches intent
- **New features/breaking changes**: Target `v{major}` branch (e.g., `v8` for Recyclarr 8.x)
- **`master`**: Maintenance mode for older versions - critical fixes only
- Breaking changes (ID removals, schema changes) are acceptable on version branches

See `CONTRIBUTING.md` for full branching policy.

## Structure

Two-tier template system:

- `templates.json` - Maps template IDs to top-level files (user-facing entry points)
- `includes.json` - Maps include IDs to reusable components

Top-level templates reference includes via `include:` directive using IDs from `includes.json`.

```txt
radarr/
  templates/           # Top-level templates (referenced in templates.json)
  includes/
    custom-formats/    # Custom format definitions
    quality-definitions/
    quality-profiles/
    sqp/               # Special Quality Profiles (storage-optimized)

sonarr/
  templates/           # Top-level templates for Sonarr v4
  includes/
    custom-formats/
    quality-definitions/
    quality-profiles/
```

### Naming Conventions

Templates: `{resolution}-{source}-{language-variant}.yml`

- Resolution: `hd` (1080p), `uhd` (2160p)
- Source: `bluray-web`, `remux-web`, `web`
- Language variants: `french-vostfr`, `french-multi-vf`, `french-multi-vo`, `german`
- SQP templates: `sqp-{1-5}` for size-quality balanced profiles

## Constraints

- Templates MUST include `Updated:` date in header comment
- Modified templates MUST update date to current `YYYY-MM-DD`
- Template headers MUST include documentation links

## Commits & PRs

Commits with `feat:` or `fix:` trigger Discord notifications via `notify.yml`. Choose types
carefully.

### Type Selection

- `feat:` - New files in `*/templates/**`, `*/includes/**`, or new entries in `*.json`
- `fix:` - Modifications to existing template/include files
- `docs:` - `*.md`, `LICENSE`
- `ci:` - `.github/workflows/**`
- `chore:` - Everything else (default)

### Scopes

- `radarr/**` - `(radarr)`
- `sonarr/**` - `(sonarr)`
- `templates.json`, `includes.json` - `(config)`

### Breaking Changes (!)

- Template or include ID renames/removals
- Schema changes requiring user config updates

### CI Checks

- `yaml-lint.yml` - YAML syntax (all pushes/PRs)
- `check-paths.yml` - Paths in `templates.json` exist
- `check-trash-ids.yml` - Trash IDs valid against TRaSH-Guides (PRs only)
- `check-dates.yml` - `Updated:` dates in headers (PRs only)
