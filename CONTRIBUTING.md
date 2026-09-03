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

[Conventional Commits](https://www.conventionalcommits.org/) (`feat:`, `fix:`, `refactor:`,
`chore:`, `docs:`, `test:`, `ci:`), with a `!` after the type and a `BREAKING CHANGE:`
footer for anything that breaks the public API. `git log` is the best reference for the
tone and level of detail expected in a commit body — explain *why*, not just what changed.

### One commit per layer, not one commit per feature

A change that spans migration → model → validation → service/handler → resource → tests →
docs is that many commits, committed bottom-up — not a single large `feat`. The cut is the
architectural layer the code lives in, ranked roughly:

```
migration/schema < model/entity < request/validation/rule
  < service/controller/handler/job < resource/serializer/view
  < test/spec < doc/readme < config/chore
```

Rules for the slice:

- **Scope is the feature and stays constant** across the whole series, so
  `git log --grep <scope>` returns it whole. **Type is each layer's nature** (`feat` for
  the behavior commit, `test` for its tests, `docs` for the doc update, …).
- Subject ≤50 chars, lowercase, imperative mood.
- Substantive commits carry a wrapped body saying *why*; trivial chores carry none.
- No AI-attribution or `Co-Authored-By` trailers.
- Intermediate commits need not be individually green (the model commit lands before its
  migration's tests); the series as a whole must build.
- Split a single file across two commits only when it genuinely holds two unrelated changes.

Two unrelated top-level areas (say `routes/` and `app/Providers/`) are two commits even
when they share a layer rank.

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
