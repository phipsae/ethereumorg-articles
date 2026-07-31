1/ You cast one vote onchain and just handed the whole world your wallet, your balance, and exactly how you voted.

@phipsae's new article shows how to build private apps that don't leak who you are 🧵

---

2/ Voting is just one use. The real pattern, anonymous membership, powers a whole class of private apps.

You register once, then prove later you're in the group without linking it to your wallet. A zero-knowledge proof handles that without revealing which member you are.

---

3/ There are three moving parts:

→ a commitment that registers each member
→ a Merkle tree that pools every member into one anonymity set
→ a proof that hides which member you are, plus a nullifier that blocks a second use

---

4/ Put those three together and you've got a reusable privacy gate.

The same setup can power private payments, or let people prove they hold a token without revealing which wallet is theirs.

The plumbing stays the same, only the app on top changes.

---

5/ You don't have to build any of this yourself. There are already protocols for it, like Semaphore for group membership and MACI for private voting.

And if you want to write your own circuits, that's what Circom and Noir are for.

---

6/ You can build private apps on Ethereum today, the tools are already here.

@phipsae's article walks through the whole pattern and the full stack to build it. ↓

https://ethereum.org/latest/privacy-apps-on-ethereum/
