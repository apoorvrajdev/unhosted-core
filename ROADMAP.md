# Roadmap

This is the public ordering of work. No dates. We ship and then tell you what works.

The status of every line below maps to one of: `shipped`, `building`, `designed`, `research`. Definitions: `shipped` runs from `main` today; `building` has code in tree but is not yet the primary path; `designed` has an ADR but no code; `research` is open-ended.

## Now

- `shipped` — Single-machine inference proxy wrapping llama.cpp `llama-server` (v0.0.1)
- `shipped` — Two-node LAN cluster with round-robin routing and loop prevention (v0.0.2)
- `shipped` — mDNS peer discovery and one-click pairing (v0.0.3)
- `shipped` — `unhosted pull <model>` and `unhosted models` (v0.0.3)
- `shipped` — Embedded web UI on `127.0.0.1:7777` with conversation history (v0.0.3)
- `shipped` — Ed25519 identity, signed peer requests, replay defense
- `shipped` — Relay client with challenge/response registration

## Next

- `building` — QUIC peer transport with TLS 1.3 and Ed25519-bound certificates ([ADR 0008](design/0008-quic-peer-transport.md))
- `designed` — Trusted-peer pairing in the WireGuard style ([ADR 0004](design/0004-trusted-mode.md))
- `designed` — VRAM pooling across machines via llama.cpp RPC backend
- `designed` — Windows GPU support after macOS and Linux are stable

## Later

- `designed` — Tauri desktop app shipping the embedded web UI ([ADR 0002](design/0002-application-frontends.md))
- `designed` — Public swarm MVP on a testnet, then USDC mainnet ([ADR 0001](design/0001-public-mode-architecture.md))
- `designed` — Public-mode onboarding and wallet binding ([ADR 0006](design/0006-public-mode-onboarding-and-wallet.md))
- `research` — Verifiable inference: optimistic plus redundancy first; ZK proofs when affordable

## What we will not do

Listed for honesty.

- No Discord bot, no email gate, no token pre-sale, no SaaS pivot of the open-source code. The reasoning is in [BRAND.md](BRAND.md) under "what we won't do" and in [MANIFESTO.md](MANIFESTO.md).
- No marketing benchmarks. When numbers appear they will land in `benchmarks/` with reproducible scripts.

## Where the timeline lives

[CHANGELOG.md](CHANGELOG.md) records each shipped version with its date.
