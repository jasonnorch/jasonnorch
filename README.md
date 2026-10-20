# Jason Norch

Backend engineer. 26 years shipping and operating production systems, Go exclusively for
the last few. Before that, a decade of Python building real-time security data pipelines
on PostgreSQL and Redis, including a 24/7 intelligence collection platform I was the sole
architect and operator of.

I like systems I can hold in my head: standard library first, interfaces at the consumer,
one place for every opinion. I would rather own a thing end to end, from the design through
deployment and the years of maintenance after, than hand off at any one of those stages.

**Contact:** job@norch.com · North Canton, Ohio

## Projects

### [Intelligence API](https://github.com/Security-Accelerated/intelligence-api) · Go, PostgreSQL, Redis, Stripe

A commercial domain and IP intelligence REST API, live at
[api.securityaccelerated.com/docs](https://api.securityaccelerated.com/docs). One request
fans out to concurrent lookups (WHOIS, DNS, TLS, email security, certificate transparency,
BGP, threat intelligence, GeoIP, and more) and returns one JSON document with an optional
aggregate risk score.

The parts I think are worth reading:

- **Per-lookup caching.** Each lookup has its own TTL and reports its own `cache_hit`, so a
  single response can mix cached and fresh data honestly. Failures are never cached.
- **Partial-failure contract.** A sub-lookup that fails returns 200 with the reason in a
  per-result `errors` array. The caller gets everything that worked.
- **Self-service keys.** Customers rotate their own keys with a 24-hour grace window and
  query their own usage. Billing is Stripe, provisioning is webhook-driven.
- **SSRF prevention at dial time.** Every outbound client goes through one package that
  refuses private and link-local addresses when the connection is made, not when the URL
  is parsed.
- **Operations.** Fly.io, Neon, Upstash. Tag-driven deploys gated on manual approval.
  Pushover alerting with throttling and spike detection. An operator CLI for customer and
  key management.

### [go-service-starter](https://github.com/jasonnorch/go-service-starter) · Go

The foundation I start new Go services from, extracted from the above and from client
work. Built on `net/http` and `ServeMux` with no framework: typed config, `slog`, graceful
shutdown, health checks, one JSON error envelope for every failure including 404 and 405,
and a middleware chain that composes with anything taking `func(http.Handler) http.Handler`.

Each dependency lives in exactly one package so it can be swapped by editing one file.
Postgres, Redis, and API-key auth are optional modules that `setup.sh` removes cleanly.
`make verify` passes on a fresh clone with nothing running and no config.

### AI oversight agent for GLP toxicology · Go, Anthropic API, Azure AD, React

Contract work for a preclinical biotech, private. A medical officer cross-checks study
documents (protocols, pathology narratives, CRO minutes) for deviations before the final
report is locked. I replaced a browser-only prototype with a single Go binary that embeds
the React build and holds every credential, prompt, and piece of compliance logic
server-side. Includes an append-only 21 CFR Part 11 audit trail with scheduled export to a
WORM folder and SSO-scoped document access.

## Previously

**Binary Defense Systems, 2015 to 2025.** Senior Security Operations Developer. Sole
architect and operator of the counterintelligence collection and analysis platform: a dark
web crawler processing about a million new pages a day, real-time ingestion from social
and paste sources, and a rules engine evaluating 1.5 million events a day into analyst
tickets. Running it unattended removed the need for a planned off-hours watch floor.

Before that, intranet and field-audit systems for an energy company, and a run of .NET
work going back to 2000.

Full resume on request.
