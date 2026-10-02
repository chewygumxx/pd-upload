---
__cgxx: |
  # vim:set expandtab shiftwidth=2 filetype=markdown foldlevel=3:
  # SPDX-License-Identifier: GPL-3.0-only

  #
  #
  # ~chewygumxx/pd-upload.git
  # ::: :/README.md
  #
  #

ctime: 2026-10-02
title: "pd-upload"
description: >-
  Learning how to upload to Proton Drive using their official client sdk
tags:
  - learn
---

# pd-upload

Learning how to upload to Proton Drive using their official client sdk

## CI

`.github/workflows/ci.yaml` calls the shared
[standard workflow](https://github.com/chewygumxx/.github#standard-workflow):
commitlint, the header sync, generic lint and format checks for workflows,
shell and zsh scripts, TOML, YAML and `.editorconfig`, and the metadata sync.
This repository's own `bun run check` follows, against the commit the header
sync pushed.

## Development

- `bun run commit` composes a commit interactively.
- `bun run test` runs `src/**/*.test.ts` with `bun test`. Bun strips the
  types itself, so `tsc` only typechecks, with `bun run typecheck`.
- `bun run check` runs the checks CI runs: the typecheck, the build, the
  tests, Biome's format and lint checks, Markdown lint, the YAML checks
  (prettier, then yamllint with `@chewygumxx/yamllint-config`) and a check
  that rejects em dashes.
- `bun run format` applies Biome formatting, and prettier's to YAML, which Biome
  does not read.

The pre-commit hook runs the same checks on staged files. The commit-msg hook
runs commitlint.

## Publishing

`bun run build` compiles `src/` to `dist/` with its declarations, and
`prepack` runs it, so a tarball always holds a fresh build.

To release, bump `version` in `package.json` (`bun.lock` does not record it),
commit, and push a matching `v*` tag. `.github/workflows/publish.yaml` runs
the check, compares the tag with `version`, then stages the version with
`npm stage publish`; approve it on npmjs.com to publish it. That one step
still uses npm, as staged and trusted publishing are npm CLI features.

Before the first release, add a Trusted Publisher in the package's npm
settings that names this repository and the `publish.yaml` workflow with the
stage publish permission. No `NPM_TOKEN` secret exists or is needed.
