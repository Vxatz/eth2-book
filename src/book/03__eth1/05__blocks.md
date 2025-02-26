---
title: "Blocks"
---

# {{ $frontmatter.title }}

Blocks, as we know, are collections of transactions that form the history of events within a blockchain system. We make use of blocks, instead of simply chaining together individual transactions, because they reduce the rate of unnecessary chain forks in the presence of high network latency. Any given chain of blocks, connected by each block's reference to its parent, produces some final state as a result of the transactions the chain contains. In this sense, blocks do not contain the state of a system, but provide nodes with the information necessary to compute that state for themselves. In this section, we take a look at the structure and production process of blocks in Eth1.

As blocks primarily serve to formalize the inclusion of transactions within a blockchain, they themselves introduce little additional complexity to our system. Most complexity is captured within the state and transaction structure. Blocks are simply records of transactions and their effects on Eth1's state. Some content within each block, however, pertains to the process by which blocks are produced. It's therefore both valuable and reasonable to detail the block production process prior to examining low-level block structures.

All transactions within any block must be initiated by users with access to the private key of some account. In most cases, these transactions are generated by users and then broadcast to the full network of online Eth1 nodes. Once broadcast, transactions remain in a "pool" of other transactions pending for inclusion in a block. This pool does not reside on any single machine, but is instead stored by any node that wishes to access it. Although we often consider there to exist one unified pool, different nodes may see different pools depending on the flow of transaction data through the network. For the sake of most analysis, the mental framework of a global pool is sufficient.

Nodes that aim to produce, or "mine," blocks must first select transactions for inclusion from the transaction pool. Nodes may choose any of these transactions, but the total gas consumption of transactions within a block is capped by a limit that may only be changed by `1/1024` of the limit of the previous block. Eth1 includes this cap as to avoid unnecessarily large blocks that delay block propagation. Most miners select transactions in order of highest "gas price" to maximize their rewards from transaction fees.

For each transaction selected from the pool, nodes execute the transaction against their current state. The first transaction selected is executed against the state resulting from the last block known by the node. Each additional transaction then operates on the output state of the previous transaction. Transactions continue to be processed in this manner until the total gas used reaches the specified block gas limit. At this point, the node generates most of the content within the block based on the effect of included transactions. All information attached is detailed later within this section.

Their blocks mostly complete, nodes must now attempt to find a partial hash collision that makes the block acceptable by other nodes. Eth1 requires that nodes execute a special-purpose hashing algorithm called "Ethash." Ethash uses the information included within a block to randomly select values from a large dataset on the order of several gigabytes in size. The SHA512 hash of these values then produce the final hash attempt result. When a miner increments the nonce field in their block, new values are selected from the dataset. This mechanism forces mining hardware to repeatedly access memory and thereby reduces the effectiveness of custom machines optimized to compute one specific computational task. Ethash has had the effect of significantly extending the period of time during which consumer hardware, namely computer graphics cards, could be profitably employed in the mining process. In theory, this quality expanded the base of people with access to adequate mining devices.

For a given hash attempt to be a valid Proof-of-Work, it must be at least less than the block's target difficulty. Eth1 computes the target difficulty of a block based primarily on the time difference between the block and its parent. When this difference increases, the difficulty also increases, and vice versa. As a result, general advances in hardware efficiency, leading to a more optimized hashing process, are met with an equal increase in difficulty and, on average, a stable time interval between blocks. Eth1 additionally scales block difficulty based on the current block number and thereby slowly increases difficulty over time. This "difficulty bomb" was originally introduced to provide an incentive for the development of Eth2 in the face of gradually degrading Eth1 network performance. However, the mechanism has been "diffused" by delay on several occasions. Eth1 lastly considers the rate of ommer blocks in its difficulty formula, attempting to maintain a constant rate over time. The complete formula is as follows:

::: tip TODO
Something was supposed to go here but I forget what.
:::

Once a miner has found a valid Proof-of-Work for their block, they broadcast the block to the network at large. Other nodes can then consider this block when choosing a canonical chain via their fork choice rule. We detail Eth1's fork choice rule in the following section. If a node finds the block to be canonical, it may repeat this block production process for new transactions executed against the final state given by the block. The block production cycle has now been successfully completed.

Now that we've understood the mechanism by which blocks are generated, we can take a look at Eth1's block structure. Eth1 blocks are composed of two elements, the "block header" and the "block body." The block header serves as a concise description of a block. It includes, among others fields, a block's position in the blockchain, a block's timestamp, and a Proof-of-Work. It additionally contains cryptographic commitments to the contents of the block body. A block's body holds a more complete record of the effect of the block, like the full list of transactions it executes. Let's begin by diving into the header.

A block's header consists of the following elements:

::: tip TODO
Something was supposed to go here but I forget what.
:::

A block's body contains:

::: tip TODO
Something was supposed to go here but I forget what.
:::

Although the exact structure of a block changes in Eth2, many of the same principles found here are quite universal. References to transactions, validation information, and identification details can be found in the blocks of most any blockchain system. Two-part block header and block body distinctions are equally ubiquitous. All of these elements are included within Eth2. Through this examination of a tested and proven Eth1, we've built a strong foundation for our studies of Eth2 to come.
