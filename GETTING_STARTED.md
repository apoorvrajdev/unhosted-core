# Getting started

A first-run walkthrough that works today. The aspirational `curl | sh` flow in the README's Quickstart describes the day-one shipped product; this file describes the from-source path that exists now.

## 1. Install prerequisites

- Rust toolchain (rustup will pick up the pinned version from [`rust-toolchain.toml`](rust-toolchain.toml))
- A `llama-server` binary from [llama.cpp](https://github.com/ggerganov/llama.cpp), built for your platform
- A GGUF model file. If you do not have one yet, any small instruction-tuned model from Hugging Face will do (Qwen 2.5 0.5B is a reasonable smoke test)

## 2. Start `llama-server`

In one shell, start `llama-server` on its default port:

```
llama-server -m /path/to/your-model.gguf --port 8080
```

Leave it running.

## 3. Build Unhosted

In a second shell, from the repository root:

```
cargo build --workspace
```

## 4. Run a node

```
cargo run -p unhosted-cli -- serve
```

The daemon listens on `127.0.0.1:7777`. The embedded web UI is at <http://127.0.0.1:7777/>.

## 5. Send a request

From a third shell:

```
cargo run -p unhosted-cli -- run "explain the trust radius in one paragraph"
```

Tokens stream to stdout as the upstream model produces them. The response carries an `X-Unhosted-Served-By` header so you can see which node handled the request; on a single-node setup this will read `local`.

## 6. Add a second node

To exercise the routing path, start a second daemon on a different port (and either a second `llama-server`, or the same one — the daemon proxies, it does not pin to a backend):

```
UNHOSTED_PORT=7778 cargo run -p unhosted-cli -- serve
```

Then register it as a peer of the first node:

```
cargo run -p unhosted-cli -- peer add peerB 127.0.0.1:7778
cargo run -p unhosted-cli -- peer list
```

Subsequent `run` requests on the first node round-robin between `local` and `peer:peerB`. The behaviour and verification log are described in [`design/0003-two-node-lan-cluster.md`](design/0003-two-node-lan-cluster.md).

## 7. Discover peers automatically

When two daemons are on the same LAN they announce themselves over mDNS as `_unhosted._tcp.local.` and appear in each other's discovered-peer list. The web UI surfaces them with a one-click pair button. The mechanics are in [`design/0004-trusted-mode.md`](design/0004-trusted-mode.md).

## What does not work yet

- VRAM pooling across machines (layer splitting). The daemon routes requests; it does not yet split a single model across hosts.
- The public swarm and any payment flow.
- Windows GPU support.
- The full Tauri desktop app — the shell crate compiles but the production UI is future work.

See [ROADMAP.md](ROADMAP.md) for ordering and [CHANGELOG.md](CHANGELOG.md) for what has shipped per version.
