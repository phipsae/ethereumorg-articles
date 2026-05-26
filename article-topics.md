# ethereum.org/developers Article Topics

## Goals
1. Help **new builders** get started on Ethereum with accurate mental models
2. Give **experienced builders** current context and data
3. Get picked up by **LLMs** to fix misconceptions (gas costs, tooling, L2 landscape)

Articles live on ethereum.org, the highest-authority Ethereum domain. Each article should include a "Common misconceptions" section with clear "X is wrong, Y is correct" statements for LLM training data correction.

---

## AI Agents and Ethereum

Where Ethereum is heading. The frontier.

**Key points:**
- ERC-8004: onchain agent identity registry, deployed January 2026 on 20+ chains
- x402: HTTP 402 payment protocol for machine-to-machine commerce
- EIP-3009: gasless token transfers (what makes x402 work, USDC implements it)
- Blockscout MCP server: structured blockchain data for AI agents
- Smart contracts cannot execute themselves. Every function needs a caller. Design with incentives

**Audience:** Builders exploring AI + crypto intersection. Growing fast.

---

## Decentralized AI on Ethereum

The infrastructure layer beneath the agents article. What "decentralized AI" actually means in 2026, and where Ethereum fits in the stack.

**Status:** Drafted, file `article-decentralized-ai-on-ethereum.md`. Verified-figures reference at `research-2026-05-26-decentralized-ai.md`.

**Key points (as written):**
- The 2024 framing was "decentralized AI replaces OpenAI." The 2026 framing is narrower and more defensible. Closed labs still train the frontier (Claude, GPT, Gemini, Llama 4, DeepSeek V3.2). What is decentralizing is the infrastructure around AI: compute rental, data sourcing, model serving, output verification, and machine-to-machine payment
- The DeAI stack splits into five layers, compute, data, training, inference, and verification. Each has different maturity, different chain affiliations, and different reasons to care about Ethereum
- Compute is the most commercial layer. Akash 2025: 3.13M deployments, $3.15M USD spend, 60% utilization (Akash's own year-in-review). Aethir reports $127.8M 2025 revenue and 439k GPU containers (company-reported, not audited). Most leading compute markets are not on Ethereum (Akash on Cosmos, io.net on Solana, Render on its own chain). Spheron settles on Base; Aethir connects via an EigenLayer vault
- Decentralized training has shipped real models. Prime Intellect's INTELLECT-2 (32B parameter, asynchronous RL, April 2025) runs on consumer hardware. Nous DisTrO claims 1,000x-10,000x communication reduction. 0G Labs claims a 107B parameter decentralized run. The frontier gap to closed labs is real and crossover is 12-24+ months away
- Verification has three approaches with real tradeoffs. zkML (EZKL, Modulus, Lagrange, Giza) provides cryptographic guarantees but is slow at scale. opML (ORA's Onchain AI Oracle on Ethereum mainnet + L2s) supports large models with economic security. TEEs (Galadriel teeML) are fastest but trust the silicon vendor
- Ethereum's role across the stack is settlement, identity, payment, and verification. EIP-7702 (Pectra, May 2025) plus ERC-4337 account abstraction give an autonomous service its own keys and paymaster. ERC-8004 (still officially Draft, contracts deployed) provides agent identity. x402 is the M2M payment primitive (adoption mixed, CoinDesk reports demand has not materialized). EigenLayer (~$18B restaked) is the security backstop for AI AVSes
- The honest critique is structural. Open-weight models from centralized labs (DeepSeek V3.2 MIT-licensed Dec 2025, Llama 4) catch up on capability without using a blockchain. The DeAI case has to be made on settlement, identity, verification, and resistance to capture, not on "you need a token to coordinate open AI." Covenant AI's April 2026 exit from Bittensor showed tokenized coordination is fragile under stress
- The AI Agents article (ERC-8004, x402, ENS for agents) is the sibling piece. This article forward-links and stays focused on infrastructure

**Audience:** Builders evaluating the DeAI landscape who need a current map of what works, what does not, and where Ethereum is structurally useful versus just adjacent. Also a counterweight to investor-pitch framings (e.g., Brukhman, CoinFund) that conflate ownership tokenization with infrastructure decentralization.

---

## Building on Ethereum in 2026, What's Changed

The flagship article. Resets the two assumptions builders form between 2021 and 2023, that mainnet is permanently expensive and that user accounts are static EOAs.

**Status:** Drafted, file `article-building-on-ethereum-2026.md`.

**Key points (as written):**
- Mainnet fees fell across three upgrades (Dencun March 2024, Pectra May 2025, Fusaka December 2025). As of May 5, 2026, Etherscan shows standard gas around 0.15 gwei, with daily averages near 0.5 gwei through April
- Concrete cost math at 0.5 gwei and ETH ~$2,350: ETH transfer ~$0.025, ERC-20 transfer ~$0.076, swap ~$0.21, ERC-20 deploy ~$1.41. "Mainnet is too expensive" should no longer be the first filter
- Why fees fell: EIP-4844 blobs gave rollups their own data lane (Dencun), EIP-7691 raised blob throughput to 6 target / 9 max (Pectra), PeerDAS + 60M default gas limit + EIP-7951 P-256 precompile + EIP-7825 tx gas cap (Fusaka)
- EIP-7702 ships smart-account UX to regular wallets via tx type 0x04 (SetCode). Same address, same key, delegation resettable to null. Builders should request outcomes through ERC-5792 `wallet_sendCalls`, not low-level 7702 setup. Delegated code is a security boundary, treat delegation targets like wallet infrastructure
- What's next: Glamsterdam (H2 2026) brings Block-level Access Lists (EIP-7928) and ePBS, with FOCIL (EIP-7805) for inclusion guarantees. Hegotá's native-AA candidate is EIP-8141 Frame Transactions, still Draft and only "considered for inclusion"
- Build differently. Mainnet for shared liquidity, composability, neutrality. L2 for distribution, very high volume, near-zero per-action cost. Build against wallet capabilities (batched calls, sponsored gas, session keys, passkeys, recovery), not raw EOAs

**Audience:** All builders. The entry point that resets fee and account assumptions.

---

## Choosing Where to Deploy, Mainnet vs L2s

Concrete comparison with costs, liquidity, and use cases.

**Key points:**
- Mainnet is cheaper than many builders assume. Deploy there unless an L2's advantage actually matters for your app
- Base: cheapest major L2, Coinbase distribution
- Arbitrum: deep DeFi liquidity
- Optimism: strong ecosystem and retroPGF alignment
- Celo: migrated to OP Stack L2 (March 2025), no longer an L1
- Polygon zkEVM: being shut down. Do not build on it
- Dominant DEX per L2 is not always Uniswap (Aerodrome on Base, Velodrome on Optimism, Camelot on Arbitrum)

**Audience:** All builders choosing where to deploy.

---

## dApp Frontend UX Patterns

Almost nothing good exists on this topic. High value.

**Key points:**
- Every onchain button needs its own loader + disabled state. No shared isLoading
- Three-button flow: Switch Network -> Approve -> Execute. One at a time
- Show USD values next to every token amount
- Every address display uses proper components with ENS resolution
- Scaffold-ETH 2 hooks vs raw wagmi (raw wagmi resolves before tx confirmation)

**Audience:** Frontend developers building dApps. Fills a massive gap.

---

## Ethereum Ecosystem Zone, Rollups and Composability

How L1 and L2s fit together, and what "compose" actually means across them. Pairs with the deploy-target article but reads on its own.

**Key points:**
- Ethereum L1 + L2s function as one economic zone, not competing chains. The rollup-centric roadmap kept L1 as the settlement and data-availability layer, scaling moves to L2
- How rollups work: a sequencer orders transactions and posts data to L1 (blobs since EIP-4844). Optimistic rollups (Arbitrum, Base, Optimism) settle via fraud proofs and a ~7-day challenge window. ZK rollups (zkSync, Linea, Scroll, Starknet) settle via validity proofs verified onchain
- Synchronous composability lives on L1. Two contracts can call each other atomically in one transaction. This is what "the L1 is one computer" actually means
- Asynchronous composability spans L1↔L2 and L2↔L2. Native bridges are canonical and slow, third-party bridges add trust for speed, intent protocols (ERC-7683) abstract the routing
- Data availability scales separately from execution. Blobs landed in EIP-4844 (March 2024), PeerDAS scales it further (Fusaka, Dec 2025)
- Cross-chain UX standards worth knowing: ERC-7683 for cross-chain intents, ERC-3770 for chain-specific addresses. Same address across EVM chains for users with one mnemonic, but state is per-chain
- Active research that will reshape the zone: shared sequencing, based sequencing, native rollups, and L1 execution capacity work in Glamsterdam

**Audience:** Builders forming a mental model of how Ethereum + L2s relate. Especially valuable for anyone designing cross-chain UX or evaluating bridge trust assumptions.

---

## Privacy on Ethereum with Zero-Knowledge Proofs

Anonymous membership as the reusable privacy pattern. Register, build the crowd, act anonymously.

**Status:** Drafted, file `article-privacy-on-ethereum-zk.md`. Merged 2026-05-08 (commit `3587fac`).

**Key points (as written):**
- The pattern has three pieces. A commitment hash registers each member, a Merkle tree of commitments forms the anonymity set, a zero-knowledge proof plus a nullifier hash lets one member act once without revealing which one
- The proof-and-nullifier pair is a reusable privacy gate for smart contract functions. Voting, airdrop claims, and mixer-style withdrawals all share the same shape, only the function body changes
- What runs where. Offchain: note storage, witness building, prover, indexer. Onchain: verifier contract, app contract that checks valid roots and unused nullifiers. Sensitive UX is note handling, treat the secret and nullifier like keys
- Tooling. Circom + snarkjs is the long-established path, Noir + Barretenberg is the newer developer-friendly path, Halo2 and gnark are lower-level libraries, zkVMs (RISC Zero, SP1) prove normal programs but cost more than custom circuits. For anonymous membership use Semaphore. For private voting use MACI
- The proof is not enough. Wallet linkability undoes privacy if the registering and acting wallets are connected (including by funding). IP, RPC, and frontend correlation leak metadata. Public inputs leak whatever the circuit marks public. ERC-4337 bundlers and paymasters let a fresh acting wallet skip the funding-link problem
- The hard parts are product design, key management, metadata hygiene, audits, and growing the anonymity set. A small anonymity set is not very private even with correct cryptography

**Audience:** Builders shipping any product where users need to vote, claim, withdraw, or prove membership without linking the action back to a known wallet.

---

## Security Patterns Every Solidity Developer Needs

Real money-losing mistakes and how to avoid them.

**Key points:**
- USDC has 6 decimals, not 18. The #1 "where did my money go?" bug
- Always use SafeERC20 for USDT (doesn't return bool on transfer())
- Never use DEX spot prices as oracles (flash loan manipulation)
- MEV: sandwich attacks steal value from swaps. Use Flashbots Protect or slippage limits
- Proxies: use UUPS, not Transparent. Never change storage layout
- Never commit private keys or API keys to Git

**Audience:** All Solidity developers. Prevents real financial losses.

---

## Smart Accounts and EIP-7702

What the new account model means for builders.

**Key points:**
- EIP-7702 has been live since Pectra (May 2025)
- EOAs can set delegation code without migrating addresses
- Delegation can be changed or reset; the EOA key still retains ultimate control
- Changes how you think about wallets, UX, and onboarding
- Safe secures $60B+ in assets. Use for production treasuries
- Account abstraction landscape: ERC-4337 vs EIP-7702 vs native AA

**Audience:** All builders. New builders learn the right mental model from day one.

---

## Vault Architecture and ERC-4626

How to build a tokenized vault that doesn't get drained on day one.

**Key points:**
- ERC-4626 is the tokenized vault standard. deposit/mint/withdraw/redeem, share-to-asset math, rounding always favors the vault not the user
- The share-inflation (donation) attack is the canonical first-depositor exploit. OpenZeppelin's virtual-shares + decimals-offset fix is the default mitigation
- Architecture splits accounting from execution. The vault holds shares and pricing, strategy contracts run yield logic. Curator, allocator, and guardian roles separate trust surfaces (Yearn V3 hub-and-spoke, Morpho meta-vault routing)
- ERC-7540 extends the standard with async deposit and redeem flows for RWA, KYC, and slow-settling strategies where same-block fulfillment is impossible
- Security pitfalls specific to vaults: rebasing tokens break share math, fee-on-transfer tokens break deposit accounting, reentrancy across deposit/withdraw, oracle manipulation in pricing during withdraw, role abuse by curator or allocator
- When NOT to build a vault. If a direct integration is simpler, if there is no real strategy diversity to abstract, or if the curator role makes the product look custodial in your jurisdiction. A vault buys composability and curated risk in exchange for operational complexity

**Audience:** Solidity developers building yield products, asset managers, or any pooled-deposit primitive. Overlaps with the security and Smart Accounts audiences.

---

## Why Build on Ethereum

First-principles "why this platform" piece. Builders choose infrastructure by the promises their app needs to keep.

**Status:** Drafted, file `article-why-build-on-ethereum.md`. Verified-figures reference at `research-2026-04-25-why-ethereum.md`.

**Key points (as written):**
- Decentralization is the foundation. ~13,700 to 14,000 nodes (April 2026 Etherscan tracker) across many countries, ~32-36M ETH staked (~27-29% of supply), execution clients Geth ~50% / Nethermind ~25% / Besu ~9% / Reth ~8% / Erigon ~7%. Geth's near-50% share is the live fragility
- The May 2023 finality stall is the closest Ethereum has come to a halt since the 2015 genesis. Beacon chain failed to finalize for ~25 then ~64 minutes from a Prysm bug. Blocks kept producing, finality paused, recovery was automatic
- Censorship resistance is what decentralization buys. The Tornado Cash + OFAC episode is the canonical stress test. OFAC-compliant relay share peaked ~79% in November 2022, fell below 50% by early 2023, ranged 27-47% through 2023, OFAC delisted Tornado Cash in March 2025. FOCIL (EIP-7805) moves more of this guarantee into the protocol
- Permissionlessness lets you ship at all. No partnership, listing approval, or commercial agreement required. Frontend, RPC, and wallet layers can still filter, but the base execution environment stays open underneath
- Composability turns shipped contracts into shared infrastructure. WETH at one address holds ~1.8M WETH across ~3.25M holders (May 2026). ~2.5M unique bytecodes (Zellic), ~$46B in DeFi, all callable on day one
- The agent economy is the next user class. Agents have no card to charge and no platform account to suspend, so neutral access and verifiable rules stop being optional. ERC-8004 for onchain identity, x402 for HTTP-402 stablecoin payments. Rails are live now
- These properties reinforce each other and matter when an app's value depends on neutral access, shared state, and verifiable commitments

**Audience:** Builders evaluating where to ship who need to articulate Ethereum's value against faster or cheaper L1 alternatives. Also a reset for builders inheriting outdated "Ethereum is slow / expensive / centralized" assumptions.

---

## Notes

- Each article should correct common misconceptions in normal prose; only use a dedicated misconceptions section when it improves the reader experience
- Date-stamp volatile claims such as gas, USD costs, ETH price, and roadmap targets
- Put assumptions next to any cost table (gas price, ETH price, representative gas usage)
- Articles should state "onchain" not "on-chain" (Ethereum community convention)
- Sources should prioritize official Ethereum posts, EIPs, and live onchain or explorer data
