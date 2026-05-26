# "Ethereum token standards for issuing securities onchain" Reference Doc (May 2026)

Compiled 2026-05-26. Numeric claims in the article are listed here with the source URL and a qualifier flag (primary, company-reported, press release, or secondary). Where a figure is date-sensitive (AUM, market totals, member counts), the qualifier explicitly says so. The article should be reverified against this dossier before publication, and any AUM or RWA-volume figure should be refreshed against a live data source.

## Framing and scope

**1. Article job.** Help a builder or issuer choose a token standard for tokenized securities, starting from a business framing (why standards matter, what the landscape looks like) and moving into per-standard technical depth. Audience is bifurcated: a non-technical reader stops after the decision frame, a technical reader continues into the standards detail. The article is not a Solidity tutorial; code-level guidance is deferred to vendor documentation.

**2. Universe of standards in scope.** Permissioned ERC-20, ERC-3643, CMTAT, ERC-4626, ERC-7540, ERC-1400 (and its sub-EIPs 1410/1594/1643/1644), ERC-1404, ERC-1155, ERC-7092. Out of scope deliberately: ERC-1820 (interface registry), ERC-2222 (funds distribution), ERC-3525 (semi-fungible), and other proposals with limited production adoption.

**3. Standards explicitly de-prioritized.** ERC-1400 and ERC-1404 are presented as legacy with their pioneering ideas now embedded in ERC-3643 and CMTAT. This framing is correct as of May 2026 but should be revisited annually; if a new wave of ERC-1400 adoption appears (for example, from a major US bank's tokenization program), the framing changes.

## Market scale

**4. RWA on Ethereum mainnet and L2.** Article cites approximately $12.77B on Ethereum mainnet and $1.14B on Ethereum L2. Source: RWA.xyz aggregator, https://app.rwa.xyz/ . Qualifier: aggregator figure, date-sensitive. The exact numbers shift daily. Confirm against the live dashboard the day before publication. Treat as "as of late May 2026" if used as stated.

**5. Ethereum dominance among RWA chains.** Article asserts Ethereum and its L2s host more tokenized RWAs than any other chain. Source: RWA.xyz cross-chain breakdown. Qualifier: aggregator claim. Holds as of May 2026; the Stellar and Plume challenger ecosystems are growing fast enough that this should be reconfirmed at publication.

## Permissioned ERC-20 and BUIDL

**6. ERC-20 specification.** Six standard functions (`transfer`, `approve`, `transferFrom`, `totalSupply`, `balanceOf`, `allowance`). Source: https://eips.ethereum.org/EIPS/eip-20 . Primary, EIP itself. Not date-sensitive.

**7. BUIDL launch.** BlackRock USD Institutional Digital Liquidity Fund launched on Ethereum mainnet in March 2024 with Securitize as the registered transfer agent. Source: https://securitize.io/learn/press/securitize-launches-blackrocks-first-tokenized-fund-buidl-on-the-ethereum-network . Primary (Securitize's own press release). Launch date is settled.

**8. BUIDL AUM (~$1.26B).** Article cites approximately $1.26B AUM at the time of writing. Source: Securitize and BlackRock press materials; RWA.xyz tracker. Qualifier: company-reported and aggregator-tracked, date-sensitive. AUM fluctuates daily with subscriptions and redemptions. Confirm at publication; if a more recent figure is materially different, update.

**9. BUIDL yield mechanism.** Article describes BUIDL as distributing daily yield by minting fresh tokens to holder wallets. Source: BlackRock and Securitize product materials. Primary. The rebasement-via-minting design is the documented mechanism, not an inference.

## ERC-3643 and ABN AMRO

**10. ERC-3643 EIP status.** Standard published as ERC-3643, formerly T-REX protocol. Source: https://eips.ethereum.org/EIPS/eip-3643 . Primary. The "Final" or "Last Call" status of the EIP should be reconfirmed against the EIP repository before publication; if it has moved between Draft and Final, the article phrasing should match.

**11. ERC-3643 Association member count (>130).** Source: https://www.erc3643.org/ (association membership page). Qualifier: association's own count, company-reported. The figure has been growing; reconfirm at publication.

**12. DTCC integration.** Article cites DTCC's integration of ERC-3643 into its digital asset platform. Source: DTCC public statements and Tokeny press materials referencing the integration. Qualifier: press-release-grade. The strongest specific claim ("DTCC has adopted ERC-3643 to automate the processing of tokenized securities") came from the user's draft and should be reconfirmed against a primary DTCC source before publication. If the language is too strong, soften to "DTCC has integrated ERC-3643 in its digital asset platform."

**13. ABN AMRO green bond.** September 2023 issuance on Polygon, €5M raised for Vesteda green asset refinancing, ERC-3643 token via Tokeny infrastructure. Source: ABN AMRO and Tokeny press materials from September 2023. Qualifier: company-reported but settled; the issuance is documented and not date-sensitive.

**14. ABN AMRO claim issuer role.** The bank performed KYC offchain, then issued cryptographic claims to investor ONCHAINIDs, after which every transfer was validated automatically. Source: Tokeny case study on the deployment. Primary mechanic, documented in vendor materials.

## CMTAT and UBS Project Guardian

**15. CMTAT framework provenance.** Developed by the Capital Markets and Technology Association in Switzerland, extended for German eWpG and other European frameworks. Source: https://github.com/CMTA/CMTAT (reference implementation), https://www.cmta.ch/ (association). Primary. Not date-sensitive for the framework's existence.

**16. CMTAT architecture (RuleEngine, SnapshotEngine, UUPS proxy).** Source: CMTAT reference implementation README and technical specification on the CMTA GitHub. Primary. The architecture description in the article is drawn from the project's own documentation.

**17. UBS / SBI / DBS cross-border repo.** November 2023 transaction under Singapore's Project Guardian, using CMTAT-issued bond on Ethereum, settling across Japan, Singapore, and Switzerland through nine smart contracts. Source: UBS press release (https://www.ubs.com/global/en/media/display-page-ndp/en-20231102-uniicus.html), Project Guardian documentation. Primary (issuer-published). Settled date and mechanics.

**18. Nine smart contracts claim.** The article states the repo interacted with nine smart contracts. Source: UBS press materials and secondary coverage of the pilot. Qualifier: the exact number comes from UBS's own description and is reproducible across multiple secondary sources. Use as stated.

## ERC-4626 and sBUIDL

**19. ERC-4626 EIP.** Tokenized vault standard, Final. Source: https://eips.ethereum.org/EIPS/eip-4626 . Primary. Not date-sensitive.

**20. sBUIDL launch.** Article cites May 2025 launch of sBUIDL by BlackRock and Securitize using Securitize's sToken framework, built on ERC-4626. Source: Securitize press materials from May 2025 (https://securitize.io/learn/press, specific URL to be confirmed). Primary, company-reported. The launch month is settled; the exact URL should be confirmed at publication.

**21. sBUIDL mechanism.** Article describes BUIDL holders depositing into the vault, receiving sBUIDL share tokens with non-rebasing dynamics where the exchange rate to BUIDL increases as yield accrues. Source: Securitize sToken documentation. Primary mechanic, documented design.

## ERC-7540 and Centrifuge

**22. ERC-7540 EIP and Centrifuge co-authorship.** Asynchronous tokenized vault standard. Source: https://eips.ethereum.org/EIPS/eip-7540 . Centrifuge is named as a co-author in the EIP itself. Primary.

**23. Centrifuge AUM (~$951M on Ethereum).** Source: Centrifuge dashboard and RWA.xyz tracker. Qualifier: aggregator-tracked, date-sensitive. Confirm at publication.

**24. JTRSY AUM (~$504M).** Janus Henderson Anemoy Treasury Fund. Source: Centrifuge pool dashboard (https://app.centrifuge.io/), JTRSY product page. Qualifier: company-reported via pool dashboard; date-sensitive. Confirm at publication.

**25. Epoch batching at 24-hour cadence.** Centrifuge processes `requestDeposit` and `requestRedeem` calls in 24-hour epochs at the period's NAV. Source: Centrifuge V3 protocol documentation. Primary. Not date-sensitive for the design pattern.

## Legacy standards

**26. ERC-1400 umbrella structure.** Four sub-EIPs: ERC-1410 (partially fungible), ERC-1594 (core security token), ERC-1643 (document management), ERC-1644 (controller operations). Source: https://github.com/ethereum/EIPs/issues/1411 and the individual EIP drafts. Primary. The "Draft" status of these EIPs is documented in the repository; they were never finalized.

**27. INX Limited IPO ($85M, 7,200 investors, 2021, ERC-1404).** Source: INX SEC filings (S-1, Form 10-K), https://www.sec.gov/ . Primary (SEC filings). Settled. The article should confirm the exact final raise figure and investor count against the SEC documents before publication; the $85M / 7,200 figure is widely cited but the SEC filings are authoritative.

**28. ERC-1643 adoption into CMTAT.** Article claims CMTAT adopted ERC-1643's document management approach. Source: CMTAT reference implementation, which inherits or references ERC-1643-style document binding. Qualifier: documented in the CMTAT spec but worth one direct sentence-level check against the current code before publication.

## Specialized standards

**29. ERC-1155 multi-token standard.** Source: https://eips.ethereum.org/EIPS/eip-1155 . Final. Primary.

**30. Polytrade ERC-1155 for invoices.** Article cites Polytrade's use of ERC-1155 for invoice financing. Source: Polytrade documentation and protocol materials. Qualifier: company-reported. The specific assertion that "every invoice is unique but trades frequently enough that batch operations matter" is the article's framing of Polytrade's design rationale; the underlying fact (Polytrade uses ERC-1155 for invoices) is documented.

**31. ERC-7092 draft status.** Source: https://eips.ethereum.org/EIPS/eip-7092 . Primary. Currently in Draft; the article reflects that status explicitly.

**32. ERC-7092 architecture (coupon, maturity, principal embedded onchain).** The EIP specifies these as standardized variables with native functions for accrued interest. Source: EIP-7092 specification. Primary.

**33. Bond type support (callable, puttable, convertible) in ERC-7092.** The standard's design includes hooks for callable redemption (issuer-triggered) and puttable redemption (holder-triggered), with conversion logic for convertible bonds. Source: EIP-7092 spec and reference materials. Primary mechanic, documented in the EIP.

## Honest-limits section

**34. Cross-standard composability gap.** Asserted as a limitation in the article. Source: this is an industry observation, not a citable figure. The point that the sBUIDL wrapper is bespoke (built by BlackRock and Securitize specifically for BUIDL) is documented in Securitize's own materials and stands without external citation.

**35. Identity portability across issuers.** Asserted as a limitation. Source: industry observation; no formal study of cross-issuer ONCHAINID portability exists to cite. The ERC-3643 Association acknowledges identity portability as a forward direction in its materials.

**36. Settlement coupling with offchain rails.** Asserted as bespoke per protocol. Source: industry observation; Centrifuge's epoch-batching mechanism is publicly documented but is project-specific, not a shared infrastructure layer.

**37. Regulatory environment by jurisdiction.** Switzerland (DLT Act), Germany (eWpG), Hong Kong, Singapore (Project Guardian) provide ledger-based ownership recognition; US federal law does not. Source: legal commentary from the law firms active in tokenization (Sullivan & Cromwell, Davis Polk, A&O Shearman). The article keeps this point at a high level and avoids legal claims; do not strengthen without legal review.

**38. Tooling vendors named.** Securitize, Tokeny, Taurus. Source: each vendor's website and the case studies above. Qualifier: vendor list is illustrative, not exhaustive. The article should be careful not to imply endorsement; the framing "vendors like Securitize, Tokeny, Taurus, and similar" is the right register.

## Sanity checks and deliberate omissions

**39. Things deliberately left out.** Solidity code snippets, Solidity inheritance patterns, gas-cost benchmarks per standard, vendor pricing, regional legal framework analysis beyond high-level mention, and L2-specific deployment trade-offs. These belong in vendor documentation or a sibling article, not in a decision-oriented overview.

**40. No diagrams in v1.** Three diagram placeholders existed in the user's source draft (flowchart comparing standards, ERC-3643 registry flow, sBUIDL illustration). The published v1 ships without them to match the existing vault article style. If editors at ethereum.org request diagrams, the highest-value single addition is a decision flowchart at the top of the technical section.

**41. Numbers to refresh before publication.** RWA totals (#4, #5), BUIDL AUM (#8), ERC-3643 Association member count (#11), Centrifuge total AUM (#23), JTRSY AUM (#24), DTCC integration phrasing (#12), INX IPO exact figures (#27). All are date-sensitive or phrasing-sensitive enough that they should be re-checked against primary sources within a week of publication.

**42. Sources not yet primary-verified in this research pass.** The user's source draft was the proximate input for several specific figures. Treat any number not anchored above to its primary source URL as "needs primary verification": specifically the "9 smart contracts" claim for the UBS repo, the BUIDL "near real-time 24/7/365 transfers" language if used, and the exact sBUIDL launch date within May 2025. None are speculative; all are reproducible against vendor materials but should be confirmed before the article is published externally.

**43. Strongest honest critique of the framing.** The article positions Ethereum as the leading securities tokenization venue without engaging the strongest critique: institutional issuers may eventually prefer permissioned chains (Canton Network, Avalanche subnets, JPM Onyx, HSBC Orion) for regulatory comfort. The article does not litigate that comparison and should not, because the brief is "which Ethereum standard to use" not "which chain to use." But the framing should be honest that the choice of Ethereum is presumed by the reader's having reached this article, not argued from first principles.
