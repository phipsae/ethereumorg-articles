# Critical read: "We need better wallets - A charge for builders"

## The biggest issue: audience drift

Title promises "a charge for builders." First half is a consumer privacy explainer; the builder argument starts ~40% in and is the weakest-developed section. Either cut the consumer setup to two short paragraphs and lead with the thesis ("the wallet is the only layer that sees every dApp, address, and RPC request, so it must carry privacy"), or split into two posts. Right now neither audience gets the strongest version.

## Substantive problems

1. **Tornado Cash gets one sentence with zero regulatory context.** This is the article's most serious omission. An EF-branded piece telling builders to integrate a protocol whose original deployer is mid-litigation, with sanctions only partially lifted, cannot just say "it's being added." Builders shipping this in apps will face real legal questions. Either treat it seriously or leave it out of the launch story.

2. **Privacy Pools mischaracterized.** "Same as Tornado Cash but with a compliance layer on top" misses what makes Privacy Pools architecturally interesting (association sets / proof-of-innocence). Builders reading this need the actual model, not a one-line gloss.

3. **PQ gas costs framed misleadingly.** 1.7M-8.4M gas called "real but tractable." 8.4M is more than half an L1 block. On mainnet today this is not a usable account, only meaningful on L2s or future EVM optimizations. Say that.

4. **"Completely unlinkable" overstates the guarantee.** Tornado Cash withdrawals to fresh addresses can be linked by timing, amounts, gas patterns, and RPC IP. The article already raised RPC linkage earlier, then forgets it here.

5. **Off-chain analogy is wrong in a way that builders will notice.** "Your neighbor knows... where you ate lunch yesterday, what medications you buy" is not what on-chain leaks. On-chain leaks salary streams, token holdings, NFT collections, DeFi positions, DAO votes, ENS-tied identity. Use accurate examples and the case is stronger, not weaker.

6. **Light client section conflates two threats.** Trustless state verification stops a malicious RPC from feeding you false state. It does not stop the same RPC from logging your IP against your queries. The article structurally implies it covers the RPC privacy concern from earlier; it does not.

7. **"There's no way to link your wallets to one another"** in the fresh-wallet-per-dApp section. Too strong. Browser fingerprint, RPC IP, timing, and on-chain funding patterns remain unless the shielded-funding step is genuinely unlinkable, which depends on anonymity set size, never discussed.

8. **"Working in your wallet in an afternoon."** Optimistic to the point of misleading. Integrating real shielded balances into a production wallet (key management, balance UI, gas, error states, recovery) is multi-week work. This line will burn credibility with the exact audience you're recruiting.

9. **Internal contradiction.** The piece argues the app layer is the wrong place for privacy, then advocates a per-dApp wallet flow which is essentially app-scoped privacy. Worth resolving: the wallet provides the primitive, dApps inherit it. Right now it reads as a tension.

## Missing for a builder audience

- Anonymity-set sizes for Railgun and current Privacy Pools deployments. Privacy guarantee is entirely a function of this.
- Cost to end users (gas overhead, shield/unshield friction).
- Threat model. Chainalysis, nation-state, opportunistic robber, abusive ex: each demands different defenses. The article gestures at all of them without committing.
- Who builds and maintains Kohaku, and what EF's support commitment is. Wallet teams adopting an SDK want to know the bus factor.
- Roadmap items: which have RFCs / working code vs aspirations.

## Style and tone

- "Yikes!", "If you are like me", "Yes, you read that correctly", "let your imagination do the rest", "some fancy zero knowledge cryptography": these undercut the article's authority with the builder audience. A consumer post can be breezy; a builder charge should be tight.
- Em-dashes throughout. If house style permits, fine; if not, sweep them.
- "Wallets... your only way to interact with Ethereum" is false (CLI, scripts, custodians, smart contracts calling each other). Soften to "the primary way."
- "Ethereum only gets two hard forks a year" is presented as a hard fact; recent cadence has been slower historically. Worth a less brittle phrasing.

## Smaller things

- "ETH (via WETH)" is technically OK but readers will note ETH itself is not an ERC20; the parenthetical does the work but could be tighter.
- "Centralized RPC" is editorial; some RPCs are decentralized (e.g. light-client networks, multi-provider routers).
- "The starting point is the docs at ethereum.github.io/kohaku/getting-started", verify the URL exists and is the canonical entry point; this is the one piece of the post most likely to rot.
- Reference links at the end have titles but no URLs in the text as shown; assume the published version fills them in.

## The strongest argument is buried

The thesis ("wallet sees every dApp, every address, every RPC request, it is the only layer that can carry a coherent privacy story") is the single sharpest paragraph in the piece and is given one short section. Lead with this. Make the rest support it.
