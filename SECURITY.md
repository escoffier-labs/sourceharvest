# Security Policy

## Supported versions

SourceHarvest is pre-1.0. Only the latest minor release on the `master` branch receives security fixes. Pin to a released tag if you need a known-good version.

## Reporting a vulnerability

Please **do not** open a public GitHub issue for security problems. Email **me@solomonneas.dev** with: <!-- content-guard: allow pii/email -->

- A short description of the issue.
- Steps to reproduce (or a minimal proof of concept).
- The version or commit you tested against.
- Whether you would like to be credited in the release notes.

You should get an acknowledgment within 72 hours. If you do not, please follow up - the mail may have been filtered.

## In scope

- Code execution, path traversal, or symlink-attack flaws in any reader (`jsonl`, `json`, `markdown`, `files`, `html`, `gitlog`) or in the export writer.
- A scanner command making a network call. SourceHarvest is local-only by design, so any outbound request from a reader is a bug.
- Output files written outside the requested `--out` target, or written without the atomic, owner-only (`0600`) guarantees the writer is supposed to enforce.
- Harvested text being treated as instructions rather than data anywhere in the pipeline.
- The install script (`install.sh`) fetching, verifying, or executing an asset in a way that an attacker could subvert.

## Out of scope

- Bugs in `content-guard` itself - please report those upstream at
  <https://github.com/escoffier-labs/content-guard>.
- Bugs in MiseLedger or StationTrail - report those to their respective repos.
- Issues that require an attacker to already have write access to the user's machine or to the local files SourceHarvest is asked to read.
- The contents of exported source data you point SourceHarvest at. It normalizes whatever local files you give it; reviewing those files for sensitive content is the operator's responsibility.

## Disclosure

We aim to ship a fix within 14 days of confirming a valid report. A coordinated disclosure timeline can be negotiated for issues that need longer.
