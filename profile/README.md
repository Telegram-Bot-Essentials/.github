# Telegram Bot Essentials

A Laravel framework for running many Telegram bots from one codebase, each bot its own
tenant. Incoming webhook updates are dispatched through typed buses — **Commands**,
**ReplyKeys**, **CallbackQueries**, and **StateAnswers** — instead of one giant update
handler, so a multi-tenant, multi-locale bot platform stays organized as it grows.

## Packages

All published on Packagist under `telegram-bot-essentials/*`.

| Package | Purpose |
|---|---|
| [`essence`](https://github.com/Telegram-Bot-Essentials/essence) | Core framework: webhook routing, buses, state machine, per-bot tenancy, access control |
| [`settings`](https://github.com/Telegram-Bot-Essentials/settings) | Per-bot typed key/value settings |
| [`billing`](https://github.com/Telegram-Bot-Essentials/billing) | Invoices, payments, and a pluggable gateway registry |
| [`gateway-card`](https://github.com/Telegram-Bot-Essentials/gateway-card) | Manual card-to-card payment gateway for Billing |
| [`gateway-zibal`](https://github.com/Telegram-Bot-Essentials/gateway-zibal) | Zibal payment gateway for Billing |
| [`user-wallet`](https://github.com/Telegram-Bot-Essentials/user-wallet) | Per-user balance/credit wallet |
| [`user-management`](https://github.com/Telegram-Bot-Essentials/user-management) | Admin user list with sortable/filterable columns |
| [`affiliates`](https://github.com/Telegram-Bot-Essentials/affiliates) | Referral tracking and commission payout via the wallet |
| [`announcements`](https://github.com/Telegram-Bot-Essentials/announcements) | Bulk broadcast to bot users with live progress and retraction |
| [`skeleton`](https://github.com/Telegram-Bot-Essentials/skeleton) | `composer create-project` template for scaffolding a new companion package |

## Getting started

```bash
composer require telegram-bot-essentials/essence
php artisan tbe:install
php artisan migrate
```

Every companion package depends on `essence` and registers itself into its buses and event
system from its own service provider. Install only what you need.

## Contributing

Issues and pull requests are welcome across every repo. The shared
[contributing guide](https://github.com/Telegram-Bot-Essentials/.github/blob/main/CONTRIBUTING.md),
[code of conduct](https://github.com/Telegram-Bot-Essentials/.github/blob/main/CODE_OF_CONDUCT.md),
and [security policy](https://github.com/Telegram-Bot-Essentials/.github/blob/main/SECURITY.md)
apply to all of them.
