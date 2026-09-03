# Contributing

This guide applies to every `telegram-bot-essentials/*` package. `essence` also carries its
own `CONTRIBUTING.md` with a few package-specific notes — read that one too when working on
the core.

## Setup

```bash
composer install
```

Tests run against an in-memory SQLite database via
[Orchestra Testbench](https://packagist.org/packages/orchestra/testbench) — no external
database or Telegram credentials needed. Companion packages resolve `essence` (and any
sibling packages they depend on) straight from Packagist.

## Before opening a PR

```bash
composer test      # Pest
composer lint      # Pint, check only
composer format    # Pint, fixes in place
composer analyse   # PHPStan / Larastan, level max
```

All four run in CI across the PHP/Laravel matrix in `.github/workflows/tests.yml`; a PR
won't merge red.

If PHPStan flags something in code you didn't touch, that's pre-existing debt frozen in
`phpstan-baseline.neon` — leave it alone unless your change specifically improves it. New
code should not add to the baseline; if `composer analyse` fails on a line you wrote, fix
the type rather than regenerating the baseline to hide it.

## Commit style

[Conventional Commits](https://www.conventionalcommits.org/) as `type(scope): summary`,
where `type` is one of `feat` / `fix` / `refactor` / `chore` / `docs` / `test` / `ci`, with
a `!` before the colon and a `BREAKING CHANGE:` footer for anything that breaks the public
API.

Slice a change into **one commit per layer, committed bottom-up** — schema → model →
service/handler → resource → tests → docs is that many commits, not one bundled `feat`. The
`scope` names the work stream and stays constant across the whole slice, so
`git log --grep <scope>` returns it whole; the `type` tracks each layer. When a change is
also split by permission tier, tag the tier at the end of the summary:

    refactor(essence-0.12): guard pruned message meta on resume (Admin)

Summaries are lowercase and imperative. Substantive commits carry a wrapped body explaining
*why*; trivial chores carry none — `git log` is the reference for tone. No AI-attribution
or `Co-Authored-By` trailers.

## Versioning

Every package is pre-1.0: a minor version bump may include breaking changes while the API
stabilizes. Breaking changes still need a `!` / `BREAKING CHANGE:` marker and a
`CHANGELOG.md` entry so downstream consumers can find them at a glance.

## Cross-package changes

`essence` is the core; the other packages extend it via the same bus/handler pattern, and
several depend on each other (Billing → Settings; the gateways and Wallet → Billing;
Affiliates → Wallet). If your change touches a hook or interface other packages rely on
(bus signatures, handler base classes, config keys, events), call that out explicitly in
the PR description and check the dependents.
