# Hi, I'm Sebastian 👋

**Full-stack developer — React, Node.js, PostgreSQL, and Cloudflare edge runtimes.**

I build complete products end to end: database schema, REST API, authentication,
realtime features, payments, and the deployment that keeps it all running for
$0 a month.

---

## Projects

### 🚚 [CaterMatch](https://github.com/sebastiancapraru1x-rgb/CaterMatch)
**Two-sided marketplace connecting mobile food vendors with event hosts.**

A classifieds marketplace (think Facebook Marketplace, rebuilt for the food-truck
economy) where vendors post available services and hosts post event gigs. One
toggle flips the entire browse experience between the two sides.

**Highlights**
- **Geo-radius search** in pure SQL — a `haversine_km()` helper powers
  10/25/50/100 km filtering with no external geocoding API and no per-request cost.
- **Real-time 1-on-1 chat** over Socket.io rooms, with read receipts, unread
  counts and `last_message_at` inbox sorting.
- **Stripe Checkout + webhooks** for paid listing boosts, with signature
  verification before any boost state is mutated. Runs in a simulated dev mode
  when no keys are present, so the flow is testable without a payment account.
- **JWT + bcrypt auth** with role-based middleware guarding every protected route.
- **Staff-only admin panel** with a ledger of every boost transaction.

**Stack:** React 18 · Vite · Tailwind · Socket.io · Express · PostgreSQL/PostGIS · Stripe · JWT

---

### ⚽ [CleatFarm](https://github.com/sebastiancapraru1x-rgb/CleatFarm)
**A football-boot storefront with an automated supplier-catalog pipeline and a private inventory dashboard.**

A public shop (no account needed, flat pricing, per-product photo galleries with
a lightbox) backed by a password-protected dashboard for inventory, orders and
the import pipeline. The hard part is the pipeline, not the shop.

**Highlights**
- **Referer-aware image proxy with an on-disk cache.** The upstream photo CDN
  returns `HTTP 567` to any request without a matching `Referer`, so a plain
  `<img>` tag renders nothing. Every image is proxied through the app with the
  correct headers and cached to disk — first view ~250 KB, every view after is local.
- **Serverless-first architecture on Cloudflare Workers** (Hono + D1), with a
  Node/Express fallback so the same codebase runs locally and on Render.
- **D1 HTTP driver built by hand** — retry with backoff, chunked batch writes
  respecting the 100-bound-parameter limit, and explicit errors for 401/403/404/500.
- **A sync pipeline that refuses to damage its own data.** If an upstream read
  comes back suspiciously small (expired session, changed layout) it *skips*
  archiving and flags the run instead of emptying the storefront. It also cannot
  oversell, and respects per-item price locks.
- **Password hashing that fits the platform.** scrypt costs ~28 ms on the Workers
  runtime against a ~10 ms budget and blocks the single-threaded isolate, so
  hashing moved to a pepper-keyed HMAC-SHA256 at **0.004 ms** — while still
  verifying legacy scrypt hashes so existing logins survive the migration.
- **A real test suite** (`npm run verify`): 54 selfcheck assertions, port checks,
  auth round-trips, and D1 driver tests — all passing.

**Stack:** Cloudflare Workers · Hono · D1 (SQLite) · Wrangler · Node.js · Stripe · R2

---

## What I care about

- **Security in the small places.** Peppered HMACs over plaintext secrets, HMAC
  keying so a leaked database yields nothing crackable offline, webhook signature
  verification before state changes, and a `.gitignore` that keeps real `.env`
  files and credential material out of version control.
- **Software that defends itself.** The sync pipeline's "refuse to wipe the store
  on a bad read" guard is the single decision in this work I'd point at first —
  correctness under unexpected input, not just correctness on the happy path.
- **Genuinely free infrastructure.** Both projects are designed to build and run
  at **$0/month** on permanent free tiers. No trials, no credit cards.

---

## Contact

Reach me on GitHub — open an issue or a discussion on either repo.

<!--
Want your own profile README like this? It's a single markdown file in a repo
named exactly like your GitHub username.
-->