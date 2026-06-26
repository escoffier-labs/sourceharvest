# Contributing to SourceHarvest

SourceHarvest is a local-first Go CLI that exports local source-system records to `miseledger.adapter.v1` JSONL. It is the non-agent sibling of [StationTrail](https://github.com/escoffier-labs/stationtrail), feeding the same [MiseLedger](https://github.com/escoffier-labs/miseledger) evidence store. Patches are welcome. Before you start, please skim this file so we both spend our time on the right things.

## What kinds of changes land easily

- **Bug fixes** in any reader (`jsonl`, `json`, `markdown`, `files`, `html`, `gitlog`) or in the export writer.
- **New local readers** that map a real local input shape onto `miseledger.adapter.v1` records, built from a real local schema or a redacted sample export.
- **Sharper normalization**: better actor, artifact, link, or raw-reference extraction from inputs we already support.
- **Test coverage** for any of the above, with fixtures under `testdata/`.
- **Docs fixes** that keep the README (Readers and Usage sections) and `docs/ROADMAP.md` in sync with behavior.

## What needs a conversation first

- **A new top-level subcommand or a change to an existing flag's meaning.** Open an issue first describing the user story. These are the public surface and renaming them later is painful.
- **Changes to the `miseledger.adapter.v1` record shape.** The schema is a contract with MiseLedger and StationTrail. Coordinate before changing it.
- **Anything that adds a runtime dependency.** SourceHarvest is stdlib-only on purpose (the Markdown front-matter parser, for example, is hand-rolled rather than pulling in a YAML library), and we want to keep it that way.

## What does not land

- **Storage, search, or serving.** SourceHarvest only exports JSONL. Archive, SQLite, search, evidence-bundle, GUI, and server behavior belong in MiseLedger.
- **Live crawling.** Scanner commands read local files only and must not make network calls. Never shell out to the live crawlers (`discrawl`, `gitcrawl`, `graincrawl`, `notcrawl`, `slacrawl`, `telecrawl`); consume their already-exported local files instead.
- **Treating harvested text as instructions.** Exported text is untrusted evidence. Normalize it and pass it through as data.
- Personal details, hostnames, IPs, account IDs, raw transcripts, credentials, or local evidence in code, tests, or fixtures. Use `192.0.2.x` for example IPs. The `content-guard` check will fail otherwise.
- Cron jobs or hooks that post or call out to the network without explicit opt-in.
- AI co-authorship trailers on commits (`Co-Authored-By: <model>`). Conventional commits only.

## Local dev

```bash
git clone https://github.com/escoffier-labs/sourceharvest.git
cd sourceharvest
go build -o bin/sourceharvest ./cmd/sourceharvest
go test ./...
```

The entry point is `cmd/sourceharvest/main.go`. CLI and readers live in `internal/app`; the record schema lives in `internal/adapter`. Fixtures live in `testdata/`.

## Definition of done

Run the full gate, which mirrors CI (`.github/workflows/ci.yml`):

```bash
./scripts/verify
```

It runs `go test ./...`, `go vet ./...`, `govulncheck`, and `go build`. Run it before reporting any code change complete. Paste failures verbatim and never claim a result you did not observe.

- Never weaken a failing gate to reach green. Do not skip, delete, or loosen failing tests, and do not bypass `go vet` or `govulncheck`. Fix the code, or report the failure and stop.
- Never push with `--no-verify`. If a pre-push hook blocks you, paste its output exactly.
- Never invent commands or flags. Confirm against `./bin/sourceharvest --help` or `internal/app/app.go` first.

## Adding a reader

A reader turns one local input shape into `miseledger.adapter.v1` records. To add one:

1. Implement the reader in `internal/app`, following the existing readers' structure.
2. Wire the subcommand into the CLI in `internal/app/app.go`.
3. Add a fixture under `testdata/` (real local schema or redacted sample only) and a test in `internal/app`.
4. Add a row to the Readers table and a Usage example in `README.md`, and update `docs/ROADMAP.md`.

## Filing issues

Please use the templates under `.github/ISSUE_TEMPLATE/`. They exist to save you from re-typing the version, reader, and OS every time. Before posting output, remove tokens, private hostnames, private repo names, and unredacted absolute paths.

## License

By contributing you agree that your contribution is licensed under the MIT License, same as the rest of the repo.
