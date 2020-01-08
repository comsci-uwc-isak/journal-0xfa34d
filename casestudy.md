Case study questions
======

- <ins>Who/What changes the bitcoin difficulty?</ins> Magic...
- <ins>How many miners (i.e. processing power) are currently needed to run the 51% attack?</ins> TODO
- <ins>What are some possible attacks on a blockchain?</ins> Hacking a major company which handles wallets, sends bitcoins, stores major blockhains, etc.
- <ins>Are there major bitcoin servers?</ins> The ones of said companies
- <ins>Distribution of bitcoin owners?</ins> 39% owner own 98.3% (1% of population owns 50% of global wealth)
- <ins>What is a Merkle tree?</ins> Tree in which every leaf node is labelled with the hash of a data block. Used to prove inclusivity in large datasets and majority of blockchain applications


Individual questions
-----
<ins>Merkle proof</ins>:
- Used to decide upon the following factors:
  - If the data belongs in the merkle tree
  - To prove the validity of data being part of a dataset (without storing the whole data set)
  - To ensure the validity of a certain data set being inclusive in a larger data set without revealing either the complete data set or its subset.
- Makes use of one way hashing
- Established by hashing a hash’s corresponding hash together and climbing up the tree until you obtain the root hash (which is or can be publicly known)
- Merkle proofs are interpreted visually using a tree data structure (hence Merkle tree)
- Source: https://medium.com/crypto-0-nite/merkle-proofs-explained-6dd429623dc5

<ins>Takeover attack</ins>: 51% attack => creating a blockchain larger than the public and legitimate one will override it; double-spending can then occur. See https://blog.goodaudience.com/what-is-a-51-attack-or-double-spend-attack-aa108db63474 for more info.
