# We need better wallets - A charge for builders

Wallets, your only way to interact with Ethereum, the fully transparent, permission-less network. Yet, every wallet address is exposed and can be linked to every other address with which it interacts. This leaves some serious concerns for your privacy. Wallet addresses by themselves are at least pseudonymous, meaning they don't carry any intrinsic data about who is using them, but the concerns mount up when you consider a single transaction can end up inadvertently linking your entire wallet history, and adjoining wallets, to you.

This article is meant to illuminate the current pitfalls, what Kohaku is trying to do to eliminate them, and where the opportunity lies if you are a builder, specifically, a wallet builder.

## The rough edges of wallet privacy

Most people have multiple wallet addresses that they use for different activities. This is a good security practice because you can limit the blast radius of signing the wrong transaction. It can also be somewhat helpful in separating your activities so that an onlooker cannot associate activities from one address to the activities of another one but, in practice, this is a very frail form of privacy. If two wallet addresses are funded by the same origin or have payments back and forth then they are effectively linked.

Having all your wallets linked means that anyone can know exactly which tokens you hold, what quantities, and when you are performing various actions. Imagine if your off-chain spending habits were public information. Suddenly your neighbor knows how much money you make, where you ate lunch yesterday, what medications you buy, this isn't just about your neighbor but about scammers, hackers and robbers being able to use this information against you.

For instance, if you have a sizable amount of crypto then you may get targeted by nefarious groups who use your data to match personal records, maybe an ENS name that is similar to an email address that was leaked alongside home address information. If you are like me, the idea of getting wrench attacked doesn't sound all that fun.

Another huge privacy concern is the data that is leaked off-chain through the RPC provider. An RPC (Remote Procedure Call) provider is the server your wallet talks to whenever it needs to read or write onchain, unless you are running your own node, every interaction your wallet makes is going through someone else's machine. Every time your wallet queries for data onchain (e.g. wallet balance) the RPC sees more data than it should. Your IP address is associated with every request which means a single RPC provider can connect all of your wallets, onchain associates and interactions with you. Your IP address also includes some data about where you are located geographically. All of this could be combined with onchain data to profile you surprisingly well. This may lead to something as inert as ad-targeting but the same data could be sold (or leaked through a hack) to your landlord, your abusive ex, foreign governments, or robbery rings. Yikes! Not to mention how AI is making it dramatically cheaper to process large volumes of data, spotting patterns where no human could make a connection. Essentially, we are toast if we don't find solutions to maintain privacy onchain.

## Why the wallet layer is the best place to carry the weight

Ethereum has plans to integrate privacy into the base layer, but Ethereum only gets two hard forks a year, and the privacy-relevant upgrades will likely roll in across several of them. That's a multi-year wait. Users need privacy now. These hard forks will come in time and provide a better foundation for privacy, but users don't need to wait to have privacy today.

You might think the app layer should be the one to fix this, but apps have little incentive to integrate privacy themselves, more risk to their core use cases and a potentially rougher user experience hang in the balance. Except for apps where privacy is the core use case, they have little reason to prioritize privacy. And even if they did try, every app rolling its own privacy story produces an inconsistent, half-broken experience for users.

That leaves the wallet. The wallet is the one piece of software that sees every dApp the user touches, every address they hold, and every RPC request they make. If anyone is positioned to pull as much weight as possible on privacy, it's the wallet. This is where Kohaku comes in.

## What Kohaku is

The entire goal of Kohaku is to make it easier for people to have privacy and security onchain. Kohaku is an SDK (Software Development Kit) that makes it easier to integrate the powerful privacy and security primitives that are on Ethereum, Railgun and Privacy Pools are currently supported and Tornado Cash is in the process of being added, alongside infrastructure pieces like a light client for trustless state verification and post-quantum-ready smart accounts.

This means wallets can add privacy features by importing the Kohaku SDK, enabling the specific packages they want to make accessible to users from their wallet interfaces. It is modular and wallet-agnostic. You take what you need.

## What the SDK unlocks

### Private balances

One amazing feature is the concept of a private balance. You can selectively shield any amount of your tokens and the wallet will track both balance types side by side. Yes, you read that correctly, any ERC20 token, including ETH (via WETH). This is made possible through a protocol called Railgun. With this you can also do private token transfers to other wallet addresses and interact with DeFi applications.

Tornado Cash is in the process of being added, a protocol enabling a less sophisticated way to shield funds. The way it works is you deposit funds into a pool and over time other deposit to the same pool, once some time has passed you withdraw to a brand new address, completely unlinkable to your original wallet address because of some fancy zero knowledge cryptography.

Kohaku also integrates Privacy Pools, which works the same way as Tornado Cash but adds a compliance layer on top. It makes it so you can prove your funds are not from an illicit source while being able to move them to an unlinked address.

Working with these privacy protocols directly often comes with large learning curves to make sure you don't accidentally expose linkable data or, even worse, lose your funds. Kohaku makes all of this easy by abstracting the rough edges. From the wallet builder's perspective, you're calling a handful of methods on a typed SDK. From the user's perspective, privacy becomes a feature that is accessible through a couple button pushes.

### Trustless state verification

Every time you interact with Ethereum you are doing so through a centralized RPC (unless you are running your own node, you would know if you were). Kohaku uses what is called a light client to verify the state that is being returned from the RPC. This way the RPC cannot send you invalid chain information without the wallet realizing and rejecting it.

### Post-quantum-ready accounts

Quantum computers will eventually break the ECDSA signatures that secure every Ethereum account today. Nobody knows exactly when, but "eventually" is a problem you want to be ready for before it becomes urgent. Kohaku ships an ERC-4337 account implementation (`@kohaku-eth/pq-account`) that pairs a normal ECDSA key with a post-quantum signature. The account verifies both signatures on each transaction, so the ECDSA path keeps working today and the PQ key is the safety net that takes over when ECDSA stops being safe.

The verifier contracts are already deployed on Sepolia and Arbitrum Sepolia testnets, with gas costs in the 1.7M-8.4M range depending on which scheme you pick, real but tractable, and actively being optimized. The point is that you can start integrating PQ-ready accounts today, well before anyone has to scramble.

### A fresh wallet for every dApp

This is more of a suggested flow demonstrated in the Ambire-based Kohaku extension demo. It's not the only way to do it, but it's a good example of what's possible. This single flow greatly increases user privacy in one of the best user experiences possible.

Imagine that when you connect to a new dApp, your wallet doesn't quietly forward your active account. Instead, it asks: do you want to create a new wallet for this one? If you say yes, that dApp gets its own dedicated address, funded privately from your shielded balance. So every dApp you interact with is funded through these privacy tools, disconnected from any other wallet you use, and that fresh wallet only ever interacts with that one dApp. There's no way to link your wallets to one another to profile you.

## What's on the roadmap

The above is what's in builders' hands today (or close to it). There are a number of other planned features that will add more layers of privacy or even just user experience improvements:

- **Peer to peer transaction broadcasting**, submitting a transaction today means handing it to an RPC, which sees the transaction (and your IP) before propagating it. P2P broadcasting lets your wallet shove transactions directly into the gossip network, so even the write side of your wallet activity stops being linked to your IP address.
- **Social recovery through ZK-email and ZK-passport**, losing your seed shouldn't mean losing your account, but most recovery options today require trusting some intermediary. Zero-knowledge proofs of an email or passport let a wallet verify you are you without revealing the email or passport itself.
- **Spending policies**, a smart account with multiple signers can enforce rules per signer. The hot key on your phone might be limited to small transactions; anything bigger needs the hardware wallet to sign too. This is what moves smart accounts from "more complex than EOAs" to "actually safer than EOAs."
- **A universal Ethereum app for hardware wallets**, the firmware that signs Ethereum transactions on your Ledger or Trezor is written and controlled by each manufacturer. A standardized open reference breaks that vendor lock-in and lets hardware support advanced features, like signing privacy-protocol transactions, without waiting on each vendor's roadmap.

The point of listing these isn't to oversell what's shipped, it's to give wallet teams a sense of what they're opting into long-term. Once the SDK is plugged in, these become things you can light up over time rather than projects you have to start from scratch.

## Seeing it in action

If you want to see what all of this looks like in a working wallet, the Kohaku team has built a reference browser extension on top of the Ambire codebase. It is not aiming to be a mainstream consumer wallet, and it is not trying to compete with MetaMask, Rainbow, or Rabby, it exists to show what becomes possible once the SDK is in place. Install it, try the dApp-connection prompt, shield a few tokens, and let your imagination do the rest.

## What this means if you're building a wallet

The current wallet layer is very frail when it comes to preserving your privacy. Kohaku is taking a stab at providing the latest and greatest privacy features in a way that makes them easy to ship, whether you are adding privacy to an existing wallet or building a new one from the ground up. Pick the packages you want, wire them into your flows, and put whichever UI feels right for your users in front of them.

You don't need to commit to all of it. You don't need to be a privacy specialist. The reference extension is there to show you the shape of these features in case it helps, but add the Kohaku SDK to your wallet project and let it do the heavy lifting while you focus on making it the best experience possible for your users.

The starting point is the docs at ethereum.github.io/kohaku/getting-started, install `@kohaku-eth/railgun`, follow the account flow, and you'll have private balances working in your wallet in an afternoon.

You can watch and contribute to progress at the following links:

- Kohaku project board
- ethereum/kohaku, the SDK
- ethereum/kohaku-extension, the Ambire-based reference wallet
- kassandraoftroy/kohaku-cli
