# The SeqChain protocol

## Contract structure
The contract has this structure:

```
testservice - SeqChain v.0.0.2
Local time: 20201003-120000
----------------------------------------
PubKey:Hash:Signature
AsIHfc3NTruKPz5wplxP4IhJTDl5iaYKNuxfegQfT2C3:b41f4987b2deb17043714e98c25c871e718c399041d7c1267c16304dbb2b3085:bnYUn4utUlhviHPfUw/JYoE8BUYZ0PtWTPkh89ogZQUTEmmokAbV11lGEz6L0g11F4rVOTYxN4MrPHlKg5iUaA==
----------------------------------------
Actual seal: BTC:7786ec53a232647cd3c8ff2cd1b8863e9148731e6e4ac2de56513cde40c3a5ca:0
Next seal: BTC:9c8e9e4681b0834d49c1c4f59020dae5f6015baee1bc292ac748c44f30b34285:1
Emergency seal: BTC:4b18b6cb8fad69bd42f61e80994db63fc01008253ecb05f2037fb25a26e6cc22:1
```

- header, the first line contains the name of the service and the protocol name and version,
- `Local Time`, the local time of the server in format `YYYYMMDD-hhmmss`,
- the list of all new attestations,
- `Actual seal`, the actual seal spent with an `op_return` output (format chain:txid:vout),
- `Next seal`, the utxo able to update the contract with a new one (format chain:txid:vout),
- `Emergency seal`, an emergency utxo able to update the contract in case of emergency (format chain:txid:vout).

In the case of single supported blockchain the prefix `chain` is optional and can be omitted.
