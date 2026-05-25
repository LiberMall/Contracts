# Security Policy

These FunC contracts have run on TON mainnet without known exploits since 2022. They have **not** undergone a paid third-party audit. Treat that gap as your call to make before forking.

## Reporting a vulnerability

If you discover a security issue **do not open a public GitHub issue**. Use one of these private channels:

| Channel | Use it for |
|---|---|
| **Email**: [`security@libermall.com`](mailto:security@libermall.com) | Most reports. PGP key on request. |
| **GitHub Security Advisory** | Private coordinated disclosure via the *Security* tab. |
| **Telegram** to [@LibermallIDbot](https://t.me/LibermallIDbot) → `/security` | Quick disclosure with screenshots. |

We acknowledge reports within **48 hours**, triage within **5 business days**, and aim to ship a fix within **30 days** for high-severity issues.

For critical findings affecting funds at risk, we may offer a bug bounty on a case-by-case basis — disclose first, before discussing terms.

## In-scope assets

- The contract source in this repository
- The deployed marketplace contract on TON mainnet that backs [libermall.com](https://libermall.com)
- The per-listing sale contracts spawned by that marketplace

Out of scope:

- Front-end vulnerabilities — report to [`TON-Marketplace-NFT-Web-Frontend`](https://github.com/LiberMall/TON-Marketplace-NFT-Web-Frontend) or the active web at [libermall.com](https://libermall.com)
- Forks of these contracts under other accounts — report to fork owners directly
- Issues that depend on a compromised authorization key (the auth pubkey is a critical secret; if it leaks, that is itself the issue)

## Safe-harbor

We won't pursue legal action against researchers who:

1. Make good-faith effort to avoid privacy violations and service degradation.
2. Don't exfiltrate data beyond what's needed to prove the issue.
3. Give us reasonable time to remediate before public disclosure (typically 90 days).
4. Don't exploit the issue for personal gain.

## Hall of fame

Researchers who report valid vulnerabilities will be credited (with consent) in [`CHANGELOG.md`](CHANGELOG.md) and on [`id.libermall.com/security.html`](https://id.libermall.com/security.html).
