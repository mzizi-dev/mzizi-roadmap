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

## Licence

Licensed under the [Apache License 2.0](./LICENSE).

Mzizi is an open-architecture project of the **Bundu Foundation**, operated and
developed by **Nyuchi**.
