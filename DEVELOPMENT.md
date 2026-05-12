# Development

Working notes for building, running, and testing Unhosted from source. This file complements [CONTRIBUTING.md](CONTRIBUTING.md), which covers process; here we cover commands.

## Prerequisites

- Rust toolchain. The version is pinned in [`rust-toolchain.toml`](rust-toolchain.toml); installing rustup and running `cargo` from this directory will use it. MSRV: 1.75.
- A `llama-server` binary from [llama.cpp](https://github.com/ggerganov/llama.cpp), reachable on `http://127.0.0.1:8080`. The daemon proxies inference to it; we do not bundle or supervise it.
- macOS or Linux for the v0.0.x line. Windows GPU support is on the roadmap; see [ROADMAP.md](ROADMAP.md).

## Build

```
cargo build --workspace
```

Individual crates also build in isolation:

```
cargo build -p unhosted-core
cargo build -p unhosted-cli
cargo build -p unhosted-desktop
cargo build -p unhosted-relay
```

## Run

The CLI binary is `unhosted`, produced by the `unhosted-cli` crate.

```
# start a node (listens on 127.0.0.1:7777 by default)
cargo run -p unhosted-cli -- serve

# from a second shell, send a prompt
cargo run -p unhosted-cli -- run "explain the trust radius"
```

Configuration and state live under `~/.config/unhosted/` and `~/.cache/unhosted/`, following the XDG Base Directory spec.

For a guided first run, see [docs/getting-started.md](docs/getting-started.md).

## Test

```
cargo test --workspace
```

The suite runs on macOS and Linux in CI. See [`.github/workflows/rust.yml`](.github/workflows/rust.yml).

## Lint and format

```
cargo fmt --all
cargo clippy --workspace --all-targets -- -D warnings
```

CI rejects any clippy warning or unformatted file. Both commands are idempotent; running them before pushing keeps PRs green.

## Repository layout

| Path | Contents |
|---|---|
| [`crates/unhosted-core`](crates/unhosted-core) | Daemon library: peer routing, mDNS, identity, transport, request proxying |
| [`crates/unhosted-cli`](crates/unhosted-cli) | `unhosted` binary; thin wrapper over `unhosted-core` |
| [`crates/unhosted-desktop`](crates/unhosted-desktop) | Tauri shell pointing at the local daemon |
| [`crates/unhosted-relay`](crates/unhosted-relay) | Rendezvous server for trusted-peer pairing |
| [`design/`](design/) | Numbered architecture decision records (ADRs 0001–0008) |
| [`docs/`](docs/) | GitHub Pages site (HTML/CSS/JS) |
| [`branding/`](branding/), [`assets/`](assets/) | Visual identity |
| [`scripts/`](scripts/) | Build, bundle, and rasterization helpers |

## Where to look next

- [ARCHITECTURE.md](ARCHITECTURE.md) — one-page map of the design decisions
- [docs/getting-started.md](docs/getting-started.md) — first-run walkthrough
- [design/](design/) — eight numbered ADRs that record what was decided and why
- [CHANGELOG.md](CHANGELOG.md) — shipped versions, dated
