# Security policy

txsure handles API keys, webhook signing secrets, bank account details, and payer
submissions. We take reports seriously and we will not pursue or penalise anyone who
reports a vulnerability in good faith.

## Reporting a vulnerability

**Do not open a public issue.** Use GitHub's private advisory form:

<https://github.com/txsure/txsure/security/advisories/new>

That channel is visible only to you and the maintainers. If GitHub is unavailable,
contact a maintainer directly through their GitHub profile.

Please include:

- the affected component (`apps/api`, `apps/web`, `packages/sdk`, Docker image)
- the version, image tag, or commit
- what an attacker can do, and the conditions required
- reproduction steps or a proof of concept, if you have one

## What to expect

| Stage | Target |
|---|---|
| Acknowledgement | within 3 working days |
| Initial assessment and severity | within 7 working days |
| Fix or mitigation plan shared with you | within 30 days |
| Public advisory | after a fix ships, coordinated with you |

We will credit you in the advisory unless you ask us not to.

## Supported versions

txsure is pre-1.0. Security fixes land on `main` and ship in the next tagged release or
Docker image. If you self-host, pin an image tag and upgrade when a security release is
published.

## In scope

- Authentication and session handling, including better-auth configuration
- API key issuance, storage, and comparison
- Webhook signing, verification, and replay protection
- The public checkout page (`/c/:id`): session isolation, uploads, enumeration
- Receipt verification logic and SSRF reachability of bank endpoints
- Secret handling in the Docker image, install script, and generated `.env`
- Anything that would let one merchant instance read another merchant's data

## Out of scope

- Vulnerabilities in dependencies that are already public, without a txsure-specific
  impact path (report them upstream; our Dependabot config will pick up advisories)
- Misconfiguration of your own deployment: open Postgres or Redis ports, a shared
  `BETTER_AUTH_SECRET`, missing TLS, or an exposed `.env`
- Social engineering, spam, or physical access
- Denial of service from traffic volume alone
- Automated scanner output with no demonstrated impact

## A note on funds

txsure does not process, hold, or transfer money. A payment is a bank transfer between the
payer and the merchant, verified by a human. Compromising a txsure instance cannot drain
funds by itself, but it can let an attacker approve fraudulent submissions or read payer
data — treat it as sensitive.
