Ergo Merged Mining Framework
===============================

In this document we outline specification for merged-mined sidechains on Ergo. This specification would be useful to 
framework developers as well as Ergo miners.

Merge Mined Sidechains Data
---------------------------

In this section we consider how sidechain data can be stored on the main-chain. A sidechain progress can be represented in the simplest form as a tuple $(h, T_h, U_h, C_{h})$, where
$h$ is a sidechain height, $T_h$ are state changes (transactions) done in $h$, $U_h$ is digest of AVL+ tree built on top
of UTXO set after processing the changes $T_h$, $C_{h}$ is AVL+ tree which contains all the previous sidechain states in form
of key-value pairs $h \rightarrow hash(h || T_h || U_h || C_{h-1})$.

We write this data in a box associated with sidechain id. 

We can make this block spendable every input-block, which would produce a lot of transactions though. So it would be better
to make transaction of class 2 (spendable in ordering block only), and then from a stream of such transactions only one 
included into ordering block is actually included into the blockchain and paying a fee for that, others are not processed
by the network but accounted via Merkle tree included 

Again, sidechain data box is identified via NFT put in the box. The box can be updated via signature corresponding to 
miner's pubkey. In case of a different script, a script may include condition which depends on mining pubkey or 
header votes and always be evaluated to `true`, e.g. `CONTEXT.minerPubKey.size >= 0`. 

Register R4 of the box contains hash of sidechain data.

Register R5 of the box contains id of previous box which contains sidechain data, whether included in ordering block, 
or just referenced in input block. 
Refresh transaction is spending box included into last ordering block, and possibly referencing other attempts to spend 
via R5 register of the sidechain data box,

There is Merkle tree support in Sigma-Rust and `extension_proof_for` function to check sidechain data box refresh 
transaction proof.


Interaction With the Ergo Blockchain
------------------------------------


Extending Validation Context With Sidechain Data
------------------------------------------------




Implementation Plan
-------------------

* Implement functions to work with input blocks in Ergo Client ( github.com/SethDusek/ergo_client ), also, 
a function to get extension section corresponding to an input block (implement  corresponding API method 
in reference client as well)
* implement tracking loop to get recent state committed on the chain (with signalling on rollback etc)
* implement sidechain data posting loop