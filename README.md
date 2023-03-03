# ZK Voting Systems

A program with rewarding users in voting systems.

Imagine a system where users can vote with tokens(which they retain) and are rewarded for the number of votes they receive in a period.

Suppose that some wealthy user acquires some quantity N of tokens, and as a result each of the user's k votes gives the recipient a reward $N•q$, $q$ here probably being a very small number, e.g think $q = 0.000001$.

The user simply upvotes their own sockpuppet accounts, giving themselves the reward of $N•k•q$. Then each user has an 'interest rate' of $k•q$ per period.

see [collision article]() and [governance]() by Vitalik.

## Bribery in voting systems

Suppose Alice can vote for a project and receive a grant. If Charlie has a candidate project he may want to bribe Alice to vote for his project, he could do this via a side channel, and it would be unknown to the voting system.

A partial way around this is to encrypt the votes, so that if Alice's vote is seen in the system, we cannot tell which project she voted for. To do this Alice could use some key, however is the encrypted vote is public, Alice could send the details of how she voted to Charlie, who could verify it, and Alice could claim her bribe.

A further refinement is then to allow Alice to vote multiple times, revoking the previous key she used, effectively invalidating the previous vote.

In the case Charlie loses the confidence that he has in the information that Alice sends him, as he knows she could have accepted his bribe, then later voted for someone else (and maybe get a bribe from them etc.)

MACI uses this approach, plus ZKPs to create an infrastructure that mitigates the effect of collision.

## Minimal Anti-Collision Infrastructure

The process of implementing this in a smart contract.

Whitelisted voters named Alice, Bob, and Charlie register to vote by sending their public key to a smart contract. Additionally, there is a central coordinator Dave, whose public key is known to all.

When Alice casts her vote, she signs her vote with her private key, encrypts her signature with Dave's public key, and submits the results to the smart contract.

Each voter may change her keypair at any time. To do this, she creates and signs a key-change command, encrypts it, and sends it to the smart contract. This makes it impossible for a briber to ever be sure that their bribe has any effect on the bribee's vote.

If Bob, for instance, bribes Alice to vote a certain way, she can simply use the first public key she had registered – which is now void – to cast a vote. Since said vote is encrypted, as was the key-changing message which Alice had previously sent to Dave, Bob has no way to tell is Alice had indeed voted the way he wanted her to.

Even if Alice reveals the clear text of her vote to Bob, she needs to not show him the updated key command that she previously used to invalidate that key. In short, as long as she had submitted a single encrypted command before her vote, there is no way to tell if said vote is valid or not.

## ZKPs used in Minimal Anti-Collision Infrastructure

From MACI documentation:

There are two zkSNARK circuits in MACI: one which allows the coordinator to prove the correctness of each state root transaction, and the other which proves that they have correctly tallied all the votes.

## Quadratic Voting

Quadratic voting works by allowing users to "pay" for additional votes on a given matter to express their support for given issues more strongly, resulting in voting outcomes that are aligned with the highest willingness to pay outcome, rather than just the outcome preferred by the majority regardless of the intensity of individual preferences.

A general drawback to these ideas occurs where the number of votes cast is small.

## Team

- Meek Msaki - Studying Zero Knowledge Proofs bootcamp by extropyio
- Erik Larson - Online Fintech Instructor & Studying Zero Knowledge Proofs Bootcamp by extropyio
