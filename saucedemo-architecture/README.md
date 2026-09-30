# SauceDemo Architecture Analysis (C4 Model)

## Task
Given a live site (saucedemo.com) with no documentation, reverse-engineer 
and document its architecture using the C4 model.

## Method
1. Inspected live behavior via Chrome DevTools (Network, Sources, Application tabs)
2. Confirmed stack via Wappalyzer
3. Modeled findings using C4: Context → Container → Component

## Key Finding
SauceDemo is a static React PWA with zero backend — auth, cart, and 
"persistence" are all client-side. Its only real external dependency 
is Backtrace.io for error monitoring. Delivery is handled by 
GitHub Pages behind Fastly/Varnish.

## Diagrams
- [Context + Container](diagrams/01-context-and-container.png) — actors,
  external systems, and the delivery path (Fastly → Varnish → GitHub Pages)
- [Component](diagrams/02-component-checkout.png) — checkout flow internals

The first view merges the Context and Container levels. SauceDemo has a small
enough footprint that one page stays readable, and the elements do not overlap:
Context contributes the End User, the system boundary, and Backtrace.io, while
Container contributes the PWA, localStorage, and the CDN chain.

Source for both diagrams: [saucedemo-c4.drawio](diagrams/saucedemo-c4.drawio)
(editable in app.diagrams.net). Export to PDF with Crop rather than Page View,
otherwise each diagram tiles across several sheets.

## As-Is vs As-If
[findings.md](findings.md) separates observed behavior from inference, then
covers the three gaps between this system and a production commerce build:
server-side authentication, persistent storage, and server-side order
validation.
