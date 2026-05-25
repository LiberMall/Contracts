<div align="center">

# Libermall Marketplace — FunC Contracts

**Open-source FunC smart contracts behind the original Libermall NFT marketplace on TON.**

[![License: MIT](https://img.shields.io/badge/License-MIT-FFD60A.svg?style=flat-square)](LICENSE)
[![Network](https://img.shields.io/badge/Network-TON%20mainnet-0098EA?style=flat-square)](https://ton.org/)
[![Language](https://img.shields.io/badge/Language-FunC-1E90FF?style=flat-square)](https://docs.ton.org/develop/func/overview)
[![Live since](https://img.shields.io/badge/Live%20since-2022-22C55E?style=flat-square)](https://libermall.com)
[![Stars](https://img.shields.io/github/stars/LiberMall/Contracts?style=flat-square&label=★)](https://github.com/LiberMall/Contracts/stargazers)

[**Libermall**](https://libermall.com) · [**Libermall ID**](https://id.libermall.com) · [**TNT NFT tool**](https://github.com/LiberMall/tnt) · [**Marketplace front-end**](https://github.com/LiberMall/TON-Marketplace-NFT-Web-Frontend)

</div>

---

## What this is

The on-chain side of the original Libermall NFT marketplace — two FunC contracts that have been live on TON mainnet since 2022:

- **`marketplace.func`** — the registry / deployer. Holds the marketplace's owner address, an authorization pubkey, the embedded sale contract code, and the per-deploy / per-transfer fee table. Every new listing spawns a sale contract.
- **`sale.func`** — one instance per active listing. Stores marketplace address, NFT contract address, owner, full price, and the fee split (marketplace + royalty). Atomic buy / cancel.

## Architecture

```
                    ┌──────────────────────────────────┐
                    │   Libermall marketplace contract  │
                    │   ──────────────────────────────  │
                    │   owner / auth pubkey             │
                    │   sale_code  ◄── code cell        │
                    │   deploy_amount + transfer_amount │
                    └──────────────────┬───────────────┘
                                       │ ① deploy a new sale
                                       ▼
                    ┌──────────────────────────────────┐
                    │   sale contract (one per listing) │
                    │   ──────────────────────────────  │
                    │   marketplace_address             │
                    │   nft_address                     │
                    │   nft_owner_address               │
                    │   full_price                      │
                    │   fees_cell                       │
                    └──┬───────────────────────────────┘
                       │ ② buy / cancel
                       ▼
              ┌────────────────────────────┐
              │   Royalty + payouts         │
              │   ─────────────────────     │
              │   marketplace fee  ──►  marketplace owner
              │   royalty          ──►  royalty destination
              │   remainder        ──►  NFT seller
              └────────────────────────────┘
```

## Repository layout

```
Contracts/
├── README.md           ← you are here
├── LICENSE             ← MIT
├── SECURITY.md
├── CHANGELOG.md
├── marketplace.func    ← registry / sale-deployer
├── sale.func           ← per-listing sale contract
├── definitions.func    ← shared op-codes and helper assembly snippets
└── stdlib.func         ← TON FunC stdlib snapshot used at compile time
```

## Op-codes

| Op | Hex | Where | What it does |
|---|---|---|---|
| `transfer` | `0x5fcc3d14` | NFT item | Transfer ownership of an NFT item |
| `ownership_assigned` | `0x05138d91` | NFT item | Notification sent when ownership is transferred |
| `excesses` | `0xd53276db` | Common | Refund excess gas to the original sender |

Full set is in [`definitions.func`](definitions.func). The marketplace itself accepts custom internal messages — read the relevant `recv_internal` block in [`marketplace.func`](marketplace.func) for the exact body format.

## Compile

This repository ships raw FunC. Compile it with any of the standard tooling:

### Using [func-js](https://github.com/ton-community/func-js)

```bash
npm install -g @ton-community/func-js
funcjs marketplace.func stdlib.func definitions.func > marketplace.fif
funcjs sale.func stdlib.func definitions.func > sale.fif
```

### Using [Blueprint](https://github.com/ton-community/blueprint)

```bash
mkdir libermall-marketplace && cd libermall-marketplace
npx create-ton@latest .
# copy marketplace.func, sale.func, definitions.func into contracts/
# write a TS wrapper, then:
npx blueprint build
```

## Audit status

> 🔍 **These contracts have NOT undergone a paid third-party audit.** They have been running in production on TON mainnet since 2022 with no known exploits, but please do your own due diligence before forking them for production use.
>
> If you operate a service that depends on these contracts and want to fund an independent audit, [reach out](mailto:hello@libermall.com).

## Security

- The marketplace contract holds **no funds** between sale operations — every payment flows through a sale contract and is split atomically inside `recv_internal`.
- Sale contracts cannot be re-used. Once a sale completes or is cancelled, the contract is destroyed and any remaining gas is refunded.
- The authorization pubkey in the marketplace contract is a critical key — compromise allows deployment of malicious sale contracts. Treat it the same as a wallet seed.
- We use `min_gas_amount() = 1 TON` in `sale.func` to ensure room for the worst-case payout chain.

To report a vulnerability privately, see [`SECURITY.md`](SECURITY.md) or email [`security@libermall.com`](mailto:security@libermall.com).

## Used by

- [Libermall NFT Marketplace](https://libermall.com) (production since 2022)
- Reference reads / deploys: [`tnt`](https://github.com/LiberMall/tnt), [`TON-Marketplace-NFT-Web-Frontend`](https://github.com/LiberMall/TON-Marketplace-NFT-Web-Frontend)

## Roadmap

- [x] v1 — marketplace + sale contracts live since 2022
- [ ] Independent third-party audit
- [ ] **Tact rewrite** for better tooling / type safety / smaller binaries
- [ ] Royalty-enforcement upgrade for secondary trading on partner surfaces
- [ ] Test suite under [`@ton/sandbox`](https://github.com/ton-org/sandbox)

## License

[MIT](LICENSE) © 2022 Libermall. The contracts themselves are MIT; the `Libermall` wordmark and M-shield logo are trademarks, see the brand guidelines in [`LiberMall/libermall-id-landing`](https://github.com/LiberMall/libermall-id-landing/blob/main/BRANDBOOK.md).

---

<div align="center">

**Part of the [Libermall ecosystem](https://libermall.com).**

[Identity](https://id.libermall.com) · [DEX](https://dex.libermall.com) · [Pay](https://pay.libermall.com) · [Card](https://card.libermall.com) · [Reviews](https://sites.reviews) · [TonChat AI](https://tonchat.ai) · [TON.CEO](https://ton.ceo)

</div>
