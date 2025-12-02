# EverArcade & EverVault: Compliant RWA Arcade + QOZB DeFi Primitive on Evernode/Xahau
## v1.0 | December 2025 | Chad [Your Last Name], Founder & Evernode Architect

### Executive Summary (1 page)
- **Mission**: Unified platform: Play-to-own arcade (HotPocket dApps) acquires users → tokenized 10-year QOZB vaults (Angell smart issuers + ZK accreditation) for tax-advantaged yields. Blue-ocean: Rural job creation via remote beta testing + compliant RWAs for institutional acquisition.
- **Traction**: Live landing (everarcade.games), Hetzner MVP host earning leases, litepaper deployed. One-man bootstrap → $250k XRPL ask for legal/MVP scale.
- **Projections**: 10k DAUs → $1M ARR (1% tx fees); Acquisition target: Q4 2026 at 10x multiple.
- **Ask**: XRPL Grants R7 ($250k: $100k legal/Reg D, $75k oracle/ZK, $75k arcade beta); Local rural grants for job dev.

### The Problem & Opportunity (2 pages)
- Rural dev gap: Underserved areas need job-creating tech (your location as proof). DeFi lacks compliant RWAs (QOZ tax breaks untapped on XRPL).
- Market: $10T RWA tokenization by 2030; Gaming → DeFi flywheel (e.g., Axie but compliant). Blue-sky: Reg D vaults for accrediteds, arcade for mass onboarding.

### Solution: The EverArcade Flywheel (3 pages)
- **Arcade Layer**: Modular HotPocket games (.hp bundles deployed via `evrs deploy`) – state on host disk + Xahau hooks, assets IPFS. Payments: XRPL pathfinding (any token → XAH).
- **Vault Layer**: Personalized QOZB mints (Angell escrow creates, MPT issues via blackhole issuer). ZK proofs gate accreditation; 10-year escrows for yields.
- **Unification**: Game wins → fractional shares; Vault yields → in-game boosts. Conglomerate: Game studio (retention) + DeFi primitive (yield) + Security (Reg D compliant).

### Technical Architecture (5 pages)
- **Separation of Concerns** (Deep-dive, with diagrams – use Mermaid in MD):
  | Layer | Tech | Storage/Why | Trap Avoidance |
  |-------|------|-------------|---------------|
  | Game Logic | HotPocket JS/Rust | Host disk (Hetzner) | No web ports (UFW deny 80/443); SSH via `host.everarcade.games` A record. |
  | State | Xahau Hooks + Disk | Consensus (no gas) | Modular: `systemctl --user status sashimono` isolated from Vercel. |
  | Assets | IPFS/nft.storage | Immutable CDN | Off-chain; URI on-chain for query. |
  | Vaults | Angell Issuers + Escrows | XRPL MPT/TrustSet | ZK oracle stub → Chainlink integration milestone. |
- **Deploy Flow**: `curl evernode.sh | bash` → UFW ranges (22861-22885/tcp Sashimono) → `evrs deploy`. IPv6 mirror for scale.
- **Compliance Tech**: Reg D gates via ZK (Groth16 proofs); Escrow maturity = 10y QOZ hold.

### Business Model & Compliance (4 pages)
- **Revenue**: 1% on arcade txns + vault fees; Rural jobs: 50 beta testers (grant-tied).
- **Reg D/SEC Path**: Exemption for accredited investors (Howey Test compliant via ZK verification). Ask includes $100k for counsel (SPV custody, AML/KYC via FinCEN MSB reg). Blue-sky: Offshore (Cayman) for non-US, onshore trusts for US.
- **Risks/Traps**: Solo scale → funding milestones; Legal: Substance-over-form (tokens = securities → Reg D filing).

### Roadmap & Milestones (2 pages)
- **Q1 2026**: Testnet vaults (ZK oracle live); Local grant-funded beta (10 rural jobs).
- **Q2**: Mainnet arcade; XRPL funds disbursed.
- **Q4**: Acquisition outreach (Animoca for gaming, Ripple for RWAs).

### Team & Ask (1 page)
- **You**: Evernode OG, shipped Hetzner host, Vercel infra. Advisors: Denis Angell (issuers).
- **Funding**: XRPL $250k (legal 40%, tech 30%, marketing 30%). Local: USDA RBDG $50k (job dev).

### Appendix: Code Snippets & Projections (7 pages)
- Rust hook stub from your paste (evervault_rwa.rs).
- ARR model: Simple table (DAUs → Fees).
