# Maigret

Collect a dossier on a person from a username — across thousands of public sites.

> **Upstream / attribution:** This repository is a copy of [soxoj/maigret](https://github.com/soxoj/maigret) by Soxoj and contributors (MIT © 2020–2026 Soxoj). Local commits here add CI, dependency, and security maintenance; use the upstream project for releases and issue reports. [简体中文说明](README.zh-CN.md).

Maigret searches a username across a large database of public websites, then follows discovered identifiers to build a connected profile. Normal searches need no site API keys, and results can be exported as reports or graph data.

## Features

- **Large site database** — 3,000+ site entries listed in [sites.md](sites.md); a default run checks the highest-ranked sites, `-a` checks all of them, and `--tags` narrows by category or country.
- **Recursive discovery** — usernames, IDs, and links found on one profile seed further searches (`--parse`, `--permute`).
- **Rich exports** — HTML, optional PDF (`maigret[pdf]` extra), XMind, JSON (simple/NDJSON), CSV, TXT, an interactive D3 graph (`--graph`), and Neo4j Cypher scripts (`--neo4j`).
- **Web UI** — `maigret --web 5000` opens a browser interface with a results graph and report downloads.
- **Network options** — HTTP/SOCKS proxy, Tor and I2P gateways, and an opt-in, experimental FlareSolverr integration for JavaScript challenges.
- **Optional AI summary** — `--ai` sends collected findings to an OpenAI-compatible endpoint for a short investigation summary; core searches do not need an API key.
- **Embeddable** — the CLI wraps an async Python API you can call from your own tooling.

## Quick start

Python 3.11.5 or newer:

```sh
pip3 install maigret
maigret username
```

From this checkout:

```sh
git clone https://github.com/anya12forger12-max/maigret.git
cd maigret
pip3 install .
maigret username
```

The local `Dockerfile` defines a default `cli` target and a `web` target:

```sh
docker build -t maigret .
docker build --target web -t maigret-web .
docker run -v "$PWD/reports:/app/reports" maigret username --html
```

Upstream also publishes `soxoj/maigret:latest` (CLI) and `soxoj/maigret:web` (browser UI) images.

## Usage

```sh
maigret user --html                 # HTML report
maigret user --json ndjson          # machine-readable export
maigret user --csv
maigret user --graph                # interactive D3 graph
maigret user --neo4j                # Neo4j Cypher import script
maigret user --tags photo,dating    # limit to tagged sites
maigret user1 user2 -a              # check every site in the database
maigret --web 5000                  # browser UI
```

Useful extras: `--parse URL` seeds a recursive search from a profile page, `--permute` generates name variants, `--keywords` highlights pages mentioning terms, and `--ai` produces an assisted summary (set `OPENAI_API_KEY` in your environment or `openai_api_key` in local `settings.json`). Run `maigret --help` for all options, see [TROUBLESHOOTING.md](TROUBLESHOOTING.md) for blocks and timeouts, and consult `docs/source/` for the full documentation set.

## Project layout

| Path | Purpose |
| :--- | :--- |
| `maigret/` | Python package: checking, site database, reporting, web UI, AI integration |
| `maigret/resources/data.json` | Bundled site database and settings |
| `sites.md` | Human-readable site list and tags |
| `docs/source/` | Documentation sources |
| `tests/` | Test suite |
| `utils/` | Site database and release helper scripts |

## Ethics and responsible use

Maigret is an OSINT tool. Use it only for lawful, legitimate investigations, respect privacy and applicable data-protection rules (such as GDPR and CCPA), and do not use results for harassment, doxxing, or impersonation. Verify findings independently — a username match is not proof of identity. Keep API keys in your environment or local settings, never in shared reports or documentation. The upstream authors disclaim responsibility for misuse.

## License and credits

MIT License — Copyright (c) 2020–2026 Soxoj. See [LICENSE](LICENSE). Release history is in [CHANGELOG.md](CHANGELOG.md). Credit for the project belongs to Soxoj and its contributors; this repository is a copy, not the canonical upstream.

## Quick start — 1 or 2 commands

```bash
./setup.sh          # install dependencies
./setup.sh run      # start the project (when a run target exists)
```

`setup.sh` detects this repository's build system and installs the right toolchain
dependencies. Use `./setup.sh test` to run the available checks. If a project has no
standard run target, the script prints the correct command from the sections above.
