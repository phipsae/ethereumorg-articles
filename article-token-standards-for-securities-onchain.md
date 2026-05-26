# Ethereum token standards for issuing securities onchain

Tokenized securities now sit on Ethereum at meaningful scale. RWA.xyz tracks roughly $12.77 billion in real-world assets on Ethereum mainnet and $1.14 billion on Ethereum Layer 2 networks. Treasury funds, private credit, equity tranches, green bonds, and trade finance instruments are issued today by banks, asset managers, and registered transfer agents using a small set of token standards that did not exist five years ago.

The choice of standard is not cosmetic. Unlike a stablecoin or a governance token, a security is a liability contract. The issuer owes obligations to the holder under specific legal frameworks, and the holder's right to transfer is bounded by jurisdiction, accreditation, lock-up windows, and corporate-action history. Tokenization works only if the standard encodes those obligations into the contract itself, so compliance is enforced at the protocol layer instead of reconciled against an offchain spreadsheet after the fact.

This guide is for two readers. A business reader scoping a tokenization initiative can stop after the opening sections and the decision frame, with a clear picture of which families of standards solve which job. A technical reader (a Solidity team, a transfer agent's engineering lead, a regulated issuer's vendor) can continue into the per-standard sections, which cover the mechanism, the trade-off, and a production example for each.

## How to read this guide

The standards split by job, not by chronology. Skip to the section that matches your asset.

- Simplicity and speed for stablecoins and money market funds: **Permissioned ERC-20**
- Private markets, identity portability, and EU passporting: **ERC-3643**
- Modular open architecture across multiple regulatory regimes: **CMTAT**
- DeFi connectivity for otherwise restricted assets: **ERC-4626**
- Settlement delays and T+2 cycles: **ERC-7540**
- Legacy standards still in production: **ERC-1400** and **ERC-1404**
- High-throughput marketplaces with many small instruments: **ERC-1155**
- Onchain fixed income with native coupon and maturity logic: **ERC-7092**

## Choosing a standard starts with the asset and its compliance posture

Three questions narrow the field. What is the asset (stablecoin, money market share, fund interest, equity, bond, invoice, real estate)? Where will it be sold (US Reg D, Reg S, EU MiCA, MiFID II, Swiss DLT Act, Singapore Project Guardian)? Will it need to plug into DeFi protocols, or sit in segregated custody? The standards covered below map cleanly to combinations of those three answers. The rest of this guide walks them in order of adoption, starting with the simplest and most widely used.

## Permissioned ERC-20

**Mechanism:** Standard ERC-20 token with an allowlist check added to the transfer function. The token contract queries an internal or external registry of approved wallet addresses before any transfer.
**Use case:** Stablecoins, tokenized money market funds, and other high-frequency assets where maximum wallet and exchange compatibility matters more than granular onchain state.
**Trade-off:** No native support for partitioned balances (vesting versus free-trading shares), no native corporate-action primitives, and no portable identity across tokens from the same issuer.

### How it works

ERC-20 is the most widely implemented token interface on Ethereum. The standard defines six functions (`transfer`, `approve`, `transferFrom`, `totalSupply`, `balanceOf`, `allowance`) that any wallet, exchange, or smart contract can call without custom integration. That universality is its commercial value.

The permissionless transfer logic is also its compliance problem. Possession of the private key gives the right to send tokens to any address, including a sanctioned entity, a retail investor in a prohibited jurisdiction, or an unverified wallet. The standard workaround is a permissioned wrapper. A registered transfer agent maintains an allowlist of KYC-approved wallets, and the token's transfer function rejects any transaction whose destination is not on the list. The allowlist is mutable, so investors who pass new screening can be added, and addresses linked to compromised holders can be removed.

### In production: BlackRock BUIDL

BlackRock and Securitize launched the BlackRock USD Institutional Digital Liquidity Fund (BUIDL) on Ethereum mainnet in March 2024. The fund holds approximately $1.26 billion in assets under management at the time of writing. It targets a stable $1 net asset value and distributes daily yield from US Treasury bills, notes, and other Treasury-backed obligations by minting fresh BUIDL tokens into holder wallets.

BUIDL is a permissioned ERC-20. Securitize, as the registered transfer agent, performs KYC and AML offchain. Approved investors' wallet addresses are written to an allowlist inside the BUIDL contract. Every transfer call checks the allowlist before executing. The result is a fund that settles peer-to-peer on Ethereum (any time of day, any day of the week), offers daily yield distribution, and supports flexible custody, but cannot interact directly with DeFi protocols whose smart contracts are not on the allowlist. BlackRock and Securitize address that gap separately through sBUIDL, an ERC-4626 wrapper covered later in this guide.

## ERC-3643: identity-centric compliance

**Mechanism:** Three contracts working together. A token contract overrides the transfer function to call out to an identity registry and a compliance validator before any transfer. Eligibility lives onchain in a separate ONCHAINID smart contract per investor; personal data does not.
**Use case:** Private market securities, regulated equities, EU markets, and any issuance program where one verified identity should grant access to many products from the same issuer (passporting).
**Trade-off:** Adds gas overhead from external calls on every transfer. Depends on the ONCHAINID ecosystem and its tooling, currently maintained by a coalition of issuers, custodians, and infrastructure providers rather than a single vendor.

### How it works

ERC-3643 (formerly the T-REX protocol) decouples the token from the compliance logic. Instead of an allowlist living inside each token, three contracts coordinate.

The token contract is a modified ERC-20. Its overridden transfer function pauses to call the identity registry and the compliance validator before executing. The identity registry checks whether both sender and receiver hold valid ONCHAINIDs (smart contract identities) with the credentials the token requires, for example "accredited investor" or "EU-passportable." The compliance validator runs the dynamic rules: maximum investor count, prohibited cross-jurisdiction transfers, lock-up windows, holding caps. Only if both pass does the token contract execute the transfer.

ONCHAINID stores cryptographic attestations, not personal data. When an investor passes KYC with a trusted issuer or third-party identity provider, that party issues a verifiable claim that lives in the investor's ONCHAINID contract. The token contract checks the claim. Names, social security numbers, and passport scans never touch the chain.

The architecture's commercial value is identity portability. In a permissioned ERC-20, an investor who buys five products from the same issuer is added to five separate allowlists. In ERC-3643, one ONCHAINID grants access to every product whose token contract references the same registry. The ERC-3643 Association, a non-profit coalition supporting the standard, counts more than 130 member organizations. The Depository Trust and Clearing Corporation has integrated ERC-3643 into its digital asset platform.

### In production: ABN AMRO green bond

In September 2023, Dutch bank ABN AMRO issued a digital green bond on Polygon (an Ethereum Layer 2) to raise €5 million for real estate manager Vesteda's green asset refinancing. The bond was issued as an ERC-3643 token using infrastructure from Tokeny.

ABN AMRO acted as the claim issuer. KYC and AML were performed offchain; once an investor was verified, the bank wrote a cryptographic claim to that investor's ONCHAINID. From then on, every transfer of the bond token was validated automatically against the registry. The result was a public-network bond issuance with a real-time, immutable register of holders, where eligibility was enforced at the protocol layer rather than reconciled by a registrar after the trade.

## CMTAT: modular open architecture

**Mechanism:** Open-source modular smart contract framework. The core contract handles ownership and basic transfer logic. Heavier compliance is delegated to an external RuleEngine, and corporate actions are coordinated through a SnapshotEngine.
**Use case:** Regulated equity, debt, and structured products in jurisdictions with detailed legal frameworks for tokenized securities, including Switzerland (DLT Act), Hong Kong, and Germany (eWpG). Specialized variants exist for bonds with onchain coupon and maturity data.
**Trade-off:** Heavier contract bytecode than ERC-3643 or permissioned ERC-20. The reference implementation pushes compliance into external modules to manage code size, which adds integration surface.

### How it works

The Capital Markets and Technology Association Token (CMTAT) was developed to bridge regional securities law and onchain issuance. Originally built for Swiss regulations, the framework has been extended to support German and other European frameworks.

CMTAT starts with an ERC-20 core and layers on a RuleEngine contract that enforces dynamic transfer restrictions, accredited investor checks, holder caps, and similar rules. Conceptually it plays the same role as the ERC-3643 compliance validator, but the integration is modular: an issuer composes the RuleEngine from smaller modules rather than inheriting a fixed compliance stack.

A SnapshotEngine records balances at specific block heights for corporate actions (dividends, voting, capital calls) without pausing the contract. Yield distribution and shareholder votes reference the snapshot, not the live balance.

Optional integrations include ERC-1363 for post-transfer callbacks and atomic settlement, and ERC-2771 for meta-transactions, so the issuer can pay gas on behalf of investors. A UUPS proxy pattern lets the issuer upgrade contract logic to comply with new regulations without changing the token address or disrupting balances.

### In production: UBS cross-border repurchase agreement

In November 2023, UBS, SBI, and DBS Bank executed the first cross-border repurchase agreement using a natively issued digital bond on Ethereum. The transaction was part of the Monetary Authority of Singapore's Project Guardian, and used the CMTAT framework so the bond complied with Swiss law while settling on public infrastructure across Japan, Singapore, and Switzerland.

The repo interacted with nine smart contracts to settle the bond purchase, the repurchase, and the redemption autonomously. The pilot demonstrated CMTAT's ability to coordinate multi-leg, multi-jurisdiction workflows on a public chain, with every leg auditable in real time.

## ERC-4626: DeFi connectivity through tokenized vaults

**Mechanism:** Standard interface for yield-bearing vaults that wraps an underlying asset and issues fungible share tokens. The vault implements ERC-20 itself, so the share token is composable with any DeFi protocol that accepts ERC-20.
**Use case:** Connectivity layer between permissioned, identity-restricted securities and the open DeFi ecosystem. Lets a restricted asset back a freely-tradable share token without breaking the underlying compliance model.
**Trade-off:** Assumes atomic settlement. Deposits and withdrawals execute instantly, which does not match the operational reality of assets with T+1 or T+2 settlement cycles. Extensions like ERC-7540 exist for those cases.

### How it works

ERC-4626 is not a security token standard. It is an adapter. A vault contract accepts deposits of an underlying asset and mints standardized share tokens in return. The exchange rate between shares and assets is dynamic: as the vault accrues yield, each share becomes worth more underlying. Burning a share redeems the user's proportional slice of the pool.

Because the share token is itself a standard ERC-20, it can be used anywhere an ERC-20 is accepted: as collateral in lending markets, as a base asset in automated market makers, in yield strategies. The underlying restricted asset stays inside the vault and never touches the open ecosystem directly. That separation is the design.

### In production: BlackRock sBUIDL

In May 2025, BlackRock and Securitize launched sBUIDL using Securitize's sToken framework, built on ERC-4626. The mechanism is straightforward.

An allowlisted BUIDL holder deposits BUIDL into the sBUIDL vault. Because the vault is itself a Securitize-recognized compliant entity, it can hold BUIDL on behalf of the depositor. In exchange, the depositor receives sBUIDL share tokens. sBUIDL is a standard ERC-20 and is composable with DeFi protocols that accept it.

As the underlying BUIDL fund distributes daily yield by minting new BUIDL into the vault, the vault's holdings grow. The total supply of sBUIDL does not change. The exchange rate shifts, so each sBUIDL becomes worth slightly more BUIDL each day. When a depositor redeems, the vault burns their sBUIDL and returns the proportional slice of the pooled BUIDL, including accrued yield.

The investor gets a composable, dynamic share token that accrues Treasury bill yield, can be posted as collateral in lending markets, and is fully redeemable for the underlying allowlisted BUIDL. The compliance model is untouched, because every wallet that interacts with the underlying BUIDL is still on the allowlist; the open ecosystem interacts only with sBUIDL.

## ERC-7540: asynchronous vaults for T+2 assets

**Mechanism:** Extension of ERC-4626 that splits deposits and withdrawals into a two-step Request and Claim flow. The vault accepts requests, processes underlying settlement (which may take hours or days), and only mints or redeems shares once settlement completes.
**Use case:** Tokenized funds backed by assets with real settlement cycles: private credit, real estate, corporate bonds, and Treasury bill funds whose underlying instruments settle T+1 or T+2 through traditional rails.
**Trade-off:** Loss of atomicity. A user submits a redemption request and waits, instead of receiving cash instantly. Operationally closer to a traditional fund subscription than a DeFi swap.

### How it works

ERC-4626 assumes that the vault can settle a deposit or redemption in the same block. For private credit, real estate, or any fund whose underlying assets settle through banks rather than blockchains, that assumption breaks. If an investor redeems $1 million from a private credit fund, the manager often needs days to sell the underlying instruments and wire the cash. A synchronous vault would either lock up or stay perpetually under-collateralized.

ERC-7540 splits the flow. A user calls `requestDeposit` or `requestRedeem`. The vault records the intent but does not move tokens yet. The vault manager (or an automated system) processes the underlying settlement: selling a Treasury bill, drawing down a credit line, completing a real estate sale. Once settlement clears, the vault updates the request to "claimable." The user then calls `claimDeposit` or `claimRedeem` to finalize and receive their shares or assets.

The vault never promises liquidity it has not yet settled. That property is what makes the standard usable for assets that traditional finance handles in cycles longer than one Ethereum block.

### In production: Centrifuge and JTRSY

Centrifuge, an RWA tokenization platform with over $951 million in assets under management on Ethereum, co-authored ERC-7540 and uses it to power its V3 protocol. Centrifuge pools are structured into tranches (junior and senior), where each tranche is deployed as its own ERC-7540 vault. Instead of processing every deposit instantly, vaults batch `requestDeposit` and `requestRedeem` calls into 24-hour epochs. At the end of an epoch, the vault receives an updated net asset value for the underlying pool and processes all pending requests simultaneously at that price. Batching at a precise NAV prevents arbitrage between onchain pricing and offchain reality.

One example is the Janus Henderson Anemoy Treasury Fund (JTRSY), a tokenized fund with approximately $504 million in assets under management that invests in short-term US Treasury bills. The underlying Treasuries settle on traditional banking rails (one to two business days). The fund cannot support instant redemptions without taking on liquidity mismatch risk. With ERC-7540, an investor submits a redemption request that locks their tokens in a pending state. Once the fund manager sells the necessary Treasuries and the cash settles offchain, the vault marks the request claimable. The investor pulls USDC at the correctly calculated NAV.

## Legacy: ERC-1400 and ERC-1404

ERC-1400 and ERC-1404 are early security token standards that saw real-world adoption but were eventually superseded by lighter, more modular designs. Both remain in draft status on the EIP repository. Their concepts persist in newer standards even where the standards themselves are no longer the default choice.

ERC-1400 was an umbrella standard combining four sub-proposals: ERC-1410 (partially fungible tokens), ERC-1594 (core security token), ERC-1643 (document management), and ERC-1644 (controller operations). Its most significant innovation was partitions, which allowed a single token contract to track different states of a balance (for example, 60 vested and 40 locked shares for one investor) without breaking overall fungibility. ERC-1400 also introduced `transferWithData`, which allowed senders to attach context (compliance certificates, payment references) to a transfer in a single atomic call. Those ideas were genuinely useful. The problem was cost: maintaining partitions and document metadata inside the token contract meant heavy bytecode and expensive transfers. Later standards (ERC-3643, CMTAT) moved the heavy logic to external registries and validators, keeping the token contract lean.

ERC-1404 was a simpler restricted-token extension developed by the platform Tokensoft. Its main contribution was solving the "blind failure" problem of ERC-20 reverts. The functions `detectTransferRestriction` and `messageForTransferRestriction` let wallets and exchanges query why a transfer would fail before submitting it, and return a human-readable reason ("Sender not KYC verified," "Lock-up period active"). ERC-1404 notably powered the INX Limited IPO in 2021, the first SEC-registered initial public offering of a digital security, which raised approximately $85 million from over 7,200 investors. The standard fell out of common use because its restriction logic was typically hard-coded at deployment, making it difficult to adapt to changing regulations.

Both standards are worth understanding because their core innovations (partitions, document binding, human-readable restriction reasons, transfer context) are now embedded in the modular successors. ERC-1400's document management standard (ERC-1643) was adopted into the CMTAT framework directly.

## Emerging standards for specialized use cases

### ERC-1155 for high-throughput marketplaces

**Mechanism:** Multi-token standard. A single contract can manage many distinct token IDs (fungible or non-fungible) with batch-transfer support.
**Use case:** Real-world asset marketplaces with many small instruments: invoice factoring, trade finance, fractional real estate. Significant gas savings on batch operations at scale.
**Trade-off:** More complex developer ergonomics than ERC-20, and weaker tooling for investor-facing custody and exchange listings.

ERC-1155 is best known as the standard behind most NFT marketplaces, but it has a separate role in tokenized debt. Where ERC-20 would require a separate contract per instrument, ERC-1155 can manage thousands of unique instruments in a single contract and batch their transfers. Polytrade, an RWA marketplace, has used ERC-1155 to tokenize invoice financing, where every invoice has a different debtor, maturity, and yield profile but trades frequently enough that gas-efficient batch operations are commercially material.

### ERC-7092 for fixed-income bonds

**Mechanism:** Emerging standard (currently draft) specifically for bonds. Embeds coupon rate, maturity date, principal, and currency in contract storage as standardized variables. Native functions calculate accrued interest and process callable and puttable redemptions.
**Use case:** Onchain fixed income where bond data needs to be machine-readable for pricing and protocol integration. Designed to interface with external identity registries (ONCHAINID, ERC-725) for compliance.
**Trade-off:** Lacks the partition and lifecycle granularity of the ERC-1400 family. Not suited to instruments with complex equity-like state, which still need ERC-3643 or CMTAT.

ERC-7092 makes bond data first-class onchain. In a typical tokenized bond using ERC-20 or ERC-3643, the financial terms (coupon, maturity, principal) live in an offchain prospectus linked from the token. With ERC-7092, those terms are smart contract variables that any protocol can read. A DeFi protocol can compute accrued interest from the standard's native function, price the bond against a yield curve, and process callable or puttable redemptions through standardized callbacks. That removes the centralized oracle from the loop for bond pricing, which is one of the bottlenecks for automated bond markets today.

## What does not work yet

Cross-standard composability is still weak. An ERC-3643 fund and an ERC-7540 vault from different issuers do not compose without bespoke wrappers. The sBUIDL pattern works because BlackRock and Securitize built the wrapper themselves. A generalized adapter layer for restricted assets into DeFi does not exist.

Identity registries are not yet portable across issuers. An investor verified for one ERC-3643 program does not automatically gain access to another issuer's products on the same standard, even though the architecture allows it. Cross-issuer identity portability requires coordination on credential schemas and trust relationships that are still ad hoc.

Settlement coupling with traditional rails is custom per protocol. ERC-7540 provides the asynchronous flow primitive, but the actual NAV calculation, the timing of settlement windows, and the bridging between onchain requests and offchain banking operations are bespoke implementations. There is no shared infrastructure for fund administrators to plug into.

Regulatory environment varies sharply by jurisdiction. CMTAT works in Switzerland and parts of Europe because the legal frameworks recognize ledger-based ownership. The US has no equivalent at the federal level; tokenized securities under US law are still securities with all the offchain compliance overhead that implies. The standard you pick is constrained by the law that governs the asset, not the other way around.

Tooling for issuer operations is uneven. Vendors like Securitize, Tokeny, Taurus, and similar provide end-to-end stacks for specific standards, but switching between standards or self-hosting the issuance pipeline remains heavy engineering work. Smaller issuers face a higher all-in cost than the contract code itself suggests.

## Choosing on Ethereum and its L2s

The decision compresses to a few questions. If the asset is a stablecoin or a daily-NAV fund and broad compatibility matters more than fine-grained state, **permissioned ERC-20** is the default. If the asset is a private market security with EU regulatory exposure or with multiple products from one issuer, **ERC-3643** is the standard. If the asset is regulated equity or debt under Swiss or German law, or needs modular composition of compliance modules, **CMTAT** is the framework. If the asset has real settlement cycles, layer **ERC-7540** on top of the underlying standard to expose it to DeFi without breaking the underlying compliance model. For bonds with rich onchain financial data, **ERC-7092** is the emerging option. For high-volume marketplaces with many small instruments, **ERC-1155** offers gas efficiency that ERC-20 cannot match.

Ethereum and its Layer 2s currently host more tokenized real-world assets than any other blockchain ecosystem. The standards above are the load-bearing primitives behind that scale. They are not finished work. Cross-standard composability, identity portability, and shared settlement infrastructure are all still maturing. But for an issuer choosing today, the question is no longer whether Ethereum can support a regulated securities issuance. It is which of these standards fits the asset and the jurisdiction it will operate in.

If you are considering issuing securities on Ethereum or its Layer 2 networks, the Ethereum Foundation's Enterprise Adoption team can help identify potential paths forward, provide technical support, and connect you with industry leaders.

## Further Reading

- [ERC-3643 Association](https://www.erc3643.org/)
- [ERC-3643: Standard for tokenized securities](https://eips.ethereum.org/EIPS/eip-3643)
- [BlackRock and Securitize: BUIDL fund](https://securitize.io/learn/press/securitize-launches-blackrocks-first-tokenized-fund-buidl-on-the-ethereum-network)
- [Securitize: sBUIDL and the sToken framework](https://securitize.io/learn/press)
- [CMTAT framework on GitHub](https://github.com/CMTA/CMTAT)
- [UBS, SBI, DBS cross-border repo on Project Guardian](https://www.ubs.com/global/en/media/display-page-ndp/en-20231102-uniicus.html)
- [ABN AMRO digital green bond](https://www.abnamro.com/en/news)
- [Tokeny: ERC-3643 infrastructure](https://tokeny.com/)
- [ERC-4626: Tokenized vaults](https://eips.ethereum.org/EIPS/eip-4626)
- [ERC-7540: Asynchronous tokenized vaults](https://eips.ethereum.org/EIPS/eip-7540)
- [Centrifuge V3 protocol](https://centrifuge.io/)
- [Janus Henderson Anemoy Treasury Fund (JTRSY)](https://app.centrifuge.io/)
- [ERC-1400 security token standards (Draft)](https://github.com/ethereum/EIPs/issues/1411)
- [ERC-1404 simple restricted token (Draft)](https://erc1404.io/)
- [ERC-1155 multi-token standard](https://eips.ethereum.org/EIPS/eip-1155)
- [Polytrade RWA marketplace](https://polytrade.finance/)
- [ERC-7092 financial bonds standard (Draft)](https://eips.ethereum.org/EIPS/eip-7092)
- [RWA.xyz: tokenized real-world asset tracker](https://app.rwa.xyz/)
- [DTCC digital asset platform](https://www.dtcc.com/digital-assets)
