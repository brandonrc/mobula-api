# mobula-api

**Generated. Do not hand-edit.** This repository is the machine-generated
API surface of [Mobula](https://github.com/brandonrc/mobula):

- [`openapi.json`](openapi.json) — the OpenAPI spec, generated from mobula's
  Rust code and **pushed here by mobula's CI** on every change to the API.
  Editing it by hand is pointless; the next push overwrites it.
- [`typescript/`](typescript) — the generator for `@brandonrc/mobula-client`.
  On any change to `openapi.json`, this repo's pipeline regenerates the typed
  client and publishes it to the GitHub Packages npm registry.

Consumers (e.g. [mobula-ui](https://github.com/brandonrc/mobula-ui)) depend on
the published `@brandonrc/mobula-client` package — never on a running server,
never on hand-written types.

```
mobula (Rust, source of truth)
   │  CI: generate openapi.json, push here
   ▼
mobula-api (this repo)
   │  CI: regenerate client from openapi.json, publish
   ▼
@brandonrc/mobula-client  →  GitHub Packages npm  →  mobula-ui
```

Versioning is intentionally loose during dev: the client publishes
`0.0.<run>` and consumers track `latest`. Aligning the client version to a
real API version is a later concern.
