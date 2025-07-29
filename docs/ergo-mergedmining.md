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

We write this data in extension section of a block. 

Every sidechain should have one byte id, from 0 to 254 (255 is left for possible extension to more).

Then key to write is (0x04, sidechain_id) (two bytes).
The value is hash of sidechain data.


Interaction With the Ergo Blockchain
------------------------------------


Implementation Plan
-------------------

* Implement Merkle tree in Rust (following reference client implementation in Scala)
