# Case studies

Engineering write-ups for products I built and shipped. Code for the client work is private, so these
describe the problem, the architecture and the decisions — the parts that are useful to read.

| Case study | Type | Stack |
|---|---|---|
| [4G Phone Store](./4g-phone-store) | Client work — live in production | Next.js · Supabase · Vercel |
| [Daftara Read](./daftara-read) | Client work — ERP integration PWA | Express · PWA |
| [NAVA](./nava) | Commerce system — bilingual EN/AR | Next.js · Postgres |
| [Systemy](./systemy) | Co-founder — offline-first shop management | TypeScript · Postgres RLS · Workers · Capacitor |
| [LifeTrack AI](./lifetrack-ai) | Product — offline-first Android + edge AI gateway | Kotlin · Jetpack Compose · Cloudflare Workers |

**Live:** [www.4g-phone.com](https://www.4g-phone.com)

---

Most of what is interesting in these projects is not the feature list. It is the constraints: a shop
owner who needs to change 200 prices in under a minute, an ERP API that must never be written to, a
second language that runs right-to-left, and a database where one tenant must never be able to read
another tenant's rows.
