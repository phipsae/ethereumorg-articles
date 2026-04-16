# Building on Ethereum in 2026: What's Changed

If your mental model of Ethereum was formed in 2021 to 2023, it is out of date.

The biggest shifts for builders are straightforward:

- Mainnet is much cheaper than most people still assume.
- Two major 2025 upgrades, Pectra and Fusaka, changed both capacity and wallet UX.
- Regular wallets can now expose smart-account behavior through EIP-7702.

That changes the default question a builder should ask. It is no longer just "which L2 is cheap enough?" In many cases the better question is "should this live on mainnet, or does a specific L2 give me a better distribution, liquidity, or UX advantage?"

---

## Mainnet is cheap again

The 2021 to 2023 fee regime is no longer a safe default assumption.

As of April 15, 2026, Etherscan's gas tracker showed standard gas around 0.046 gwei and estimated a swap around $0.039. That does not mean gas is always that low, but it does mean "Ethereum mainnet is too expensive for most apps" is now a stale starting point.

If you want a simple rule of thumb, use gas math instead of old folklore. At 0.1 gwei and ETH at roughly $2,350, illustrative costs look like this:

| Operation | Gas Used | Illustrative Cost |
|-----------|----------|-------------------|
| ETH transfer | 21,000 | **$0.005** |
| ERC-20 transfer | ~65,000 | **$0.015** |
| ERC-20 approve | ~46,000 | **$0.011** |
| Swap | ~180,000 | **$0.042** |
| ERC-20 deploy | ~1,200,000 | **$0.28** |

Those are examples, not guarantees. Actual costs move with ETH price, gas price, and contract complexity. The important point is the mental-model reset: mainnet fees are now low enough that many apps can sensibly start on mainnet and move to an L2 only when they need a specific L2 advantage.

### Why costs fell

Three changes matter most.

**Dencun moved rollup data onto blobs.** EIP-4844 gave L2s a separate data lane with its own fee market, so rollups stopped competing with ordinary execution traffic in the old way.

**Pectra increased blob throughput.** Pectra activated on May 7, 2025. Its EIP-7691 change raised Ethereum from 3 target / 6 max blobs per block to 6 target / 9 max.

**Fusaka and gas-limit increases added more headroom.** Fusaka activated on December 3, 2025 and brought PeerDAS to mainnet. Separately, the community raised the mainnet gas limit from 30M to 60M, and Fusaka's EIP-7935 standardized 60M as the default.

The result is that mainnet is no longer priced like a permanently congested chain.

---

## What Pectra and Fusaka changed for builders

### Pectra, activated May 7, 2025

Pectra matters to builders for four reasons.

- **EIP-7702** gave EOAs a path to delegation-based smart-account behavior.
- **EIP-7691** doubled blob throughput to 6 target / 9 max blobs per block, helping L2 fees.
- **EIP-2935** stores the last 8191 block hashes in a system contract, widening the onchain lookback window for protocols that need recent historical block data.
- **EIP-2537** added BLS12-381 precompiles, which lowers the cost of certain proof and signature operations.

### Fusaka, activated December 3, 2025

Fusaka matters more for capacity and execution safety.

- **PeerDAS** lets validators sample blob data instead of downloading it in full, which is the key unlock for higher blob counts.
- **EIP-7935** set 60M as the default gas limit, reflecting higher L1 execution capacity.
- **EIP-7825** capped any single transaction at 16,777,216 gas. Most apps will never notice, but very large deployments and monolithic multicalls do need to adapt.
- **EIP-7951** added native secp256r1, also known as P-256, verification. That makes WebAuthn and passkey-based wallet flows far more practical on mainnet.

The combination matters more than either fork in isolation: Ethereum got cheaper, rollup data got room to scale further, and the wallet model became more flexible.

---

## EIP-7702 matters, but the usual explanation is wrong

Many summaries say EIP-7702 "lets EOAs act like smart accounts for one transaction." That is not the best mental model.

EIP-7702 adds a way for an EOA to set a pointer to already deployed contract code using a new transaction type. That delegation can later be changed or reset to the null address. The private key still keeps control of the account.

That means:

- It is **not** an address migration. The user keeps the same address.
- It is **not** the same as turning the account into a Safe multisig. The EOA's original key still retains ultimate control unless the wider system is designed around that fact.
- It **does** make batching, gas sponsorship, session keys, recovery flows, and passkey-friendly UX much easier.

For dApp integration, this also means you should think in wallet interfaces, not raw authorization plumbing. Ethereum.org's guidance for Pectra 7702 points developers toward wallet standards such as ERC-5792 `wallet_sendCalls` rather than expecting dApps to directly manage 7702 authorizations themselves.

This is a meaningful shift in the account model, but it is not magic. The delegation contract becomes a serious security boundary, and bad explanations of EIP-7702 lead directly to bad product and security assumptions.

---

## What builders should update in their default playbook

**Recheck old fee assumptions.** If your docs, onboarding, or deployment heuristics still talk like Ethereum mainnet is a 10 to 30 gwei environment, they are teaching the wrong lesson.

**Choose L1 vs L2 for product reasons, not stale folklore.** L2s are still cheaper and often better for consumer-scale apps, but "mainnet is unaffordable" is no longer a good universal rule.

**Assume wallets are getting more programmable.** The clean EOA-versus-smart-contract-wallet split is breaking down. Your app should be ready for wallet-managed smart-account behavior to become normal.

---

## What's next

As of April 10, 2026, Ethereum's next named upgrade is still Glamsterdam. The major items in flight include enshrined proposer-builder separation (ePBS) and Block-level Access Lists. But the schedule is not locked: the Ethereum Foundation's April 10, 2026 checkpoint said progress was steady but slower than hoped, and that Q2 2026 looked unlikely.

After Glamsterdam, Hegota is planned to follow. As of that same April 10 checkpoint, FOCIL had been selected as Hegota's headlining feature, while account abstraction work remained under discussion for the non-headliner set.

The correct builder takeaway is not to memorize speculative dates. It is to separate shipped changes from in-flight ones.

---

## Further Reading

- [Pectra Mainnet Announcement](https://blog.ethereum.org/en/2025/04/23/pectra-mainnet)
- [Fusaka Mainnet Announcement](https://blog.ethereum.org/2025/11/06/fusaka-mainnet-announcement)
- [Protocol Priorities Update for 2026](https://blog.ethereum.org/2026/02/18/protocol-priorities-update-2026)
- [Checkpoint #9: Apr 2026](https://blog.ethereum.org/2026/04/10/checkpoint-9)
- [Pectra 7702 guidelines on ethereum.org](https://ethereum.org/en/roadmap/pectra/7702/)
- [EIP-7702: Set Code for EOAs](https://eips.ethereum.org/EIPS/eip-7702)
- [Etherscan Gas Tracker](https://etherscan.io/gastracker)
