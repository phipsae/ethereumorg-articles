# How to build privacy apps on Ethereum with zero-knowledge proofs

Ethereum is radically public by design. Every address, balance, transaction, contract call, and event is visible to anyone with a block explorer. That transparency is useful when you want verifiability. It is a problem when users need to vote, claim, withdraw, or prove membership without linking every action back to the same wallet.

One reusable pattern powers a large class of privacy apps on Ethereum: anonymous membership. People register first, then later prove they belong to the group without revealing which member they are. A zero-knowledge proof is the bridge between the registration wallet and the acting wallet, and the bridge does not reveal who crossed it.

The surrounding product changes, but the privacy skeleton stays the same.

## The pattern, explained through anonymous voting

In code, the pattern has three pieces. A commitment registers each member. A Merkle tree turns those commitments into a crowd. A proof and a nullifier let one member act once without revealing which member acted.

### Step one. Registering

Every voter creates private values and keeps them offchain. In a simple version, call them the secret and the nullifier. The voter hashes those values into a public commitment, then registers that commitment onchain.

The commitment is the public registration record. The secret and nullifier are the private note the voter needs later. Lose the note and the voter cannot prove membership. Leak it and someone else may be able to use the vote or claim first.

Because the commitment is a hash, observers should not be able to recover the private values inside it. The commitment says "someone registered" without revealing who will later use that registration.

### Step two. Building the crowd

As more voters register, the app collects their commitments into a Merkle tree. A Merkle tree compresses a long list of values into a single hash, called the root. Change any value in the list and the hash changes, so the root acts as a tamper-evident summary of the whole set.

That tree is your anonymity set. If ten users are in the tree, an observer can narrow a later action down to one of those ten. If ten thousand users are in the tree, the action is much harder to link to one person. A private app with a tiny anonymity set is usually not very private, even if the cryptography is correct.

In production, this means root management matters. The contract needs to know which roots are valid, and the frontend usually needs an indexer so it can rebuild Merkle paths for users.

### Step three. Acting anonymously

When the poll opens, the voter should not vote from the same wallet that registered the commitment. Instead, the voter creates a zero-knowledge proof. The statement is encoded as a circuit: "I know private values that produce a registered commitment, and I am revealing the correct nullifier hash for this poll."

The proof convinces the verifier contract that the statement is true. It does not reveal the secret, the nullifier, or which commitment was used.

The nullifier is what prevents double voting. Alongside the proof, the voter publishes a nullifier hash. The voting contract stores that hash after accepting the vote. If the same private note is used again for the same poll, it produces the same nullifier hash, and the contract rejects the second vote.

The contract learns that some registered voter acted once. It does not learn which registered voter it was.

## The reusable gate

Now the payoff. Strip away the voting story and what you have is a privacy gate for smart contract functions.

Before the function runs, the contract checks the Merkle root, verifies the proof, confirms the nullifier hash has not been used, and binds the public inputs to the right app, chain, poll, claim, or withdrawal. If those checks pass, it marks the nullifier as used and runs the rest of the function.

Put that gate in front of a vote and you get anonymous voting. Put it in front of an airdrop claim and you get anonymous claims. Put it in front of a withdrawal function and you get the core of a mixer-style withdrawal flow. Same commitment tree, same nullifier idea, same proof pattern. What changes is the function body and the surrounding app logic.

It is not the whole app. It is the privacy layer. A real system still needs a circuit, verifier and app contracts, a prover UI, root tracking, and unlinkable transaction submission.

## What runs where

The private work usually happens offchain. The user stores the note, the frontend or wallet builds the witness, and the prover creates the proof. An indexer tracks commitments and Merkle roots. A relayer or ERC-4337 paymaster can submit the final transaction so a fresh wallet does not need ETH from the user's known wallet first.

The public enforcement happens onchain. The verifier contract checks the proof. The app contract checks valid roots and unused nullifiers, stores the nullifier hash, and runs the public action.

The sensitive UX is note handling. Treat the secret and nullifier like keys. Do not put them in analytics, logs, URLs, error reports, or normal server-side telemetry. A privacy app that leaks the note outside the circuit has already lost.

## The tooling caught up

You do not need to write pairing math by hand. A common path is to write the circuit in a high-level zero-knowledge language, generate a Solidity verifier, and call that verifier from the app contract.

The right stack depends on the job. Circom with snarkjs is a long-running path for app-level circuits. Noir with Barretenberg is a newer developer-friendly path. ZoKrates is an Ethereum-oriented zkSNARK toolbox. Halo2 and gnark are lower-level circuit libraries. zkVMs such as RISC Zero or SP1 prove normal programs, but can be heavier than a small custom circuit.

For anonymous membership, study reusable protocols before designing from scratch. Semaphore packages group membership and nullifier-based double-use prevention into contracts and JavaScript libraries. For private voting and governance, MACI is the specialized path because it adds anti-collusion properties. Mature primitives are often safer than new circuits.

Hash choice matters inside circuits. General-purpose hashes such as SHA-256 are expensive to express as arithmetic constraints. Proof-friendly hashes such as Poseidon are designed for this environment and are usually a better fit. Check the current documentation for the proving system you use.

## The wallet catch

Even a perfect proof fails if the wallet flow leaks the link. Register from wallet A and later act from wallet A, and anyone watching can connect the transactions. Fund wallet B from wallet A right before acting, and that funding transaction creates the same problem.

This is why relayers and paymasters matter. The acting wallet should be fresh, and it should not need to receive ETH from a wallet the user is trying to separate from the action.

The same problem exists offchain. Submitting registration and action transactions from the same IP address, RPC provider, or session can weaken the privacy the circuit gives you. Frontends can leak through analytics, local storage, and support logs. A zero-knowledge proof hides the values inside the proof. It does not hide everything around the transaction.

Public inputs are another place privacy apps fail. Anything marked public in the circuit, emitted as an event, included in calldata, or stored by the contract is visible. Review public inputs as carefully as you review Solidity access control.

There are also product and legal constraints. Privacy tools have legitimate uses, but mixer-style flows and private transfers may face regulatory scrutiny. Builders should understand the rules that apply to their product and jurisdiction.

## Mainnet or L2

Privacy apps can run on Ethereum Mainnet because verifier contracts are ordinary smart contracts. When gas is low and the action is high value, Mainnet can be a reasonable place to verify proofs.

That does not make L2s obsolete. If the app needs frequent actions, low-value transactions, or many cheap registrations to grow its anonymity set, an L2 may still be better. The right deployment target depends on cost tolerance, liquidity needs, censorship-resistance needs, and where the users already are.

## What this changes for builders

Privacy on Ethereum is no longer only a research story. Builders can compose the pieces into real applications: a circuit for the private statement, a verifier for proof checking, an app contract for public rules, an indexer for Merkle data, and a relayer or paymaster for unlinkable submission.

The hard parts are product design, key management, metadata hygiene, audits, compliance, and growing the anonymity set. The proof can show that a user is eligible without revealing who they are. The rest of the app has to avoid leaking that answer somewhere else.

## Further reading

1. [Semaphore Documentation](https://docs.semaphore.pse.dev/)
2. [MACI Documentation](https://maci.pse.dev/)
3. [Circom Documentation](https://docs.circom.io/)
4. [Noir Documentation](https://noir-lang.org/)
5. [ZoKrates Documentation](https://zokrates.github.io/)
6. [EIP-4337: Account Abstraction via EntryPoint Contract](https://eips.ethereum.org/EIPS/eip-4337)
