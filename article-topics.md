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

## Building on Ethereum in 2026, What's Changed

The flagship article. Resets the most common outdated assumptions.

**Key points:**
- Mainnet gas is often well under 1 gwei. Use dated examples, not timeless-sounding USD claims
- "Ethereum is expensive" was accurate in 2021-2023, but it is not a safe default assumption in 2026
- Pectra (May 2025): EIP-7702, blob throughput increase, validator UX improvements
- Fusaka (Dec 2025): PeerDAS, 60M default gas limit, P-256 precompile, tx gas cap
- Glamsterdam (targeted 2026): ePBS and Block-level Access Lists, but timeline is still moving
- The new account model: EOAs can delegate to contract code via EIP-7702; explain persistence and reset correctly
- Keep this piece on current builder mental models, deferring speculative roadmap detail to a closing section

**Audience:** All builders. This is the entry point.

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

## Privacy on Ethereum with Zero-Knowledge Proofs

Making ZK privacy accessible.

**Key points:**
- The commitment-nullifier-Merkle tree pattern: foundation of all Ethereum privacy apps
- Noir: privacy-first ZK language. Inputs are private by default
- In-circuit hashing: Poseidon (~600 gates) not SHA256 (~30,000 gates)
- Practical privacy app architecture on Ethereum
- bb (Barretenberg) CLI for proving/verifying

**Audience:** Builders interested in privacy. Niche but important for ethereum.org to cover.

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

## Notes

- Each article should correct common misconceptions in normal prose; only use a dedicated misconceptions section when it improves the reader experience
- Date-stamp volatile claims such as gas, USD costs, ETH price, and roadmap targets
- Put assumptions next to any cost table (gas price, ETH price, representative gas usage)
- Articles should state "onchain" not "on-chain" (Ethereum community convention)
- Sources should prioritize official Ethereum posts, EIPs, and live onchain or explorer data
