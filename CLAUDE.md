# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repository is

A **superseded** placeholder. It holds no roadmap and no code: only `README.md`,
`LICENSE`, lint/format configs and two release workflows. The Mzizi roadmap
lives in [`mzizi-dev/mzizi` → `design/ROADMAP.md`](https://github.com/mzizi-dev/mzizi/blob/main/design/ROADMAP.md),
and each plan sits beside the code it plans (see the table in `README.md`).

Do not add roadmap content, plans or status updates here. The whole point of
folding this repo away (per `mzizi/MIGRATION.md` §1) is that a roadmap must not
live apart from the code it plans. Changes that belong here are limited to the
README's pointer text, lint/format config, and the release workflows.

The README states two facts that should stay accurate if you edit it:

- The repo is **not** actually archived on GitHub (`archived: false`), so it is
  described as "superseded", not "archived". Archiving needs an admin:
  `gh api -X PATCH repos/mzizi-dev/mzizi-roadmap -f archived=true`.
- **Phase 0 has not run** — nothing in Mzizi has been measured against the
  charter's kill criterion. Don't phrase anything as a claim of progress.

## Lint and format

There is no `package.json`, build, or test suite. CI checks are the org's
**required workflows** (defined outside this repo, in the `nyuchi` org), not a
workflow file here. The configs they read are local, and these commands pass
on the current tree:

```sh
prettier --check .                # .prettierrc; *.yml/*.yaml and LICENSE are ignored
npx markdownlint-cli2 "**/*.md"   # .markdownlint.jsonc (MD013/MD033/MD041/MD040 off, MD060 on)
yamllint -s .                     # .yamllint.yaml (140-char lines, `on:` key allowed)
```

Prettier formats Markdown tables, and markdownlint's MD060 expects that aligned
output — run `prettier --write` on a Markdown file after editing a table.
YAML is deliberately excluded from Prettier; yamllint (and the org's actionlint)
own it. `.editorconfig` keeps trailing whitespace in `*.md` (hard line breaks).

## Branches, versioning and releases

Work targets **`staging`**; `main` moves only via a PR from `staging`. Versions
exist **only as git tags** — nothing is committed back to the branch.
Policy is `nyuchi/.github#80`:

- `.github/workflows/staging-version.yml` — every push to `staging` is tagged
  as the next **patch**, via the org's reusable `reusable-staging-release.yml`.
- `.github/workflows/main-release.yml` — every push to `main` is tagged and
  GitHub-released as the next **minor** (`x.y.z → x.(y+1).0`) using the org's
  `next-version` action. It is idempotent: a commit that already carries an
  `x.y.0` tag is skipped. A **major** is only ever cut by hand with
  `workflow_dispatch` and `bump: major`.
- Both use the run's `GITHUB_TOKEN` (not `RELEASE_BUMP_TOKEN`) on purpose, so
  tag pushes trigger no further workflows.

Actions are pinned to full commit SHAs with a trailing `# <ref>, <date>`
comment; keep that form when bumping them.
