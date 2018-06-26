---
layout: post
title: Udemy - Blockchain and Bitcoin Fundamentals Part 2
category: flashcards
subcategory: blockchain
tags: Blockchain Udemy Certificate Bitcoin
summary: ""
img_post: 20180407-udemy_blockchain_p1.png
github-link: na
---

<script src="/js/plotly-latest.min.js"></script>

<script type="text/javascript"
   src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.2/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>

This is a several part summary of the Udemy lesson: Blocchain and Bit coin fundamentals completed with my own research. Welcome to Part 2.

The focus of Part 2 is to review most popular consensus algorithms.

**Consensus algortihms allow mutually untrusted, uncoordinated parties to agree on a world readable, distributed list of things: domain names, transactions, title deeds, etc...), something that cryptography makes possible.**

# Consensus


## Proof of Work: POW

With POW based systems like Bitcoins, trust concentrates at the entities that can produce the greatest amount of work.

#### Python implementation

{% highlight python linenos %}
'''
POW Distributed Consensus Algorithm
'''
import string
import random
import hashlib

def search(challenge, sz=10):
    """
    create a string as a mixed of letters lower or uppercase and digits
    """
    nonce = "".join(random.choice(string.ascii_lowercase + string.ascii_uppercase + string.digits) for x in range(sz))
    st = challenge + nonce
    st_enc = st.encode("utf-8")
    m.update( st_enc )
    hash_block = m.hexdigest()
    return nonce, hash_block


def pow(challenge, sz):
    m = hashlib.sha256()
    while True:
        nonce, hash_block = search(challenge, sz)
        if hash_block.startswith('0000'):
            print("Nonce {} | Hash Block {}".format(nonce, hash_block))
            break

if __name__ == "__main__":
    challenge = "9K7532jsXGJ8723"
    pow(challenge, sz=10)

{% endhighlight %}


## Proof of Stake: POS

In POS, miners are called **validators**. Each validators deposit their money to the blockchain to get the opportunity to validate or sign a block. In POS, the bigger your stake (deposit), the bigger the chance to solve the block. 

Here is an example, let's assume 4 validators, with different statke in the block:


<img src="/images/20180408/img01.png" width="500rem">

After some random calculation, a validator is picked to solve the problem:

  * (1) has 45% chance to be picked to solve the challenge
  * whereas (3) has only 16% chance to be picked.

The validators are not awarded with new coins but with transaction fee.

Cons: environmentally friendly, fewer cartels, impossible 51% attacks


## Delegated Proof of Stack: DPOS

Trusted Delegates are voted on by the people through approval voting.

Approval voting is a method with only upvotes, no downvotes.

The goal:

  * Decentralize Power/Trust
  * Prevetn censorship over inclusion of transaction into blocks
  * everyone agrees on the same order of events
  * apply deterministic state machine to arrive at the consensus state


Requirements characterics of the consensus algorithms:
  * what latency is until the 1st confirmation
  * what the final latency is


### Demo

We assume a set of producers who have been elected and once elected they take turns producing blocks:


(a) Every 3 seconds, a block producer is scheduled to produce a block. There is only 1 block producer who can produce a block at that time. After every block producer is used once, we shuffle them

(b) If something happens, let's say B node goes down at time 9, then we have no block at that point.

(c) Another possibility is **B** is build of **C** and next block is built of **C** too, and then this block does not propagate to the next producer. 

The rule of DPOS is that the **longest chain** is the consensus chain. That is to say we know where the consensus is because that's where the majority of the parties are.

<img src="/images/20180408/img02.png" width="500rem">

This system will work if only one honest block producer is operational . That is because that block producer can support the election that changes the set of producers that elect the new producer until we get back to full speed.


Another Feature: **The last irreversible block**. It is the block that 2/3 of the other producers have built on top of. So, anything earlier than that block are immutable. 
That features solves the problem of the long range attacks 

<img src="/images/20180408/img03.png" width="500rem">