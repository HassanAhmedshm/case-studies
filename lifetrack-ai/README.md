# LifeTrack AI — offline-first Android app with an edge AI gateway

**Stack:** Kotlin · Jetpack Compose · SQLDelight · Cloudflare Workers · Next.js · TypeScript

## The problem

A health app that gives personalised guidance is a data-hungry product: daily weight, sleep, food,
exercise, workouts. Treating that data as server state means the app is useless without a connection,
and it means holding the most personal data a person has on infrastructure that can leak it.

## What I built

**The Android app** — Kotlin and Jetpack Compose, offline-first, with SQLDelight for local storage. The
database file is the source of truth, so the app opens and works at full speed with no network.

**The gateway** — a Cloudflare Worker that proxies managed AI and nutrition data. Model providers are
called server-side, so no API key ever reaches the device, and prompts, plans, kill switches and
spending limits are controlled centrally.

**The owner console** — a Next.js admin covering accounts, plans, usage, coupons, prompts, provider
keys and kill switches, with the gateway's data contracts shared as typed Zod schemas.

## Decisions worth reading

**User content stays on the device.** Personal logs are not replicated to a server for the feature to
work. That is a product promise, and it is why the offline design is not just a performance choice.

**The gateway refuses rather than degrades.** When a provider credential is missing or invalid, the
request stops before consent, context or spend. A request that cannot be served correctly is a
failure, not a reason to call a different model quietly.

**Money and spend are reserved per request.** The assistant path reserves spend before the call and
releases it on failure, so a retry loop cannot quietly drain a budget.

**Contracts are shared, not copied.** The console, gateway and app validate against one set of schemas,
so a field rename breaks the build instead of production.

## Status

Gateway and console are deployed. The Android app is pre-release — the app is a native Kotlin rewrite
of an earlier React Native build, and the remaining work is device verification rather than scope.
The check suite runs 1,071 tests across the gateway and console.
