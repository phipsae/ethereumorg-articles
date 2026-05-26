# Decentralized AI on Ethereum

The 2024 hype was that decentralized AI would replace OpenAI. The 2026 reality is more interesting and more specific.

Closed labs still train the frontier. Claude, GPT, Gemini, and the latest Llama and DeepSeek releases are produced by centralized organizations with hundred-million-dollar training budgets and synchronized clusters. None of that is changing soon. What is changing is the supporting infrastructure around AI. GPU rental, data sourcing, model serving, output verification, and machine-to-machine payment are all becoming things that can be coordinated across many independent operators rather than provisioned by a single cloud or a single lab. That coordination is what "decentralized AI" actually refers to in 2026, and it is the part where Ethereum is doing useful work.

Ethereum is not running models inside the EVM. It is the settlement, identity, and verification substrate around offchain compute. The DeAI stack splits into five layers that builders should keep separate in their heads, compute, data, training, inference, and verification. Each has different maturity, different chain affiliations, and different reasons to care about Ethereum at all.

## The compute layer

The cleanest commercial layer is GPU rental. Several decentralized compute markets are now shipping real workloads at real prices to real customers.

Akash is the longest-running of these. Its own 2025 year-in-review reports 3.13 million deployments created during the year, total network spend of about $3.15 million in 2025, an average fee per lease of about $25, and a sustained 60 percent utilization rate for accelerated compute. Active deployments at year-end were down 69 percent from 2024 even as deployment count was up 466 percent, which is what a shift from long-running containers to short AI inference bursts looks like in the data. Akash runs on its own Cosmos SDK chain. AKT is not an Ethereum-native asset and settlement does not touch Ethereum.

io.net runs on Solana with its own native token. Public reporting puts the network somewhere around 139,000 GPUs across 138 countries and an annualized revenue figure in the low tens of millions, with Training-as-a-Service launched in August 2025. Treat these specific numbers as company-reported until a current third-party audit is available, but the order of magnitude is consistent across sources.

Aethir is the network claiming the most revenue in the sector. Aethir's own 2025 wrap-up reports about $127.8 million in 2025 revenue, around 439,000 GPU containers, and 150-plus enterprise customers, with a quoted $36 million annual recurring revenue from actual enterprise purchases. These figures are not independently audited, but Aethir is the cleanest example of decentralized compute that connects to Ethereum's economic security. An Aethir vault on EigenLayer lets ATH stakers mint a liquid staking token (eATH) backed by Ethereum-restaked ETH security.

Render and Spheron sit on different chains. Render serves rendering and increasingly AI inference workloads on its own chain, with AI compute now reported as a meaningful share of total capacity. Spheron settles on Base, the Ethereum Layer 2, with the SPON token, and aggregates a node network into both AI agent infrastructure and general compute.

The pattern across compute is that most leading projects run on their own chains. A few (Aethir via EigenLayer, Spheron on Base) connect into Ethereum directly. Ethereum's role at the compute layer is less about running the marketplace and more about offering a security and liquidity backstop that some operators choose to plug into.

## The data layer

Data has become a binding constraint on AI training. The web text supply is finite, the highest-quality datasets are increasingly contested, and the EU AI Act's August 2026 compliance window adds provenance requirements that closed labs are not well positioned to satisfy on their own.

Ocean Protocol has the longest history here. Its smart contracts are deployed on Ethereum and Polygon, and its "Compute-to-Data" pattern lets a model train against an encrypted dataset without exporting the data. Vana is an EVM-compatible Layer 1 organized around DataDAOs, where each DAO defines its own "Proof of Contribution" function (financial data DAOs reward accuracy, health data DAOs reward freshness), and contributors are rewarded in the DAO's native token. Numerai's NMR is an Ethereum-native ERC-20 powering an encrypted-model tournament for hedge fund signals.

Outside Ethereum, Grass runs on Solana and coordinates residential IPs into a permissioned web scraping network. Camp Network is a separate L1 organized around creator IP licensing. Filecoin and Arweave act as the persistent storage layer for datasets and model checkpoints across the stack.

The most interesting recent development is the joint Story Protocol and OpenLedger "rights-cleared AI training" standard announced in January 2026, which embeds IP licensing into AI inference and routes royalties automatically. The standard is early and not yet widely adopted, but it is the cleanest design pattern for satisfying both creator compensation and regulatory provenance requirements without trusting a closed compliance team.

The honest summary at the data layer is that most networks are not on Ethereum, but several of the ones doing useful work (Ocean, Vana, Numerai) are. Ethereum is positioned more as a settlement and identity layer for data licensing than as the marketplace itself.

## The training layer

Decentralized model training was widely considered impossible at frontier scale until about a year ago. That has changed. The shift is real and worth being precise about, because the bull case is often overstated and the bear case is often understated.

Prime Intellect released INTELLECT-2 in April 2025, a 32B parameter model trained globally through asynchronous reinforcement learning where anyone could contribute compute permissionlessly. Inference rollout workers run on consumer hardware (a 4×RTX 3090 machine is enough to contribute), and the training architecture decouples experience collection from policy updates, so slow nodes do not block the network. Earlier INTELLECT-1 (10B parameter) demonstrated the same pattern at smaller scale. Both models, training data, and infrastructure are open source.

Nous Research's DisTrO optimizer claims roughly 1,000x to 10,000x reductions in inter-node communication, which is the technical breakthrough that lets distributed training run over residential internet rather than requiring colocated InfiniBand. 0G Labs has published a verification framework combining trusted execution environments with economic incentives, and claims a 107B parameter decentralized training run (DiLoCoX-107B). The 107B headline is currently the largest publicly claimed decentralized model, but it is a press release figure rather than a peer-reviewed result.

On Ethereum specifically, Gensyn is building a rollup dedicated to ML training and verification, with mainnet reported to have launched in April 2026 alongside Delphi, an AI-settled prediction market. Flock.io runs federated learning on Base. Pluralis is pursuing "protocol learning," a model-parallel approach where weights are sharded across participants such that no single party can extract the full model. Bittensor's Subtensor chain runs an EVM compatibility layer and Hyperlane bridges connect it to Ethereum, but Bittensor itself is not Ethereum.

The honest tension at this layer is that the frontier moves faster than decentralized infrastructure. Open-weight models from centralized organizations, including DeepSeek V3.2 (MIT-licensed, December 2025) and Meta's Llama 4 family, are competitive on capability and cost without using a token, a blockchain, or a decentralized training network. The argument that decentralization is necessary for capable open AI is weakened by this. The stronger argument is that decentralized training avoids concentration of training control in a small number of organizations, which is a structural property that closed releases of open weights do not provide. That argument should be made on its own terms, not on a capability claim.

The April 2026 dispute between Covenant AI and Bittensor (in which Covenant exited the network, sold roughly $10 million in TAO, and triggered a roughly 27 percent token drop on allegations of centralized control) is a useful reality check. Tokenized coordination aligns incentives until participants disagree about who is in charge.

## The inference and verification layer

The hardest part of decentralized AI is not running a model. It is convincing the contract on the other side that the model actually ran, and ran honestly, on the inputs claimed.

Three approaches compete. Zero-knowledge machine learning (zkML) generates a cryptographic proof that a specific computation happened. EZKL, Modulus Labs, Lagrange's DeepProve, and the Giza stack on Cairo are the active toolchains. zkML offers the strongest guarantees but proof generation is slow and the largest models that can be proved in practical time today are well below frontier scale. Optimistic machine learning (opML) borrows the design of optimistic rollups. Claims are accepted by default and a fraud proof game can challenge them. ORA's Onchain AI Oracle uses opML to deliver verified inference for LLaMA 3 and Stable Diffusion across Ethereum mainnet, Optimism, Arbitrum, Base, Polygon, and several other EVM chains. opML supports much larger models than zkML but its security is probabilistic rather than cryptographic. The third approach is trusted execution environments. Galadriel's teeML uses hardware enclaves to attest that inference ran inside a sealed boundary. TEEs are fastest and support the largest models, but the trust root is the silicon vendor.

For most production use cases in 2026, the practical answer is opML or TEE. zkML is the right choice when the trust assumption has to be cryptographic rather than economic or hardware-based, which is rare outside privacy-sensitive applications.

Verification also benefits from Ethereum's restaking economy. Lagrange's DeepProve runs as an EigenLayer Actively Validated Service, inheriting economic security from restaked ETH rather than bootstrapping its own validator set. Aethir's vault on EigenLayer follows the same pattern for compute. The broader framing under the EigenCloud umbrella (EigenDA for data availability, EigenVerify for disputes, EigenCompute for verifiable execution, EigenAI for verifiable inference) is that Ethereum-restaked ETH is increasingly the substrate that AI services rent security from.

Chainlink Functions occupies a different position. Functions runs a Decentralized Oracle Network that can call external AI APIs (including OpenAI) and bring the result onchain. It does not produce a cryptographic proof of inference, but it does decentralize the call itself across many operators. For applications that need to consume frontier closed-lab models from inside an Ethereum contract, this is the realistic option.

## What Ethereum actually provides

Across the stack, Ethereum's role is consistent. It is not the place AI runs. It is the place AI commitments settle.

Account abstraction is the precondition. ERC-4337 made smart account behavior possible without consensus changes, and EIP-7702, activated in Pectra in May 2025, closed the remaining gap by letting a plain EOA temporarily delegate to smart-contract code. Together they let an autonomous service hold its own keys, pay its own gas in stablecoins through a paymaster, and enforce its own spending limits without depending on a hosted account.

ERC-8004 specifies onchain registries for agent identity, reputation, and validation. The standard itself is still officially Draft on the EIP repository, but reference implementations have been deployed to Ethereum mainnet and L2s, and explorer trackers report tens of thousands of registrations. The current article does not litigate the agent layer in depth (a separate article in this series covers ERC-8004, x402, and ENS for agents). For the DeAI stack the relevant fact is simply that an identity primitive exists.

Payment is the other half. x402 lets an HTTP endpoint accept stablecoin payments through the existing 402 status code, which is the obvious shape of machine-to-machine commerce. Adoption has been mixed since launch. CoinDesk reported in March 2026 that demand has not yet materialized at the scale early proponents projected. Treat x402 as a working primitive whose product-market fit is still being established, not as a sure thing.

EigenLayer is the security backstop. Recent reporting puts restaked ETH on EigenLayer at roughly $18 billion across about 1,900 operators. AI-relevant Actively Validated Services include zkML coprocessors (Lagrange), compute vaults (Aethir), and a growing list of verification and inference services under the EigenCloud umbrella. The pattern is that an AI service can rent Ethereum-grade economic security rather than build its own.

This is what the Ethereum Foundation's posture toward AI has been pointing at. Vitalik's "d/acc: one year later" essay (January 2025) frames Ethereum's role as a decentralizing counterweight to AI concentration, not as a competitor to AI labs. The argument is structural. The closed labs will keep training the frontier. The question is whether the rails for everything around them (payment, identity, verification, ownership, data licensing, compute settlement) stay in the hands of a few platforms or run on infrastructure no single party controls.

## What does not work yet

The honest limits should be stated plainly.

Decentralized training has produced credible 30B to 100B parameter models. It has not produced a frontier model. The training-time gap between synchronized clusters with NVLink-class interconnect and globally distributed nodes over residential internet is real, even with 1,000x communication-efficiency optimizations. Crossover for serious frontier work is at best 12 to 24 months away, possibly longer.

Verifiable inference is still slow at scale. zkML proofs for transformer attention have moved from "barely feasible" to "feasible for small models" over the past year, but proving frontier-scale LLM inference in real time is not yet possible. opML works at scale but trades cryptographic security for an economic dispute window. TEEs work but inherit the silicon vendor's threat model.

Tokenized coordination is fragile under stress. The Covenant exit from Bittensor in April 2026 showed that even working incentive systems can break when participants disagree about control. Token mechanics are not a substitute for institutional clarity about who decides what.

Open-weight models from centralized labs are catching up on capability without a blockchain. This is the strongest honest critique of the DeAI investor thesis. The argument for decentralized AI infrastructure cannot rest on "you need a token to coordinate open AI." It has to rest on the properties Ethereum specifically provides, namely settlement, identity, payment, verification, and resistance to capture by any single operator. Those properties are real, but they are not the same property as "capable open weights," and conflating them weakens the case.

Regulatory environment is orthogonal so far. The EU AI Act enters compliance enforcement in August 2026 with no preferential treatment for decentralized infrastructure. The US executive order on AI (December 2025) does not mention blockchain at all. Provenance and audit-trail systems built on Ethereum may eventually become regulatory advantages, but they are not yet.

## Conclusion

Decentralized AI in 2026 is not one thing. It is a stack of independent layers, only some of which are on Ethereum, only some of which work yet, and only some of which need a blockchain at all. The compute layer has real customers. The data layer is consolidating around provenance and creator compensation. The training layer has shipped serious models but has not closed the frontier gap. The inference and verification layer is where Ethereum is most directly useful, through opML, restaked security, and account abstraction.

What Ethereum provides is not a model. It is a way for everything around a model (the GPU rental, the data licensing, the inference attestation, the payment for an API call, the identity of an autonomous service) to settle on rails no single operator controls. That is a small claim and a defensible one. It is also enough to build on.

## Further Reading

- [Vitalik Buterin: d/acc: one year later](https://vitalik.eth.limo/general/2025/01/05/dacc2.html)
- [EIP-7702: Set Code for EOAs](https://eips.ethereum.org/EIPS/eip-7702)
- [ERC-8004: Trustless Agents (Draft)](https://eips.ethereum.org/EIPS/eip-8004)
- [Akash 2025 Year in Review](https://akash.network/blog/akash-2025-year-in-review/)
- [Prime Intellect: INTELLECT-2](https://www.primeintellect.ai/blog/intellect-2)
- [Nous Research: DisTrO](https://github.com/NousResearch/DisTrO)
- [ORA: Onchain AI Oracle](https://docs.ora.io/doc/onchain-ai-oracle-oao/onchain-ai-oracle)
- [opML paper](https://arxiv.org/abs/2401.17555)
- [Survey of zero-knowledge proof based verifiable ML](https://arxiv.org/abs/2502.18535)
- [EZKL: state of the project](https://blog.ezkl.xyz/post/state_of_ezkl/)
- [Aethir 2025 wrap-up](https://ecosystem.aethir.com/blog-posts/aethirs-2025-wrap-up-decentralized-gpu-cloud-milestones)
- [0G Labs: verification framework for decentralized training](https://0g.ai/blog/why-verification-matters-decentralized-ai-training)
- [Story Protocol and OpenLedger: rights-cleared AI training](https://www.prnewswire.com/news-releases/story-protocol-and-openledger-launch-new-standard-for-rights-cleared-ai-training-and-automatic-creator-payments-302673803.html)
- [Vana whitepaper](https://www.vana.org/posts/vana-whitepaper)
- [Ocean Protocol docs](https://docs.oceanprotocol.com/discover/faq)
- [Chainlink Functions](https://docs.chain.link/chainlink-functions)
- [CoinDesk: decentralized AI is in a trough](https://www.coindesk.com/business/2026/02/11/decentralized-ai-is-in-a-trough-but-real-opportunities-are-emerging-crypto-vcs-say)
- [CoinDesk: x402 demand has not materialized](https://www.coindesk.com/markets/2026/03/11/coinbase-backed-ai-payments-protocol-wants-to-fix-micropayment-but-demand-is-just-not-there-yet)
