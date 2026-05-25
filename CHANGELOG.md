# Changelog

All notable changes to the Libermall marketplace contracts are documented here.
Format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

## [Unreleased]

### Added
- Tier-1 README with badges, architecture diagram, op-codes table, compile instructions (func-js + Blueprint), audit-status callout, security model summary.
- `SECURITY.md` with private disclosure channels, in-scope assets, safe-harbor.
- This `CHANGELOG.md`.

### Roadmap published
- Independent third-party audit
- Tact rewrite
- Royalty-enforcement upgrade for secondary trading
- Test suite under `@ton/sandbox`

## [v1] — 2022 (production)

- Initial mainnet deploy of `marketplace.func` (registry / sale-deployer) + `sale.func` (per-listing).
- Op-codes for NFT transfer, ownership notifications, and gas refunds wired through `definitions.func`.
- 1 TON minimum gas reserve in sale contracts to guarantee worst-case payout chain.
- Live on TON mainnet since 2022, no known exploits to date.
