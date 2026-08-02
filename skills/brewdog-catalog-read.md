---
name: Read the BrewDog catalog without authentication
description: Browse BrewDog products, variants, prices and collections through the
  unauthenticated storefront JSON endpoints BrewDog documents for agents, without
  touching the commerce MCP endpoint.
api: examples/brewdog-products-json.json
endpoint: https://brewdog.com
operations:
  - GET /products.json
  - GET /products/{handle}.json
  - GET /collections/{handle}/products.json
  - GET /collections/all
  - GET /search?q={query}&type=product
  - GET /sitemap.xml
generated: '2026-08-02'
method: generated
source: https://brewdog.com/agents.md
grounding: Every endpoint below is listed in BrewDog's own /agents.md under "Read-Only
  Browsing (No Authentication Required)" and was verified with a live anonymous GET
  on 2026-08-02.
---

# Read the BrewDog catalog

BrewDog documents a read-only browsing surface for *"agents that only need to read
store data without transacting"*. No API key, no OAuth, no agent profile.

## Endpoints

| Purpose | Request |
|---|---|
| All products (paged) | `GET https://brewdog.com/products.json?limit=250&page=1` |
| One product | `GET https://brewdog.com/products/{handle}.json` |
| Products in a collection | `GET https://brewdog.com/collections/{handle}/products.json` |
| Browse everything | `GET https://brewdog.com/collections/all` |
| Search | `GET https://brewdog.com/search?q={query}&type=product` |
| URL inventory | `GET https://brewdog.com/sitemap.xml` |

Responses are `application/json; charset=utf-8`.

## What comes back

A `products[]` array. Each product carries `id`, `title`, `handle`, `body_html`,
`vendor`, `product_type`, `tags[]`, `published_at`, and nested `variants[]`,
`images[]` and `options[]`. Each variant carries `sku`, `price`, `compare_at_price`,
`available`, `grams`, `requires_shipping`, `taxable` and `option1..3`. The full field
graph is in `data-model/brewdog-data-model.yml`; a real captured response is in
`examples/brewdog-products-json.json`.

Note `product_type` spans far more than beer — clothing, glassware, gift boxes and
spirits all sit in the same catalog. Filter on `product_type` and `tags` rather than
assuming everything is a beer.

## Regional stores are separate catalogs

`brewdog.com` (GBP), `usa.brewdog.com` (USD) and `de.brewdog.com` (EUR) are distinct
storefronts with their own `/products.json`, their own `/.well-known/ucp` profile and
their own pricing. Pick the host that matches the buyer's country; do not translate
prices yourself.

## Etiquette

- Honour `https://brewdog.com/robots.txt`. It disallows `/cart`, `/checkout`,
  `/checkouts/`, `/orders`, `/account`, `/admin` and faceted `/collections/*sort_by*`
  and `/collections/*+*` URLs.
- These endpoints are not documented as rate-limited, but the commerce endpoint is
  per-IP limited — be conservative and back off on any 429.
- The catalog is not a recipe database. BrewDog's openly published recipe archive is
  **DIY Dog**, and the "Punk API" built on it is a third-party project, not BrewDog's.

## To transact

Use the UCP MCP endpoint instead — see `brewdog-agentic-purchase.md`.
