# Why Build on Ethereum

Builders choose infrastructure by the promises their app needs to keep.

Most software promises depend on an operator. A cloud provider keeps the server running. A platform keeps the account open. A payment processor keeps the merchant enabled. An API provider keeps the key valid. That is fine for many products. It is not enough when the product's value depends on neutral access, shared state, and commitments that users and other developers can verify for themselves.

Ethereum is built for that second category. No one owns it. The chain runs across many countries, many operators, and multiple independent client implementations. Its rules can change through public protocol upgrades, but no single company, validator, sequencer, or foundation can quietly rewrite them on its own.

For a builder, that means Ethereum is not just a place to host code. It is a place to make public commitments. Users can keep reaching what you deploy, other developers can build on it without asking you, and your app can continue to work even when any one party, including you, stops cooperating.

---

## Decentralization

The first reason to build on Ethereum is that the system is not operated by one party. If your app settles assets, records ownership, coordinates markets, or becomes infrastructure for other apps, you do not want the base layer to depend on a company account, a single data center, or one implementation being correct forever.

Ethereum's decentralization shows up across several dimensions, including who runs nodes, where those nodes are located, which client software they run, and how much economic stake secures consensus. Around 13,700 to 14,000 nodes were tracked in Etherscan's node tracker in April 2026, distributed across the United States, Germany, China, the United Kingdom, Russia, Japan, and dozens of other countries.

Every Ethereum node runs two pieces of software side by side. An execution client runs the EVM and tracks contract state. A consensus client handles proof-of-stake. It tracks which validators propose blocks, which blocks the network accepts, and when a block becomes final. Healthy decentralization needs multiple independent implementations of each, so a bug in one client does not automatically become a bug in Ethereum.

The execution layer has five major clients in production. Geth runs at roughly 50%, Nethermind around 25%, Besu around 9%, Reth around 8%, and Erigon around 7%. The consensus layer runs on Lighthouse, Prysm, Teku, Nimbus, Lodestar, and other clients. Different data sources rank the top consensus clients differently, but the important fact is that Ethereum is not a single-client chain.

Geth sitting near 50% is the honest fragility worth naming. A bug in a minority client is painful for its operators, but the rest of the network can continue. A severe bug in a client with majority share is more dangerous. That is why client diversity is treated as a live operational priority, not a marketing point.

The other layer is economic. About 32 to 36 million ETH, around 27 to 29% of supply, is staked by validators as collateral they can lose if they break the protocol's rules. An attacker would need to acquire and risk a meaningful fraction of that stake to corrupt the chain, and the protocol can slash that stake when validators provably misbehave. At April 2026 ETH prices, that means tens of billions of dollars would be at risk.

Ethereum has never had a full chain halt since genesis on July 30, 2015. The closest mainnet has come to a major incident was on May 11 to 12, 2023, when the consensus layer, called the Beacon Chain, failed to finalize for about 25 minutes and then later for about 64 minutes. The cause was a Prysm client bug. Finality requires more than two-thirds of validators to attest, and Prysm's share at the time was high enough that its issue briefly pulled the network below that threshold.

A finality stall is not the same as a chain halt. New blocks kept being produced, transactions kept being included, and most users and applications kept working. What stalled was Ethereum's strongest settlement guarantee. Under normal consensus assumptions, a block older than roughly 13 minutes cannot be reverted. Bridges, exchanges, and other systems that wait for finality before crediting deposits would have paused those flows. The chain itself recovered automatically once enough validators caught up, without manual intervention.

For builders, that history matters because reliability is not an abstract virtue. If other people are going to hold assets in your contracts, route orders through your market, or build on your primitive, they need the foundation underneath it to keep running through bugs, client failures, and institutional pressure.

---

## Censorship resistance

Decentralization is the structure. Censorship resistance is one of the practical things it buys. Users should not need permission from a company, government, relay, validator, RPC provider, or app operator to send a valid transaction to your contracts.

That does not mean every transaction lands in the next block. It means no single party can keep a valid transaction off the chain forever. Block production rotates across many independent validators, builders, and relays. If one of them filters your transaction, the next slot has a different set of actors, and eventually one of them includes it. Censorship has to persist across that whole rotating cast, which is much harder than one operator saying no. The post-Tornado Cash period showed what that looks like under pressure.

Tornado Cash is a privacy mixer contract that breaks the onchain link between deposit and withdrawal. On August 8, 2022, the US Treasury's sanctions office, OFAC, added it to the sanctions list. Several major MEV-Boost relays responded by refusing to forward blocks that included transactions from sanctioned addresses. The share of blocks passing through those "OFAC-compliant" relays climbed quickly and peaked near 79% in November 2022. The remaining 21% of blocks were built by relays and builders that did not filter, so Tornado Cash transactions could still land. Filtering made those transactions slower, not impossible. With only about one in five blocks available, the expected wait rose from about 12 seconds to about a minute.

That looked alarming, and it was. Then the number fell. By early 2023 it was below 50%, and through 2023 it generally ranged between roughly 27% and 47% depending on the week. New relays launched explicitly without filters, including Ultra Sound and Agnostic, and proposers were free to add them to their MEV-Boost setup. No one could force every proposer onto a filtering relay, so the share could not stay where it peaked. OFAC removed Tornado Cash from the sanctions list on March 21, 2025, but the episode remains Ethereum's clearest censorship-resistance stress test.

Ethereum is also moving more of this guarantee into the protocol itself. A planned upgrade called FOCIL (EIP-7805) adds inclusion lists. Randomly selected validators publish transactions they see in the public mempool, and the next block is expected to satisfy those lists. If a block ignores them, the rest of the network can reject it. So noone can stop your users from using your app. 

---

## Permissionless

Censorship resistance is about whether users can keep reaching your app after you ship. Permissionlessness is about whether you can ship in the first place.

Deploying on Ethereum does not require a partnership, account, listing approval, app-store review, or commercial agreement. Anyone can deploy code, call a contract, run a node, index data, build a wallet, or publish an interface. The base layer does not know whether you are a startup, a bank, a solo developer, an agent, a DAO, or a user with no company at all.

That changes the builder model. On a platform, the platform owner can change terms, revoke keys, block regions, remove apps, or make access conditional on a business relationship. On Ethereum, the protocol evaluates transactions by the same public rules for any caller. A contract deployed today runs by those public rules for every address as long as the chain keeps running.

This does not remove every dependency. Frontends can be taken down. RPC providers can filter. Wallets can choose what they display. App teams can still write upgradeable contracts or admin controls that users need to evaluate. But the base execution environment is open. A user can call the contract directly. Another developer can build a new interface. An indexer can read the state. A competing app can compose with the same contracts.

That is why Ethereum apps can become public infrastructure instead of private integrations. Standard ERC-20 interfaces are callable by any address. DEX routers quote swaps to any caller. Lending markets let any address open a position if it satisfies the protocol's rules. There is no API key to revoke and no waitlist to clear.

---

## Composability

Permissionlessness lets you ship. Composability is what makes shipping on Ethereum different from shipping on a closed platform. Contracts are public building blocks, and every new contract can read from or call the contracts already deployed.

WETH is the cleanest example. The contract sits at one fixed mainnet address, held about 1.8 million WETH in April 2026, had roughly 3.25 million holders, and acts as a common unit across DEXs, lending markets, vaults, and bridges. It is not an integration partner. It is code that thousands of other contracts and apps can use directly.

That pattern repeats across the ecosystem. From genesis to early 2025, Ethereum saw tens of millions of contract deployments and roughly 2.5 million unique bytecodes by Zellic's count. Standards like ERC-20 and ERC-721 became coordination layers. A token your contract emits can be traded on a DEX, borrowed against in a money market, indexed by analytics tools, displayed in wallets, and bridged or wrapped by other systems without each team negotiating a custom agreement.

The builder consequence is the difference between shipping a feature and shipping a primitive. A feature serves your own app. A primitive becomes something other people can compose with, route through, fork, extend, and make more useful than you could have planned. That compounding effect is why liquidity, standards, and developer attention matter so much.

As of April 2026, roughly $55 to 57 billion sat in DeFi on Ethereum L1 alone, with more value secured by Ethereum through L2s. That is not just capital. It is live infrastructure, including assets, markets, oracles, wallets, account systems, governance contracts, bridges, analytics, and developer tools that a new builder can plug into on day one.

---

## The agent economy

AI agents are an emerging example of why Ethereum's properties matter. They are not the only reason to build on Ethereum, and the market is still early. But they make the infrastructure problem easy to see.

On a typical hosted stack, an agent's identity is rented from a platform account that can be revoked. Its payments depend on a human's card, billing account, or API key. Its rules run on a server controlled by its operator. Its audit trail is whatever the operator exposes. Its continuity depends on a host that can disappear.

Ethereum gives agents a different shape. An agent can own keys, hold and spend value, call contracts, leave a public audit trail, and interact with other systems through shared standards. Counterparties do not have to trust the agent's operator to report what happened, because the important state transitions can happen in public.

Some pieces of that stack are now live or emerging. ERC-8004 defines onchain registries for agent identity, reputation, and validation; the standard was created in 2025 and early deployments began in 2026. x402 uses the HTTP 402 status code to let clients, including agents, pay APIs and digital services in stablecoins without traditional accounts. Adoption numbers for both are still early and often come from project-adjacent sources, so they should be treated as directional rather than settled proof.

The important point is simpler than any one standard. Autonomous economic actors need durable identity, native payments, verifiable state, and rules that do not depend on one host. Ethereum already provides the base environment where those pieces can compose.

---

Decentralization, censorship resistance, permissionless deployment, and composability are not separate selling points. They reinforce each other. Decentralization makes censorship resistance credible. Censorship resistance makes permissionless access meaningful. Permissionless access makes composability possible. Composability turns individual apps into shared infrastructure.

Ethereum is not the best substrate for every workload. If your app only needs a fast private database, use one. If your product's value depends on neutral access, shared state, open integration, and commitments that can survive any single party, Ethereum is the platform built for that.

---

## Further Reading

- [Ethereum Foundation Checkpoint #9 (April 2026)](https://blog.ethereum.org/2026/04/10/checkpoint-9)
- [clientdiversity.org](https://clientdiversity.org/)
- [Etherscan Node Tracker](https://etherscan.io/nodetracker)
- [beaconcha.in validators](https://beaconcha.in/charts/validators)
- [Post-mortem: May 2023 mainnet finality](https://medium.com/offchainlabs/post-mortem-report-ethereum-mainnet-finality-05-11-2023-95e271dfd8b2)
- [mevwatch.info](https://www.mevwatch.info/)
- [The Block: OFAC-compliant blocks fall to 27%](https://www.theblock.co/post/230179/ethereums-ofac-compliant-blocks-fall-to-27-marking-a-drop-in-protocol-level-censorship)
- [Hegota Headliner Proposal: FOCIL (EIP-7805)](https://ethereum-magicians.org/t/hegota-headliner-proposal-focil-eip-7805/27604)
- [EIP-7805: Fork-choice enforced Inclusion Lists (FOCIL)](https://eips.ethereum.org/EIPS/eip-7805)
- [EIP-8004: Onchain Agent Identity](https://eips.ethereum.org/EIPS/eip-8004)
- [coinbase/x402 GitHub](https://github.com/coinbase/x402)
- [CoinDesk: x402 demand has not materialized](https://www.coindesk.com/markets/2026/03/11/coinbase-backed-ai-payments-protocol-wants-to-fix-micropayment-but-demand-is-just-not-there-yet)
- [WETH on Etherscan](https://etherscan.io/token/0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2)
- [Zellic: All Ethereum contracts](https://www.zellic.io/blog/all-ethereum-contracts/)
- [DefiLlama: Ethereum chain](https://defillama.com/chain/ethereum)
- [L2Beat: TVS](https://l2beat.com/scaling/tvs)
