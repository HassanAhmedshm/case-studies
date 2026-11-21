# Systemy — offline-first shop management for Egyptian micro-merchants

**Role:** Co-founder, and the main author of the current build — 153 of 184 commits between
September and November 2026.
**Stack:** TypeScript · Next.js · PostgreSQL (RLS) · Cloudflare Workers · Capacitor/Android ·
Tailwind v4 · shadcn · Vitest

## The problem

A shop with one counter keeps its books on paper, in a notebook, or in a WhatsApp chat with itself.
The bookkeeping that actually matters to that shop is small and specific: who owes me, what did I sell,
what did I spend, and does the drawer match at the end of the day.

Every existing option loses on one of three things. Accounting suites are built for a bookkeeper, not
a shop owner. POS apps need a stable connection and a monthly card payment. And Arabic is treated as
a translation layer bolted onto an English product.

## What we built

A shop-management product for Egyptian micro-merchants, built so that it works with no connection, in
Arabic and English as equals, on the phone the owner already has.

Coverage includes the debt ledger, single-amount cash sales, expenses, the cash drawer close, customer
and supplier balances, inventory and pricing by weight, work tickets, field-agent commission, plan
payments, and a support inbox — 78 screens rebuilt as one design system on Tailwind v4 and shadcn.

## Screenshots

Captured from the project's own demo rig — a disposable Postgres, the static export and the
local Worker — with transactions recorded through the UI rather than painted into a mockup.

| | |
|---|---|
| ![Home](./screenshots/systemy-home.png) | ![Reports](./screenshots/systemy-reports.png) |
| Daily home: sales net of expenses, trial state, plan | Reports: period filter, net, sales/expense bars |
| ![Day close](./screenshots/systemy-day-close.png) | ![Records](./screenshots/systemy-records.png) |
| Cash-drawer close, broken down per payment method | The ledger: sales, expenses, customers, in one stream |
| ![Arabic](./screenshots/systemy-home-arabic.png) | ![Phone](./screenshots/systemy-phone.png) |
| Same screen in Arabic — the layout mirrors, it is not a translation overlay | Phone width is the primary target, not an afterthought |

![Landing](./screenshots/systemy-landing-arabic.png)


## Decisions worth reading

**One domain model, two clients.** The web app and the Android app are not two products. They share
one tenant-scoped domain model, so a debt entered on the phone is the same debt on the web app.
Android is Capacitor over the same code, which is a deliberate trade: one implementation of the
domain, and the cost is paid only where native behaviour is genuinely needed.

**The offline outbox is typed, not a queue of blobs.** Mutations are queued with a `recordKind`
discriminator that says what kind of change it is. That distinction is not cosmetic — a display-layout
change, a bounded price update, a cash-drawer close and a ledger entry all sync differently, and
flattening them into "a pending write" is how offline systems corrupt books.

**Tenant isolation is enforced at the wall, and again in the database.** Every request resolves
authorization from the signed-in identity and current membership, never from a client-supplied shop
id. Then Postgres row-level security enforces the same rule independently. The reasoning is that one
of the two will eventually have a bug, and the other has to hold.

**The ledger is append-only in the database, not in the application.** Migration 0098 adds triggers
that reject `UPDATE`, `DELETE` and `TRUNCATE` on ledger rows. Verified directly: as a superuser on a
disposable database, each of the three aborts with `SQLSTATE 23514` and the row survives. A book that
can be edited is not a book.

**The AI assistant refuses instead of degrading.** When a provider credential is missing or invalid,
the request stops before consent, before shop context is sent, and before spend is reserved. The
verifier calls the provider's models endpoint with auth only — no prompt, no shop data. A request that
cannot be served correctly fails closed and stays non-retryable, rather than silently falling back to
a different model.

**Spend is reserved before the call and released on failure.** A retry loop cannot drain a budget, and
a refunded reservation is still counted against the request count.

**Case-handling rules live in the database, not only in the UI.** Order and payment states move
through defined transitions; the checks exist below the screen that draws the buttons.

## Verification

At the last full run: 690 server tests across 140 files, 1,244 web tests across 196 files (3 skipped),
`typecheck` clean, plus 312 runtime checks. The suite covers 496 test files and 72 migrations.

The project ran repeated pre-launch audit passes — each one enumerating findings card by card, with a
ship/no-ship verdict per card. The last pass worked 19 cards to closure before re-running the gates.
Writing down that a build is *not* shippable, in a numbered list, is the part most teams skip.

## Where it stands

Migrations 0093–0101 are reviewed drafts and are not applied to a hosted database, and the production
Worker has not received the reviewed assistant code. I am stating that plainly because "deployed" and
"verified" are not the same claim, and it is the difference a reader should be able to see.
