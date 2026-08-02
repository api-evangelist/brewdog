---
name: Buy from BrewDog as an agent (UCP over MCP)
description: Search the BrewDog catalog, build a cart, run a checkout and place an order
  through BrewDog's Universal Commerce Protocol MCP endpoint, with the buyer-approval
  and rate-limit rules BrewDog publishes.
api: mcp/brewdog-mcp.yml
endpoint: https://brewdog.com/api/ucp/mcp
operations:
  - search_catalog
  - lookup_catalog
  - get_product
  - create_cart
  - update_cart
  - get_cart
  - create_checkout
  - update_checkout
  - get_checkout
  - complete_checkout
  - cancel_checkout
  - get_order
generated: '2026-08-02'
method: generated
source: https://brewdog.com/agents.md
grounding: Tool names come from the UCP Shopping Service OpenRPC schema that BrewDog's
  own /.well-known/ucp declares for this endpoint (mcp/brewdog-ucp-shopping-mcp.openrpc.json);
  the step order and the rules come verbatim from BrewDog's /agents.md.
---

# Buy from BrewDog as an agent

BrewDog's online store is a Shopify storefront that implements the
[Universal Commerce Protocol](https://ucp.dev). BrewDog documents the agent contract
itself at <https://brewdog.com/agents.md> (mirrored at `/llms.txt`).

## Before you start

1. **Discover.** `GET https://brewdog.com/.well-known/ucp`. Confirm `ucp.version`
   (currently `2026-04-08`) and that `services["dev.ucp.shopping"]` lists a `transport: mcp`
   entry. The `endpoint` in that profile is the canonical MCP URL; `https://brewdog.com/api/ucp/mcp`
   fronts it on the brand host.
2. **Bring an agent profile.** An anonymous `tools/list` against the MCP endpoint is
   rejected: HTTP 422, JSON-RPC error `-32001` `UCP discovery failed`, with
   `data.code = invalid_profile_url` and `"Missing profile uri"`. You must present a
   resolvable UCP agent profile URI before any tool is listed or called.
3. **Decide whether you should be here at all.** If you are a personal shopping
   assistant that cannot obtain contemporaneous buyer approval at the moment of
   payment, BrewDog explicitly directs you to install `https://shop.app/SKILL.md`
   and transact through Shop Pay instead of driving this endpoint.

## Flow

All calls are JSON-RPC 2.0 `POST` to `https://brewdog.com/api/ucp/mcp` with
`Content-Type: application/json` and `Accept: application/json, text/event-stream`.

1. **Find products** — `search_catalog` with the buyer's intent. For known
   identifiers use `lookup_catalog` (batch) or `get_product` (single).
2. **Build the cart** — `create_cart` with the chosen variants, then `update_cart`
   to adjust quantities. `get_cart` re-reads state; `cancel_cart` abandons it.
3. **Start checkout** — `create_checkout` from the cart.
4. **Fulfilment** — `update_checkout` to set the shipping address and method.
   BrewDog's profile sets `allows_multi_destination.shipping: false` and only allows
   the `["shipping"]` method combination, so do not attempt a split-destination order.
5. **Confirm** — `get_checkout` and show the buyer the full total, shipping and any
   discount before asking for approval.
6. **Complete** — `complete_checkout` **only after explicit buyer consent**.
   `cancel_checkout` backs out.
7. **Follow up** — `get_order` for order state.

## Rules you must honour

- **Human approval is mandatory at payment.** BrewDog states: *"Checkout requires
  human approval. Agents must not complete payment without explicit buyer consent."*
  Never call `complete_checkout` on your own judgement.
- **Pass buyer context.** Send `context.address_country` and `context.currency`, or
  pricing and availability will be wrong. BrewDog runs separate regional storefronts
  (`brewdog.com`, `usa.brewdog.com`, `de.brewdog.com`), each with its own UCP profile.
- **Back off on 429.** The MCP endpoint is rate-limited per IP.
- **Age-restricted goods.** BrewDog sells alcohol and gates the site behind an
  over-18 confirmation. Do not attempt to complete an alcohol purchase for a user
  you cannot confirm is of legal drinking age in the delivery country.
- **Payment handlers** available are Google Pay (`gpay`), Shopify Card
  (`shopify.card`) and Shop Pay (`shop_pay`). Do not handle raw card data yourself.

## Errors

Errors are JSON-RPC 2.0 error objects, not RFC 9457 problem documents. See
`errors/brewdog-problem-types.yml`. The one you will hit first is `-32001`
`invalid_profile_url` — fix your agent profile, not your payload.

## What this endpoint will not do

There is no BrewDog OpenAPI, no webhook or event surface, no A2A agent card and no
status page. For read-only catalog work you do not need MCP at all — see
`brewdog-catalog-read.md`.
