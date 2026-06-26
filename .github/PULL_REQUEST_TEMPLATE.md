<!--
Thanks for sending a patch. Keep this short; delete sections that do not apply.
See CONTRIBUTING.md for what lands easily and what needs an issue first.
-->

## What and why

<!-- One or two sentences on the user-visible change and the problem it solves. -->

Closes #

## Type of change

- [ ] Bug fix
- [ ] New reader
- [ ] Docs
- [ ] Refactor with no command-surface change
- [ ] Surface change (new subcommand, flag meaning, or adapter schema change). Opened an issue first per CONTRIBUTING.md

## Checklist

- [ ] `./scripts/verify` passes locally (tests, `go vet`, `govulncheck`, build)
- [ ] Added or updated tests and fixtures covering the change
- [ ] Updated the README (Readers and Usage) and `docs/ROADMAP.md` if behavior changed
- [ ] Updated the `Unreleased` section of `CHANGELOG.md` for any user-visible effect (entries describe effects, not commit subjects)
- [ ] No new runtime dependencies (SourceHarvest is stdlib-only on purpose)
- [ ] Scanner commands still read local files only and make no network calls
- [ ] No personal details, hostnames, real private IPs, account names, tokens, raw transcripts, or unredacted absolute paths in code, tests, fixtures, or this PR (the `content-guard` check will fail otherwise)
- [ ] Conventional commit messages, no AI co-authorship trailers
