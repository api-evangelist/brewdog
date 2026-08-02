# BrewDog

BrewDog PLC (company no. SC311560) is a Scottish craft brewer, bar and hotel operator
headquartered in Ellon, Aberdeenshire. It runs no conventional developer program, but
its direct-to-consumer store at brewdog.com is a Shopify storefront with a real,
published agent-facing API surface.

- Website — https://www.brewdog.com/
- Agent instructions — https://brewdog.com/agents.md (mirrored at /llms.txt)

## API surface

| Surface | Endpoint | Auth |
|---|---|---|
| Agentic commerce (UCP over MCP) | `https://brewdog.com/api/ucp/mcp` | UCP agent profile URI; buyer approval at payment |
| UCP merchant profile | `https://brewdog.com/.well-known/ucp` | none |
| Customer account OIDC discovery | `https://brewdog.com/.well-known/openid-configuration` | none (discovery) |
| Storefront product JSON | `https://brewdog.com/products.json` and friends | none |

No OpenAPI, no AsyncAPI, no webhooks, no A2A agent card, no `security.txt` and no
status page were found on any BrewDog host.

## Notes

- **BrewDog PLC is in administration.** The brewdog.com footer states that Clare
  Kennedy, Ian Partridge and Ben Browne were appointed Joint Administrators of
  BrewDog plc. See https://brewdog.com/pages/important-notice.
- The widely cited **Punk API** (punkapi.com) is a third-party project built on
  BrewDog's openly published DIY Dog recipe archive. It is not a BrewDog API, it was
  discontinued in 2024, and the domain no longer resolves. It is deliberately not
  catalogued here.
- Regional storefronts `usa.brewdog.com` and `de.brewdog.com` each serve their own
  `/llms.txt`, `/agents.md` and `/.well-known/ucp`.
