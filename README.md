# BrewDog

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

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
