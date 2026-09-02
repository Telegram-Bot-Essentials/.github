# .github

Org-wide defaults for [Telegram Bot Essentials](https://github.com/Telegram-Bot-Essentials).

## Community health files

GitHub applies `CONTRIBUTING.md`, `CODE_OF_CONDUCT.md`, `SECURITY.md`, and the
`.github/ISSUE_TEMPLATE` / `pull_request_template.md` set as defaults to every repo in the
org that doesn't ship its own copy. `profile/README.md` renders on the
[org profile page](https://github.com/Telegram-Bot-Essentials).

## Reusable workflows

| Workflow | Used by | Purpose |
|---|---|---|
| `.github/workflows/ci.yml` | every package's `tests.yml` | PHP × Laravel matrix — Pest, Pint, PHPStan, `composer validate --strict`, a non-blocking `--prefer-lowest` cell — behind one `ci-passed` gate |
| `.github/workflows/release.yml` | every package's `release.yml` | on a pushed `v*` tag, creates the GitHub Release with generated notes |

```yaml
# <package>/.github/workflows/tests.yml
name: tests
on:
  push:
    branches: [main]
  pull_request:
jobs:
  ci:
    uses: Telegram-Bot-Essentials/.github/.github/workflows/ci.yml@main
```

## Renovate

`default.json` is the shared Renovate preset. Each repo's `renovate.json` is just:

```json
{ "extends": ["github>Telegram-Bot-Essentials/.github"] }
```
