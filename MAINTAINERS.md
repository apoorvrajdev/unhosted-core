# Maintainers

Unhosted is a small project with a deliberate division of work between two maintainers. This file is the source of truth for who owns what.

## Ankur Sinha — original author and lead

GitHub: [@sinhaankur](https://github.com/sinhaankur)

- Core architecture and Rust implementation across the `unhosted-core`, `unhosted-cli`, `unhosted-desktop`, and `unhosted-relay` crates
- Peer routing, mDNS discovery, Ed25519 identity, QUIC transport, relay client
- [MANIFESTO.md](MANIFESTO.md), [BRAND.md](BRAND.md), and the eight numbered ADRs under [`design/`](design/)
- Release engineering and CI

## Apoorv Raj — infrastructure, documentation, developer experience

GitHub: [@apoorvrajdev](https://github.com/apoorvrajdev)

- Repository structure and presentation on this fork
- Developer onboarding: [DEVELOPMENT.md](DEVELOPMENT.md), [docs/getting-started.md](docs/getting-started.md)
- Documentation architecture: [ARCHITECTURE.md](ARCHITECTURE.md) as a map into the ADRs, [ROADMAP.md](ROADMAP.md) extracted from the README
- Forward work: distributed-systems contributions, networking experiments, additional test coverage

## Contributing

Outside contributions follow [CONTRIBUTING.md](CONTRIBUTING.md). The project is licensed under [AGPL-3.0](LICENSE); contributions must comply.

## Contact

Security issues go through the channel in [SECURITY.md](SECURITY.md). Everything else: open an issue.
