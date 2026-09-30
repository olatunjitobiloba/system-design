# SauceDemo Findings

Reverse-engineered from the live site at `saucedemo.com` using Chrome DevTools.
Everything in the As-Is section below came from observed runtime behavior. The
As-If section is inference, and it is labelled as such.

## Evidence log

| Source | What it showed |
|---|---|
| Network tab (Fetch/XHR filter) | Zero API calls across login, browse, add-to-cart, checkout |
| Sources tab, JS bundle search | Hardcoded user array: `standard_user`, `locked_out_user`, `problem_user`, `secret_sauce` |
| Application tab, Local Storage | `cart-contents`, `backtrace-guid`, `backtrace-last-active` |
| Wappalyzer | React SPA, PWA, GitHub Pages |
| Response headers | Fastly CDN in front of Varnish |

## As-Is architecture

**What it is:** React SPA, built as a PWA

**Frontend:** React + React Router 7.18.3

**Backend:** None. No API or XHR calls appeared in the Network tab during any
flow I exercised.

**Auth:** Client-side hardcoded array match. The four usernames and their
passwords ship inside the JS bundle, so anyone can read them from Sources.

**Data persistence:** Cart state in browser localStorage under `cart-contents`.
Cleared with the browser profile, scoped to one device.

**Third-party integration:** Backtrace.io for error and crash monitoring,
identified via the `backtrace-guid` and `backtrace-last-active` localStorage keys.

**Hosting:** GitHub Pages

**CDN/caching:** Fastly as the CDN, Varnish as the cache layer

**Conclusion:** SauceDemo is a static React PWA hosted on GitHub Pages behind
Fastly and Varnish. It has no backend, no real authentication, and no database.
It does have one live third-party dependency in Backtrace.

## As-Is vs As-If

The delivery tier is real infrastructure, and it is worth separating from
application logic. Fastly, Varnish, and GitHub Pages cache and serve files. None
of them run application code, so none of them stand in for the missing backend.

Three gaps separate this system from anything you would run in a production
commerce context.

### 1. Authentication

**As-Is:** Login compares the submitted username against an array in the JS
bundle. The credentials are public. `locked_out_user` exists to demonstrate a
failure path, not to enforce one.

**As-If:** An auth service holding credentials server-side, issuing a signed
session token (OAuth 2.0 or JWT), with the client presenting that token on every
request. The browser never decides who the user is.

**Why it matters:** Client-side auth is a display convenience, not a control.
Anything the browser can read, an attacker can read, and the check can be
skipped entirely by editing local state.

### 2. Persistence

**As-Is:** `cart-contents` in localStorage is the entire data layer. Orders,
users, and inventory have no server-side record at all.

**As-If:** A persistent database holding users, products, carts, and orders.
The client reads and writes through an API rather than owning state outright.

**Why it matters:** A cart that lives in one browser profile cannot survive a
device change, cannot sync across sessions, and gives the business no record of
a transaction once the tab closes.

### 3. Server-side validation

**As-Is:** The checkout form collects name, zip, and address, then renders a
confirmation. No server recomputes anything, because no server participates.

**As-If:** The server recalculates line totals, prices, and discounts from its own
records, rejects any mismatch, and is the only component allowed to declare an
order complete.

**Why it matters:** When the client computes the total, the client decides the
total. A production system treats every value arriving from the browser as
untrusted input, including prices.

None of these gaps are defects. SauceDemo is built as a UI test-automation
fixture, so a deterministic client-only app serves the purpose. The gaps become
relevant the moment real money or real customer data is involved, which is why
the distinction belongs in the writeup rather than only on the diagrams.
