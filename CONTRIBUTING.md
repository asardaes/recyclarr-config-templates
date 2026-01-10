# Contributing

Helpful information for contributing to the config templates repo.

## YAML Lint: Local Testing

In order to test locally, you first must install [Python 3] and [yamllint].

On Windows, you must set an environment variable: `PYTHONUTF8=1`. This is required otherwise you'll
see a bunch of `line-lenth` lint errors because the French YAML files use unicode characters. See
[this issue][line-length-bug] for more information.

To execute YAML lint locally, `cd` to the root of the repo and run:

```bash
yamllint .
```

[Python 3]: https://www.python.org/downloads/
[yamllint]: https://yamllint.readthedocs.io/en/stable/quickstart.html#installing-yamllint
[line-length-bug]: https://github.com/adrienverge/yamllint/issues/530#issuecomment-1402452147

## Branching Strategy

Config-templates uses version-aware branches to maintain compatibility across Recyclarr major
versions.

### Branch Structure

- `v{major}` branches (e.g., `v8`, `v9`): Templates for specific Recyclarr major versions
- `master`/`main`: Fallback branch, currently serves v7 and earlier

### How Recyclarr Selects Branches

Recyclarr automatically selects the appropriate branch:

1. Tries `v{major}` matching its version (e.g., Recyclarr 8.x tries `v8`)
2. Falls back to `master`, then `main`

Users can override with explicit `reference` in settings.yml.

### Support Policy

Users experiencing outdated configs or drift from TRaSH Guides should upgrade to the latest major
version of Recyclarr. Template maintainers are not expected to backport non-critical updates or
maintain multiple versions of config templates.

### Contributing Guidelines

- **New features**: Target the `v{major}` branch matching the current/upcoming Recyclarr release
- **Critical fixes for older versions**: May be applied to `master` (v7) on a case-by-case basis
- **v7 (master) policy**: Maintenance mode only - no new features, only critical fixes

### Creating a New Version Branch

When a new Recyclarr major version is released:

1. Create `v{new}` branch from the previous version branch (or `master` if no version branch exists)
2. The new branch inherits all templates and can diverge as needed
3. Previous version branch enters maintenance mode

### Example Workflow

For Recyclarr v8:

- `v8` branch: Active development, simplified templates using guide-backed profiles
- `master`: v7 maintenance mode, frozen except for critical fixes
- When v9 releases: create `v9` from `v8`, and `v8` enters maintenance mode
