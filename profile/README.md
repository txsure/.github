<div align="center">

<img src="./assets/logo.png" alt="txsure" width="104" />

# txsure

**Human-in-the-loop payment infrastructure for Ethiopia.**

Self-hosted · MIT licensed · txsure never holds or moves your money

[![MIT License](https://img.shields.io/badge/license-MIT-blue.svg)](https://github.com/txsure/txsure/blob/main/LICENSE)
[![npm](https://img.shields.io/npm/v/%40txsure%2Fsdk?logo=npm&logoColor=white&label=%40txsure%2Fsdk)](https://www.npmjs.com/package/@txsure/sdk)
[![deploy](https://img.shields.io/badge/deploy-self--hosted-2ea043)](https://github.com/txsure/txsure#self-hosting-production)
[![made in Ethiopia](https://img.shields.io/badge/made%20in-Ethiopia-078930)](https://github.com/txsure)

</div>

## The problem

Ethiopian developers have no standard way to accept bank transfers. Every team rebuilds
the same flow from scratch: show the payer an account number, collect a transaction
reference and a screenshot, then verify it by hand — in a spreadsheet, a Telegram group,
or a dashboard someone hacked together over a weekend.

Money moves through bank apps and SMS alerts. Nothing ties that transfer back to an order
in your system, and nothing tells your server that it was verified.

## What txsure is

txsure is the missing verification layer for that flow. It turns a manual bank transfer
into a documented, signed API event:

1. Your backend calls the SDK and gets a **hosted checkout URL**.
2. The payer picks their bank, sees your account details, pays in their banking app, and
   submits the transaction reference (plus an optional screenshot).
3. An automatic **receipt check** hits the bank's own verification endpoint where the bank
   exposes one (CBE, CBE Birr, Telebirr), then the merchant gets the submission in the
   dashboard and on Telegram.
4. The merchant **approves or rejects** with one tap.
5. txsure fires a **signed webhook** (`payment.submitted`, `payment.approved`,
   `payment.rejected`) so your server can fulfil the order.

## What txsure never does

- It does **not** process, hold, or move money. Funds land in the merchant's own bank
  account. txsure only records and verifies the claim.
- It does **not** run a hosted cloud. There is no `txsure.com` SaaS and no signup page we
  operate. You deploy an instance and you own the database, the secrets, and the payment
  data.
- It does **not** phone home. No telemetry, no license checks, no remote kill switch.

That is the whole point: financial data belongs on infrastructure its owner controls.

## Features

| | |
|---|---|
| **Hosted checkout** | Public, unauthenticated page per payment session at `/c/:id`. Bank picker, copy-friendly account details, reference and screenshot capture. |
| **Merchant dashboard** | Transactions, payment accounts, customers, webhooks, bots, API keys, team members. |
| **Signed webhooks** | HMAC-SHA256 signatures, idempotency keys, BullMQ retries with exponential backoff, and a dead-letter queue. |
| **Receipt verification** | Pluggable per-bank verifiers (CBE, CBE Birr, Telebirr) that validate the payer's reference before a human reviews it. |
| **Telegram bot** | Submissions and approve/reject buttons delivered where merchants already are. |
| **TypeScript SDK** | [`@txsure/sdk`](https://www.npmjs.com/package/@txsure/sdk) for creating sessions and verifying webhooks on the server. |
| **Single image deploy** | One Docker image plus Postgres and Redis, with Caddy TLS handled for you by the install wizard. |

## Inside

| Layer | Choice |
|---|---|
| API | Express + TypeScript, Zod validation, `/api/v1` |
| Web | Vite + React + shadcn/ui, TanStack Query, nuqs |
| Data | Drizzle ORM on PostgreSQL |
| Auth | better-auth (sessions in the same database) |
| Jobs | BullMQ on Redis (webhooks, bots, verification) |
| SDK | `@txsure/sdk` — TypeScript first, published with Changesets |

## Repositories

| Repository | What it is |
|---|---|
| [txsure/txsure](https://github.com/txsure/txsure) | The product: API, dashboard, checkout, SDK, Docker deploy. Monorepo, MIT. |
| [txsure/.github](https://github.com/txsure/.github) | This profile plus the community health files shared by our repositories. |

## Deploy it

```bash
curl -fsSL https://raw.githubusercontent.com/txsure/txsure/main/scripts/install.sh | bash
```

The wizard installs Docker, asks for your domain and bank details, generates the
encryption secrets, configures Caddy with automatic HTTPS, and leaves you a `txsure`
management CLI. Prefer to read before you run? See
[`docs/self-hosting.md`](https://github.com/txsure/txsure/blob/main/docs/self-hosting.md).

## Community

- **Questions and ideas** — [Discussions](https://github.com/txsure/txsure/discussions)
- **Bugs and features** — [Issues](https://github.com/txsure/txsure/issues)
- **Security reports** — never in a public issue; see [SECURITY.md](SECURITY.md)
- **Contributing** — see [CONTRIBUTING.md](https://github.com/txsure/txsure/blob/main/CONTRIBUTING.md)

We follow the [Contributor Covenant](CODE_OF_CONDUCT.md). Start with a
`good first issue` if you want a small, well-scoped first contribution.

## Credits

The bank receipt-verification endpoints (CBE, CBE Birr, Telebirr) are based on research
from [cheki](https://github.com/1RB/cheki) by [ray (1RB)](https://github.com/1RB) — an
MIT-licensed Ethiopian bank receipt verifier.

---

<sub>MIT licensed. Built in Ethiopia for developers who would rather own their data.</sub>
