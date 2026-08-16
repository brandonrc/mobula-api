# mobula-api

**The OpenAPI contract for [Mobula](https://github.com/brandonrc/mobula), plus
generated SDKs. Generated — do not hand-edit.**

The Rust code in the `mobula` repo is the source of truth. Its CI generates
the spec and **pushes it here**; this repo lints it and regenerates/publishes
the client SDKs. Editing the spec by hand is pointless — the next push
overwrites it.

## Layout

```
openapi.json         # the spec (authoritative artifact, pushed from mobula)
openapi.yaml         # same spec, YAML — for tools that prefer it
.spectral.yaml       # Spectral lint ruleset
redocly.yaml         # Redocly lint/docs config
sdk/
  typescript/        # config.yaml + templates/  (openapi-generator: typescript-fetch)
  rust/              # config.yaml + templates/  (openapi-generator: rust/reqwest)
  python/            # config.yaml + templates/  (openapi-generator: python)
```

Each `sdk/<lang>/` is **config-only**: openapi-generator emits the whole
package (including package.json/Cargo.toml/pyproject) from `config.yaml`;
drop `.mustache` files in `templates/` to override specific generated files.
Nothing generated is committed.

More SDK languages slot in under `sdk/<lang>/` (the pattern this mirrors,
[artifact-keeper-api](https://github.com/artifact-keeper/artifact-keeper-api),
ships TypeScript/Python/Kotlin/Swift the same way).

## Pipelines

- **`validate.yml`** — Spectral + Redocly lint the spec on every push/PR.
- **`generate.yml`** — on spec/SDK change (or a tag), regenerate the SDK and
  publish `@brandonrc/mobula-client` to the GitHub Packages npm registry.

```
mobula (Rust, source of truth)
   │  CI: generate openapi.json, push here
   ▼
mobula-api (this repo)  ──validate──▶  ──generate──▶  @brandonrc/mobula-client → npm → mobula-ui
```

## Notes

- Generation: **openapi-generator** (Mustache templates), one config per
  language under `sdk/<lang>/`. TS publishes to GitHub Packages now; Rust and
  Python build in CI and publish only when `CARGO_REGISTRY_TOKEN` /
  `PYPI_API_TOKEN` secrets are set.
- Versioning is loose during dev: the SDK publishes `0.1.<run>` and consumers
  track `latest`. Aligning the SDK version to a real API version is later work.
