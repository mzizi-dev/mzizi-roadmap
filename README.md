# Mzizi roadmap — superseded

> **The Mzizi roadmap lives in the language repository, at [`mzizi-dev/mzizi` → `design/ROADMAP.md`](https://github.com/mzizi-dev/mzizi/blob/main/design/ROADMAP.md).** This repository holds no roadmap and never did.

[![Lint](https://github.com/mzizi-dev/mzizi-roadmap/actions/workflows/lint.yml/badge.svg)](https://github.com/mzizi-dev/mzizi-roadmap/actions/workflows/lint.yml)
[![License: Apache 2.0](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](https://www.apache.org/licenses/LICENSE-2.0)

**Status:** superseded, not archived | **Contents:** this README, a licence and the org lint gate | **The roadmap:** [`mzizi/design/ROADMAP.md`](https://github.com/mzizi-dev/mzizi/blob/main/design/ROADMAP.md)

---

## It says archived; GitHub says otherwise

This README previously opened "This repository is archived and read-only."
**Checked against the API on 2026-09-12, `GET /repos/mzizi-dev/mzizi-roadmap`
reports `archived: false`.** The repository is public and writable, and someone
could open a pull request against it today.

That is a small discrepancy with a real cost: a repository named "roadmap" that
looks archived but is not is exactly the second place a roadmap might start
living again, which is what folding it away was meant to prevent.

**The fix is one call**, and it needs someone with admin rights:

```sh
gh api -X PATCH repos/mzizi-dev/mzizi-roadmap -f archived=true
```

Until then, this README describes the repository as **superseded** rather than
archived, because that is what is true. It never held anything but a one-line
README.

## Why it was folded rather than filled

[`mzizi-dev/mzizi/MIGRATION.md`](https://github.com/mzizi-dev/mzizi/blob/main/MIGRATION.md)
§1 gave this repository two possible futures — fold it into `mzizi/design/`, or keep it as a
public GitHub Projects surface — under one constraint:

> …do not leave a roadmap living apart from the code it plans. Three of the stale-doc
> defects fixed on 2026-08-23 existed for exactly that reason.

The same section, a few paragraphs on, already rejects a separate `rfcs` repository on the
grounds that "RFCs live next to the code they govern… Split them and the RFCs become
documentation nobody checks." A roadmap repository is that in different clothes. It would
hold no code, so nothing in it could be checked against anything, and its only possible
content would be restatements of plans that live beside the code they plan — which is the
drift the constraint exists to prevent.

There was also no single roadmap to host. Each plan already sits next to its own code, and
`design/ROADMAP.md` is the index of where:

| The plan                                                  | Where it lives                                                                                                   |
| --------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------- |
| The language and compiler work queue                      | [`mzizi-dev/mzizi` → `MIGRATION.md` §4](https://github.com/mzizi-dev/mzizi/blob/main/MIGRATION.md)               |
| Language design — RFC-0001 to RFC-0004 and RFC-0006       | [`mzizi-dev/mzizi` → `design/`](https://github.com/mzizi-dev/mzizi/tree/main/design)                             |
| Phase 0 benchmark                                         | [`mzizi-dev/mzizi` → `benchmarks/`](https://github.com/mzizi-dev/mzizi/tree/main/benchmarks) and `CHARTER.md` §6 |
| The Rust primitives that are the benchmark's ground truth | [`mzizi-dev/mzizi-registry#222`](https://github.com/mzizi-dev/mzizi-registry/issues/222)                         |
| What a registry is for once the language ships (RFC-0005) | `mzizi-dev/agent-tools#76` — **private repository**, so the link is omitted: it 404s for anyone without access   |

## One thing worth knowing before you read any of it

**Phase 0 has not run.** Nothing in the Mzizi framework has yet been measured against the
charter's kill criterion. What exists is a prototype compiler front end. `design/ROADMAP.md`
says so at the top, with the evidence, and this README repeats it because an archived
repository named "roadmap" is exactly the sort of thing that gets read as a claim of
progress.

## Decided direction and open tracking, as of 2026-10-20

This section points at decisions and open work. Each item says what is built today and
what is decided but not built. Nothing here is a claim that a decision has shipped.

**Release plan.** The staging-to-`main` release plan across the Mzizi repositories is
[issue #17 in this repository](https://github.com/mzizi-dev/mzizi-roadmap/issues/17)
(Waves 1 to 5). The language was released to `main` on 2026-10-20
([`mzizi-dev/mzizi#120`](https://github.com/mzizi-dev/mzizi/pull/120)).

**The four API gateways.** Each has its own repository and host. Live hostnames do not change.

| Gateway                               | Repository                                                                                               | Host                                   | Today                      | Decided, not built                                                                                                                                                          |
| ------------------------------------- | -------------------------------------------------------------------------------------------------------- | -------------------------------------- | -------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Mzizi API (public registry API, free) | [`mzizi-dev/mzizi-api`](https://github.com/mzizi-dev/mzizi-api) (formerly `mzizi-dev/mzizi-api-gateway`) | `api.mzizi.dev`                        | A Hono (TypeScript) Worker | Stays a Hono Worker for now                                                                                                                                                 |
| Nyuchi API (B2B, scoped `nyk_` keys)  | `nyuchi/nyuchi-api` (formerly `nyuchi/api-gateway`, private)                                             | `api.nyuchi.com`                       | FastAPI on Fly             | Built in Rust, including its async-writes data gateway                                                                                                                      |
| Mukoko API (consumer app surfaces)    | `mukoko-dev/mukoko-api` (private, unchanged)                                                             | `api.mukoko.com` (no DNS yet)          | Being built on Workers     | Built in Rust                                                                                                                                                               |
| Nyuchi Servers API (internal only)    | `nyuchi/nyuchi-servers-api` (formerly `nyuchi/data-gateway`, private)                                    | `api.nyuchiservers.com` (no public IP) | Not built                  | Built in Rust; internal-only Fly origin behind a private tunnel; admin, pay, pipeline, api-keys, agents, entitlements, licences, organization, workspaces and `/internal/*` |

Decided, not built: the Nyuchi API (including its async-writes data gateway), the Nyuchi Servers API and the Mukoko API are built in Rust; the Mzizi API stays a Hono Worker for now.

Tracking for the Nyuchi API rebuild is `nyuchi/nyuchi-api#353` (a private repository, so it
is named here and not linked).

**Fly regions (decided, not built).** Fly apps are not Johannesburg-only. Stateless apps (the
gateways' Rust origins and the Nyuchi Servers API) are decided to run
in three regions: `jnb` (Johannesburg), Europe (`cdg` Paris recommended, `mad` Madrid second)
and `sin` (Singapore). The intended coverage is a routing rationale: `jnb` for Southern and East
Africa, `cdg` for North and West Africa and Europe, and `sin` for Asia-Pacific. Many subsea
cables to North and West Africa land in Marseille and backhaul to Paris. Stateful data services
in data-infra (Cassandra, the Postgres ledger, Redpanda, CouchDB, Doris) stay primary in `jnb`
until a replication design exists. No latency figures are claimed for any region.

**Three build targets.** Decided: Mzizi builds for Cloudflare Workers (wasm32), for containers
(a standard OCI image, built with Docker by default and run by any OCI runtime, such as on Fly
machines) and for bare metal (a native Linux binary). The language's RFC-0016
([`mzizi-dev/mzizi#123`](https://github.com/mzizi-dev/mzizi/pull/123)) designs them, and the owner
approved it as design only on 2026-10-21: the charter amendment and every build wave wait for the
Phase 0 gate, so no container or bare-metal build work has started. Every backend
sits behind a trait with an adapter per side, so Cloudflare-hosted stores and Fly-hosted
stores are reachable from either. Built today: every Rust crate builds and tests natively,
and `mz build` emits a native axum service and a native program. There is no container image
and no Workers target yet. The data crate and the MongoDB crate have only a Workers socket
transport; a native transport is designed and waits for the Phase 0 gate.

**Components.** Decided: Mzizi components are Rust, built in-house, not borrowed from another
framework. Mzizi Roots are moving to Mzizi's own `render(props) -> String`
([`mzizi-dev/mzizi-registry#506`](https://github.com/mzizi-dev/mzizi-registry/pull/506) and
[`#510`](https://github.com/mzizi-dev/mzizi-registry/pull/510), merged: the skeleton and six
static primitives). Dioxus is still a dependency of several crates until the migration in the
registry's RFC-roots-renderer finishes. No Dioxus, Leptos or Yew in new work. Svelte is out
everywhere. Astro renders HTML on the server, and static components ship no WASM.

**The consoles.**

- Mzizi console (`app.mzizi.dev`, [`mzizi-dev/mzizi-console`](https://github.com/mzizi-dev/mzizi-console))
  is standalone. Its scope is the registry, components, tokens, architecture and developer
  tooling. Decided ([`mzizi-console#34`](https://github.com/mzizi-dev/mzizi-console/issues/34)):
  one Rust Worker serves Astro's output; sign-in moves to the server (WorkOS, session cookie);
  the data store is Cloudflare D1, not Supabase; it depends on the published Roots crates.
  Today it is Astro pages plus Dioxus islands. The RFC is open as
  [`mzizi-console#35`](https://github.com/mzizi-dev/mzizi-console/pull/35).
- Nyuchi Console (`platform.nyuchi.com`) holds the Astro console app and its Rust Worker
  backend. Billing for services runs through the Nyuchi Console.

**Data.** Decided: MongoDB stays. "Supabase" in the platform means the self-hosted Postgres
ledger on Fly, not a hosted Supabase project. The platform's system of record is the Redpanda
event log. The Mzizi console uses D1. The data crate (`mzizi-data`) is open in pull requests
and not yet merged or wired into any production app.

**Renames.** The owner renamed these repositories (decided 2026-10-20, live on 2026-10-21). Use the
new names:

- `mzizi-dev/mzizi-api-gateway` is `mzizi-dev/mzizi-api`.
- `nyuchi/api-gateway` is `nyuchi/nyuchi-api`.
- `nyuchi/data-gateway` is `nyuchi/nyuchi-servers-api`.
- `mukoko-dev/mukoko-api` is unchanged.

GitHub redirects the old names.

**Cross-repository tracking.**

- [`mzizi-registry#503`](https://github.com/mzizi-dev/mzizi-registry/issues/503): the Astro
  and Rust/WASM integration brief, the registry drift, the data crate and the decisions they
  need.
- [`mzizi-console#34`](https://github.com/mzizi-dev/mzizi-console/issues/34): the console
  rebuild on Astro with a Rust backend and Mzizi Roots.
- `nyuchi/nyuchi-api#353`: the Nyuchi API rebuild (private).

## Licence

Licensed under the [Apache License 2.0](./LICENSE).

Mzizi is an open-architecture project of the **Bundu Foundation**, operated and
developed by **Nyuchi**.
