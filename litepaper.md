---
title: EverArcade & EverVault
subtitle: RWA-Backed Gaming + Compliant QOZB Vaults on Evernode/Xahau
author: EverArcade Team
date: December 2025
geometry: margin=1in
fontsize: 11pt
header-includes: |
  \usepackage{hyperref}
  \usepackage{geometry}
  \usepackage{tcolorbox}
  \usepackage{sectsty}
  \sectionfont{\Large\bfseries}
---

# The Flywheel: Arcade Retention → Tax-Advantaged Vault Equity

EverArcade is the first production-ready, modular DeFi primitive on Evernode/Xahau: A play-to-own arcade (HotPocket dApps) that funnels users into compliant QOZB vaults (Denis Angell smart issuers + 10-year escrows). 

**Business Model**: 
- **Arcade**: On-chain games (JS/Rust .hp bundles) with XAH pathfinding payments (any token → XAH). 1% tx fees → retention loop (NFT rewards → leaderboards via Xahau hooks).
- **Vaults**: Tokenized QOZ funds (Brooklyn/Austin real estate) – ZK-gated accreditation → fractional MPT mints via blackhole issuer. 0.5% management fee → $1M ARR at 10k DAUs.
- **Acquisition Bait**: Modular (arcade/vault separation) for carve-out exits (Ripple/Animoca dipping into XRPL RWAs). Tax-advantaged (SEC-friendly via ZK proofs/oracles).

| Metric | Projection (Q1 2026 Launch) | Exit Multiple |
|--------|-----------------------------|---------------|
| DAUs   | 10k (arcade retention 40%)  | 10x ARR      |
| TVL    | $5M (QOZB equity mints)     | BlackRock pivot |
| Fees   | $1M ARR (1% arcade + 0.5% vaults) | $100M valuation |

Compliant core: No gas (Evernode leases), hooks for escrow fulfillment, oracles for valuations (Chainlink/Evernode).

# Tech Stack: Separation of Concerns for Scale

We build with surgical precision – no monoliths. Evernode CLI (docs.evernode.org) deploys .hp bundles to isolated hosts; Xahau hooks handle state without EVM bloat.

| Layer | Tech | Why (Separation) | Trap Avoidance |
|-------|------|------------------|----------------|
| **Game Logic** | Evernode HotPocket (JS/Rust) | Deterministic execution, auto-synced bundles | Host disk only – no central DB; leases via Sashimono heartbeat (UFW: 26201-26225/tcp open) |
| **State** | Host Disk + Xahau Hooks | 24/7 consensus, no gas | Hooks for escrow/ZK verify – outbound only (UFW allow outgoing) |
| **Heavy Assets** | IPFS/nft.storage | Immutable CDN for art/models | Off-host – query via URI, no lease bloat |
| **Frontend** | React + Xaman Wallet + IPFS Gateway | Wallet-native UX | Vercel static – zero Hetzner ports 80/443 (UFW deny incoming) |
| **Payments** | XRPL Pathfinding (any → XAH) | Frictionless | TrustSets auto-accept; blackhole issuer for MPT |

**Vault Smart Contract Stub** (Angell Framework v0.1.0-alpha, deployable via xrpl-contracts CLI on AlphaNet):
```rust
// evervault_rwa.rs – ZK-Gated QOZB Mint + Escrow
use xrpl_contracts::{xrpl, AccountId, Amount, Transaction, EscrowCreate, MptIssue, TrustSet, emit_batch};

const BLACKHOLE_ISSUER: &str = "rBlackholeVaultRWA1111111111111111111111111";
const MIN_INVESTMENT: u128 = 100_000_000; // 100 XRP
const ESCROW_YEARS: u64 = 10;

#[xrpl_contract]
pub fn create_vault(invoker: AccountId, amount: Amount, qozb_id: String, zk_proof: ZKProof) -> Result<Vec<Transaction>> {
    if amount.drops() < MIN_INVESTMENT { return Err("Min investment"); }
    if !verify_zk_proof(&zk_proof, &qozb_id)? { return Err("ZK fail: accreditation"); }
    
    let shares = amount.drops() / get_qozb_valuation(&qozb_id)?;
    let mpt = MptIssue { /* mint QOZB-{id} to invoker */ };
    let escrow = EscrowCreate { /* 10yr lock to manager */ };
    let trust = TrustSet { /* auto-accept, no-ripple */ };
    
    emit_batch(vec![Transaction::MptIssue(mpt), Transaction::EscrowCreate(escrow), Transaction::TrustSet(trust)])
}

fn verify_zk_proof(_proof: &ZKProof, _qozb_id: &str) -> Result<bool> { /* Oracle/Hook stub */ Ok(true) }
fn get_qozb_valuation(qozb_id: &str) -> Result<u128> { /* Evernode oracle */ match qozb_id { "BROOKLYN-001" => Ok(1_000_000), _ => Err("Unknown") } }
