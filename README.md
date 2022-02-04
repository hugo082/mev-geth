# MEV-geth

This is a fork of go-ethereum, [the original README is here](README.original.md).

Flashbots is a research and development organization formed to mitigate the negative externalities and existential risks posed by miner-extractable value (MEV) to smart-contract blockchains. We propose a permissionless, transparent, and fair ecosystem for MEV extraction that reinforce the Ethereum ideals.

## Quick start

```
git clone https://github.com/flashbots/mev-geth
cd mev-geth
make geth
```

See [here](https://geth.ethereum.org/docs/install-and-build/installing-geth#build-go-ethereum-from-source-code) for further info on building MEV-geth from source.

## Documentation

See [here](https://docs.flashbots.net) for Flashbots documentation.

| Version | Spec                                                                                        |
| ------- | ------------------------------------------------------------------------------------------- |
| v0.4    | [MEV-Geth Spec v0.4](https://docs.flashbots.net/flashbots-auction/miners/mev-geth-spec/v04) |
| v0.3    | [MEV-Geth Spec v0.3](https://docs.flashbots.net/flashbots-auction/miners/mev-geth-spec/v03) |
| v0.2    | [MEV-Geth Spec v0.2](https://docs.flashbots.net/flashbots-auction/miners/mev-geth-spec/v02) |
| v0.1    | [MEV-Geth Spec v0.1](https://docs.flashbots.net/flashbots-auction/miners/mev-geth-spec/v01) |

### Feature requests and bug reports

If you are a user of MEV-Geth and have suggestions on how to make integration with your current setup easier, or would like to submit a bug report, we encourage you to open an issue in this repository with the `enhancement` or `bug` labels respectively. If you need help getting started, please ask in the dedicated [#⛏️miners](https://discord.gg/rcgADN9qFX) channel in our Discord.

# Prupose of this fork

In order to allow an easier transaction simulation via the `eth_callBundle` method, we introduce a new parameter `stateTransactionHash` that should be in the `stateBlockNumber` child. If this parameter is provided, we will execute all transactions from index 0 to the state transaction index (included).

### Example

- Chain:
    - Block 0xA
        - Trx `0x0`
        - Trx `0x1`
        - Trx `0x2`
    - Block `0xB`
        - Trx `0x3`
        - Trx `0x4`
        - Trx `0x5`

A call to `eth_callBundle` with:
- `stateBlockNumber=0xA` and `stateTransactionHash=0x4` will result to a state at the end of the transaction `0x4`
- `stateBlockNumber=0xA` and without `stateTransactionHash` will result to a state at the end of the block `0xA`
- `stateBlockNumber=0xA` and any `stateTransactionHash` different than `0x3`, `0x4` or `0x5` is invalid