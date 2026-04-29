# "Why Build on Ethereum" Reference Doc (April 2026)

Compiled 2026-04-25. All numbers carry their source URL and observation date. Where data is contested, sparse, or unverifiable, this is flagged explicitly.

## Network decentralization

**1. Active validators on Ethereum mainnet.** Approximately 1.07 million active validators as of early 2026 (post-Pectra, validator consolidation now allows up to 2,048 ETH per validator instead of 32). Compounding-credential validators alone account for roughly 994,000 of these. Source: CryptoRank/CryptoSlate aggregations of beaconcha.in data, https://cryptoslate.com/ethereum-nears-1-million-active-validators-as-network-surge-strengthens-security/ . Qualifier: beaconcha.in itself blocks automated fetches; live exact count should be re-verified at https://beaconcha.in/charts/validators before publication.

**2. ETH staked and percentage of supply.** Approximately 35.86 million ETH staked, equal to about 28.9 percent of total supply (early 2026 reading per Datawallet/coinlaw aggregating beaconcha.in). A separate April 23, 2026 read cited "over 32 million ETH / over 26 percent" (The Block on-chain dashboard). Sources: https://www.datawallet.com/crypto/ethereum-staking-statistics-and-trends , https://www.theblock.co/data/on-chain-metrics/ethereum/ethereum-percentage-eth-staked . Qualifier: the two figures disagree; safest claim is "roughly 32 to 36 million ETH staked, ~27 to 29 percent of supply."

**3. Full nodes and geographic distribution.** Etherscan's node tracker detected 14,339 Ethereum nodes globally in early 2026. Top countries: United States 38.96 percent (5,630 nodes), Germany 14.53 percent (2,100), China 14.01 percent (2,024). Linux runs about 66 percent of nodes (ethernodes.org, 2025). Source: https://etherscan.io/nodetracker , https://ethernodes.org/countries . Qualifier: ethernodes.org returned 403 to automated fetch; numbers are from third-party citations of the dashboard.

**4. Execution-layer client diversity.** Per clientdiversity.org (data sourced from ethernodes, updated daily, no fixed timestamp on page): Geth 50.13 percent, Nethermind 25.46 percent, Besu 9.45 percent, Reth 7.67 percent, Erigon 6.53 percent, Other 0.78 percent. Source: https://clientdiversity.org/ . Qualifier: Geth above 50 percent is the long-standing supermajority concern; this figure has fluctuated around the threshold for years.

**5. Consensus-layer client diversity.** clientdiversity.org publishes three datasets that disagree materially:
- Miga Labs: Lighthouse 52.93, Prysm 24.53, Teku 11.18, Lodestar 3.81, Nimbus and others under 1 percent each.
- Rated Network: Teku 53.86, Prysm 21.17, Lighthouse 20.6, Nimbus 3.12, others under 1.
- Sigma Prime Blockprint: Teku 99.83 (almost certainly a labeling artifact, not real distribution).
Source: https://clientdiversity.org/ . Qualifier: cite Miga Labs or Rated, not Blockprint. The honest summary is "Lighthouse, Prysm, and Teku each plausibly between 20 and 55 percent depending on methodology; no single client at supermajority."

## Uptime and reliability

**6. Ethereum mainnet outages.** Ethereum has never had a full chain halt since genesis (July 30, 2015). The closest events were the May 11 to 12, 2023 finality stalls: on May 11 finalization paused for 4 epochs (~25 minutes) and on May 12 for 9 epochs (~64 minutes), both attributed to a Prysm replay/cache bug. Block production continued; client diversity allowed recovery without manual intervention. Source: https://medium.com/offchainlabs/post-mortem-report-ethereum-mainnet-finality-05-11-2023-95e271dfd8b2 , https://www.coindesk.com/tech/2023/05/11/ethereum-mainnet-was-unable-to-fully-finalize-transactions-for-25-minutes . Precise claim: "Ethereum has never had a chain halt; finality was briefly delayed twice in May 2023 but blocks kept being produced."

**7. Solana / Sui / Aptos outage history 2024 to 2026.**
- Solana: last officially acknowledged major outage February 6, 2024 (~5 hours, LoadedPrograms bug). StatusGator independently logged at least nine unacknowledged disruptions Oct 2024 to Feb 2025, including Nov 20, 2024 (39 min), Dec 23, 2024 (25 min), Jan 18, 2025 (1h13m). No official outage 2025 to present. Sources: https://www.helius.dev/blog/solana-outages-complete-history , https://statusgator.com/blog/solana-outage-history/ .
- Sui: November 21, 2024, ~2.5 hours (congestion-control bug, fixed in v1.37.4). December 14, 2025, "degraded consensus" with elevated latency. January 14 to 15, 2026, ~6 hours, validators failed to reach consensus and ~$10B in assets were frozen. Sources: https://crypto.news/sui-consensus-bug-six-hour-network-outage-2025/ , https://www.coinspeaker.com/sui-network-mainnet-outage-january-2026/ .
- Aptos: 5-hour outage on October 18 to 19, 2023 (one-year anniversary). No reported downtime 2024 or 2025 per Aptos Foundation year-in-review. Source: https://decrypt.co/202340/aptos-hit-5-hour-outage-blockchains-first-birthday , https://aptosnetwork.com/currents/2025-year-in-review .

## Censorship resistance

**8. MEV-Boost relay censorship rate.** mevwatch.info loads dynamically and would not return numeric data through automated fetch. Known structural facts: 4 of the 7 major MEV-Boost relays are OFAC-compliant censoring relays (Flashbots, BloXroute Regulated, BlockNative, Eden). Non-censoring relays in active use: Agnostic, Ultra Sound, Aestus, BloXroute Max Profit/Ethical. Trend since OFAC's August 8, 2022 Tornado Cash sanction: peaked at 79 percent OFAC-compliant blocks on Nov 21, 2022; fell below 50 percent in Feb 2023; ranged 27 to 47 percent through 2023. Source: https://www.mevwatch.info/ , https://www.theblock.co/post/230179/ethereums-ofac-compliant-blocks-fall-to-27-marking-a-drop-in-protocol-level-censorship . Qualifier: I could not retrieve a current April 2026 figure programmatically; recommend manually grabbing the live mevwatch.info number before publication.

**9. FOCIL / EIP-7805 status April 2026.** SFI'd (Scheduled For Inclusion) as the consensus-layer headliner of the Hegota fork (the upgrade following Glamsterdam). Previously CFI'd for Glamsterdam but moved to Hegota to avoid schedule risk. Source: https://etherworld.co/focil-eip-7805-cfid-with-strong-developer-community-backing/ , https://ethereum-magicians.org/t/hegota-headliner-proposal-focil-eip-7805/27604 , EF Checkpoint #9 https://blog.ethereum.org/2026/04/10/checkpoint-9 .

## Agent economy and composability

**10. ERC-8004 adoption.** Mainnet deployment January 29, 2026. Within 3 days: 22,900+ registrations. Within 2 weeks: 20,000+ agents across multiple chains. Within first month: 45,000+ agents across EVM networks per 8004 Scan. Most activity on L2s (Base, Optimism, Arbitrum) due to fee economics. Source: https://www.ainvest.com/news/erc-8004-chain-flow-21-000-ai-agents-2602/ , https://www.the-blockchain.com/2026/01/28/ethereum-prepares-erc-8004-mainnet-launch-this-week-eyes-eu-expansion/ , https://eips.ethereum.org/EIPS/eip-8004 . Qualifier: data is sparse and largely from PR-adjacent sources; treat 45k as ballpark.

**11. x402 adoption.** Over 167M cumulative transactions settled, ~85 percent on Base. Adoption peaked late 2025; weekly transactions fell from millions to under 1M by early 2026. Coinbase released "Upto" usage-based pricing model in 2026. x402 Foundation incubated under Linux Foundation, 20+ institutional backers (Cloudflare, Stripe, AWS, Google, Visa, Circle, Solana Foundation). Source: https://github.com/coinbase/x402 , https://www.coindesk.com/markets/2026/03/11/coinbase-backed-ai-payments-protocol-wants-to-fix-micropayment-but-demand-is-just-not-there-yet , https://blog.cloudflare.com/x402/ . Qualifier: CoinDesk explicitly notes demand has not materialized; cite carefully.

**12. WETH on mainnet.** Total supply 1,828,336.05 WETH (1 to 1 with ETH). Onchain market cap ~$4.24B. ~3.25M holders. WETH is the de facto unit of account for nearly every Ethereum DEX, lending protocol, and bridge (Uniswap, Aave, Compound, MakerDAO, Curve, Balancer, 1inch, every L2 canonical bridge). Source: https://etherscan.io/token/0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2 (observed April 19 to 25, 2026).

## Sanity checks

**13. Unique smart contracts deployed on Ethereum mainnet.** Roughly 70 million total contract deployments since genesis, but only ~2.5 million unique bytecodes (May 2025 Zellic enumeration; no later comprehensive count published). Source: https://www.zellic.io/blog/all-ethereum-contracts/ . Qualifier: 2026-specific figure not available; "tens of millions of contracts, millions unique" is the safe phrasing.

**14. ETH price and DeFi TVL.**
- ETH price: $2,315 to $2,322 on April 24 to 25, 2026; $2,403 on April 22. Source: Fortune daily price tracker, https://fortune.com/article/price-of-ethereum-04-24-2026/ .
- DeFi TVL Ethereum L1: ~$55.6B to $57.2B, roughly 50 to 68 percent of all DeFi depending on inclusion of LSTs/restaking/BTC DeFi. Source: https://defillama.com/chain/ethereum (April 17, 2026).
- L2 TVS (L2Beat): Arbitrum $15.81B, Base $11.93B, Mantle $1.61B, OP Mainnet $1.58B (live page, late April 2026). Earlier Feb 2026 readings: Arbitrum $16.84B, Base $10.72B. Total L2 TVS not surfaced as a single page number but the major-L2 basket sits around $35 to 40B. Source: https://l2beat.com/scaling/tvs .
- L1 + L2 combined Ethereum-secured value: roughly $90 to $100B, depending on definition (DefiLlama L1 TVL plus L2Beat L2 TVS, with double-counting caveats around bridged ETH). Qualifier: this is a derived figure, not a single canonical number from either source; cite the components, not the sum.
