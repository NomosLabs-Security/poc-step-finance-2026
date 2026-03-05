# Step Finance Stake Authority Compromise — PoC

> **Educational Purpose Only** — This PoC is created for security research and education
> purposes only. It runs on a local Solana validator, not mainnet.

## Quick Start

```bash
git clone https://github.com/NomosLabs-Security/poc-step-finance-2026
cd poc-step-finance-2026
solana-keygen new --no-bip39-passphrase --silent --force
yarn install
anchor build --no-idl
anchor keys sync
anchor build --no-idl
anchor test --skip-build
```

> **Note:** IDL generation (`anchor build` without `--no-idl`) may fail on newer
> Rust toolchains due to a known `anchor-syn 0.30.1` incompatibility with
> `proc_macro2::Span::source_file()`. The pre-built IDL is included in `idl/`
> and automatically copied to `target/idl/` and `target/types/` when tests run.

## Details

- **Exploit Date:** 2026-01-31
- **Chain:** Solana
- **Framework:** Anchor (Rust + TypeScript)
- **Loss:** ~$27.3M (261,932 SOL)
- **Recovery:** $4.7M (Token22 freeze on Remora Markets assets)
- **Expected Output:** Stake authority transfer, full treasury drain in 4 steps
- **Full Analysis:** [NomosLabs Security Intelligence Archive](https://nomoslabs.io/archive/step-finance-2026)

## Prerequisites

- [Rust](https://rustup.rs/) (v1.79+)
- [Solana CLI](https://docs.solana.com/cli/install-solana-cli-tools) (v1.18+)
- [Anchor](https://www.anchor-lang.com/docs/installation) (v0.30+)
- [Node.js](https://nodejs.org/) (v18+)

## Description

This PoC reproduces the Step Finance stake authority compromise natively on Solana
using the Anchor framework.

The test demonstrates:
1. Treasury with 261,932 SOL staked across 3 stake accounts
2. Executive device compromised — attacker obtains stake authority credentials
3. Stake authority transferred to attacker wallet (no multisig, no timelock)
4. All stake accounts deactivated and SOL withdrawn in ~90 minutes
5. **Fixed version** with multisig + 24h timelock + 50K SOL daily cap + guardian pause

## Program Structure

- `programs/step-exploit/src/lib.rs` — Simplified Solana staking program with vulnerable and fixed paths
- `tests/exploit.ts` — Step-by-step exploit reproduction and mitigation tests

## Key Insight

This was NOT a smart contract vulnerability. All audits, bug bounties, and security reviews
were irrelevant because the attacker compromised executive devices — targeting human
infrastructure rather than protocol code. The authorization model (single EOA as stake
authority) became the attack vector.

## License

MIT — For educational use only.
