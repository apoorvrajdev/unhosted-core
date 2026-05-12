# Architecture

This file is a map, not a duplicate of the ADRs. The architecture decision records under [`design/`](design/) are the source of truth; everything below is a short guide to find the right one.

## Shape

Unhosted is a small Rust daemon that proxies inference requests from a local `llama-server` to peer nodes. Each daemon is a peer; there is no central coordinator. The system organizes peers into three concentric trust radii:

- **Local** — devices owned by the same person, paired on a LAN.
- **Trusted** — friends, family, or a team, paired cryptographically with Ed25519 over a relay.
- **Public** — a permissionless swarm of strangers' GPUs, payment-gated.

Each radius has its own discovery mechanism, transport, and authentication model. The ADRs below cover each piece.

## Decision records

| ADR | Subject |
|---|---|
| [0001](design/0001-public-mode-architecture.md) | Public-mode architecture: payment chain, verifiable inference stance, flow shape |
| [0002](design/0002-application-frontends.md) | Application frontends: desktop, mobile, and web surface choices |
| [0003](design/0003-two-node-lan-cluster.md) | LAN cluster: request routing and the peer registry |
| [0004](design/0004-trusted-mode.md) | Trusted mode: Ed25519 pairing and signed requests |
| [0005](design/0005-relay-and-connection-topology.md) | Connection topology: direct, hole-punched, and relayed paths |
| [0006](design/0006-public-mode-onboarding-and-wallet.md) | Public-mode onboarding: account model, wallet binding, first-90-seconds flow |
| [0007](design/0007-security-hardening.md) | Security hardening: local bearer auth, replay defense, rate limits |
| [0008](design/0008-quic-peer-transport.md) | QUIC peer transport: TLS 1.3 with Ed25519-bound certificates |

## Crate boundaries

| Crate | Role |
|---|---|
| `unhosted-core` | Library: daemon, peer routing, mDNS discovery, identity, transport, request proxying |
| `unhosted-cli` | Binary `unhosted`; thin wrapper around `unhosted-core` |
| `unhosted-desktop` | Tauri (`tao` + `wry`) shell pointed at the local daemon |
| `unhosted-relay` | Rendezvous server that helps trusted peers find each other across NATs |

## What is shipped vs designed

[CHANGELOG.md](CHANGELOG.md) tracks shipped versions. The "What's honest" table in [README.md](README.md) maps each capability to a status: `shipped`, `building`, `designed`, or `research`. [ROADMAP.md](ROADMAP.md) gives the ordering. Read those three together.

## Reading order for new contributors

1. [MANIFESTO.md](MANIFESTO.md) — why this project exists.
2. README.md "What's honest" — what works today.
3. [ADR 0003](design/0003-two-node-lan-cluster.md) — the simplest non-trivial topology.
4. [ADR 0004](design/0004-trusted-mode.md) — how identity and pairing work.
5. [ADR 0008](design/0008-quic-peer-transport.md) — the encrypted transport on the wire.

The remaining ADRs are best read when you need them.
