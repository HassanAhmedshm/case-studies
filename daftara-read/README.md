# Daftara Read — read-only analytics PWA over an ERP API

**Stack:** Node.js · Express · installable PWA · Daftra REST API

## The problem

The same shop runs its accounting and stock in **Daftra** (دفترة), an Arabic-language ERP. The ERP is
comprehensive and awkward on a phone. The owner needed to check sales, stock and receivables while
away from the shop, without risking a mis-tap that edits the books.

## What I built

A phone-first dashboard that pulls from the Daftra REST API and installs to the home screen like a
native app. Dashboard totals, recent orders, stock alerts and receivables, in Arabic and English, with
the shop's own design language rather than a generic admin theme.

## Decisions worth reading

**Read-only by architecture, not by discipline.** The server issues `GET` requests only. There is no
write path to disable, no permission flag to misconfigure, and no code path in the UI that can mutate
client data. The alternative — a careful UI over a general-purpose client — fails the day someone adds
one convenient button.

**Offline matters more than realtime.** A dashboard that shows yesterday's numbers instantly is more
useful in a shop than one that spins on a weak connection. Cached data with a visible timestamp
beats a live number that never arrives.

**Self-hosted fonts.** All three typefaces ship with the app instead of loading from a CDN, so the
installed version stays correct without a network, and there is no third-party request in the path.

## Notes

The build notes cover the API traps and the iOS PWA behaviours — install prompts, storage eviction and
what actually survives a home-screen install.
