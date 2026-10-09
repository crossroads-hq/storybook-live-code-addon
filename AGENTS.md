# AGENTS.md

Guidance for coding agents working in `storybook-live-code-addon`, which
publishes the public npm package `storybook-live-code`: live code blocks for
Storybook Docs. It is a fork-derived prototype of the MIT-licensed
`JeremyRH/storybook-addon-code-editor` (see the README's "Fork Status").
`npm test` runs the suite; `.github/workflows/publish.yml` is the only release
path.

## Code Review Rules

A test settles a finding only if it covers that exact issue and passed on the
current PR head. A missing, skipped or stale result proves nothing.

### Public contract
- Consumers import `.` and `./styles.css` (`exports` in `package.json`) and use
  the documented props of `LiveCodeBlock`, `LiveCodeDocsBlock` and
  `LiveCodePreview`. Flag a change that removes or changes an export or a
  documented prop without a `CHANGELOG.md` entry naming it, and a change to
  `exports` or `files` that drops something a consumer imports.

### Release
- `.github/workflows/publish.yml` publishes through npm trusted publishing with
  provenance, and refuses a tag that does not match `package.json`. Flag adding
  an npm token, dropping `--provenance`, `id-token: write` or the tag-version
  guard, a dependency cache in the publish job, and a second publish path (the
  retired `release.yml` was one).

### Attribution
- Flag a change that removes the upstream project's attribution from the
  README's "Fork Status", or an upstream copyright notice from `LICENSE`, which
  ships in the package.

### Public repository
- This repository is public and takes fork pull requests. Flag self-hosted
  runner labels, `pull_request_target` that checks out pull-request code, and
  any secret reachable from a fork pull request.

### Merge gate
- `PR Validation` is the required check. Flag renaming it, and removing a
  mandatory job from its `needs` or from the results it checks.
- Flag `continue-on-error` on mandatory work, any condition other than the
  gate's `!cancelled()` that can skip the gate (GitHub treats a skipped
  required check as passing), and a change that lets the gate accept a missing,
  malformed or unauthorized skipped result. Keep the conditions that let the
  gate run and fail when a job it needs has failed.
- Do not claim a check ran unless its result is present on the pull request.
