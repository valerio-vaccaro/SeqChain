# The SeqChain protocol

## What is a state?
A state is a certain combination of variables or a document or a file at a certain point in time.

A change in a variable or bit leads to a new state. The state can be represented by applying the hash function to the serialization of the file or document, the use of a hash function allows you to represent a state with a string of fixed size and avoids publishing information relating to the document or the file itself making the content confidential.

Hereinafter by status, we mean the hash of the serialized content.

## How to register multiple states in one transaction
Multiple states for different files can be added to a single transaction on a public blockchain in the following manner.

1. collect multiple attestations (signed states),
2. check for a new utxo not connected with another contract (`Next seal`),
3. get `Emergency seal` from the previous contract,
4. get `Next seal` from a previous contract and call it `Actual seal`,
5. create a document (contract) with all the collected attestations and some utxos,
6. hash the contract using sha256,
7. spend the `Actual seal` utxo adding an `op_return` output containing the hash of the contract.

The contract serialization is described in the file [Contract.md](https://github.com/valerio-vaccaro/SeqChain/blob/master/Contract.md).

### Attestations list
The attestations list is contained between two lines of dashes `----------------------------------------`, the first line contains the description of the fields which in our case is `PubKey:Hash:Signature`, while in the following lines there are all the claims, one per line.

Every single attestation contains the following parts separated by a `:`. This version contains the following fields:
- `PubKey`, the public key associated with the status stream encoded in base64,
- `Hash`, the hash (sha256) in hex for the last status,
- `Signature`, a signature of the hash encoded in base64.

## Anti double-spending
The update of a contract can be performed creating a new contract and spend the next utxo adding an output with the hash of the new contract.

### How I can assure that exists only one chain?
The service provider publishes the generator transaction in a public space (e.g. website) and gives all contract via a special API.

1. a client get the generator transaction and identify the op_return value (we will call it HASH),
2. search a contract with hash equal to HASH using the API provided,
3. get next utxo (we will call it NEXT) in the identified contract,
4. if the emergency seal is spent NEXT will be set to emergency seal value,
5. fetch transaction associated with NEXT utxo,
6. if the output associate NEXT is not spent last document is the last valid state,
7. else go to step 3 and continue with the next contract.

The status chain for a specific document can be extracted filtering contract exists containing some attestations with the pubkey associated with the document.

## Emergency seal
If an emergency seal is spent the chain continues using the emergency seal utxo, usually this transaction will not be spent.

## Version
- 0.0.1 first version
- 0.0.2 remove tabs from the "Contract" file, add SeqChain in the header and add support for multiple blockchains

# References

- [1](https://petertodd.org/2017/scalable-single-use-seal-asset-transfer) Peter Todd "Scalable Semi-Trustless Asset Transfer via Single-Use-Seals and Proof-of-Publication"
- [2](https://petertodd.org/2016/commitments-and-single-use-seals) Peter Todd "Preventing Consensus Fraud with Commitments and Single-Use-Seals"
- [3](https://github.com/BlockchainCommons/Learning-Bitcoin-from-the-Command-Line) BlockchainCommons "Learning Bitcoin from the Command-Line"
