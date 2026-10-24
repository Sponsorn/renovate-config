# renovate-config

Shared [Renovate](https://docs.renovatebot.com/) preset for Sponsorn repos.

Use it from a repo's `renovate.json`:

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": ["github>Sponsorn/renovate-config"]
}
```

## What it does

- **Weekly**: Renovate opens PRs only before 6am on Mondays (Europe/Stockholm).
- **Grouping**: one PR per repo covers all minor and patch updates.
- **Majors** get one PR each.
- **Automerge** applies only to *patch* updates of npm `devDependencies`. They get their own group, and Renovate merges that PR itself only after the status checks pass (`ignoreTests: false`, `platformAutomerge: false`). In repos with no CI, nothing gets green checks, so nothing automerges. Runtime dependencies never automerge.
- **Release age**: npm and PyPI versions must be 3 days old before Renovate proposes them (`minimumReleaseAge`, with `internalChecksFilter: strict`). This protects against freshly hijacked releases.
- **Security**: `vulnerabilityAlerts` bypasses both the schedule and the release-age wait, and the PR is labelled `security`. `osvVulnerabilityAlerts` also checks OSV, so private repos without Dependabot alerts still get security PRs.
- **Managers**: `dockerfile`, `docker-compose`, `nvm` (`.nvmrc`), `nodenv` (`.node-version`) and `github-actions` are explicitly enabled, so base images, Node versions and Actions get bumped. Every other default manager (npm, pep621, pip_requirements, cargo, ...) stays on.

This repo can be private: the Renovate GitHub App reads presets from private repos it is installed on, under the same owner.
