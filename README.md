# Ironfish Rpc Protocol Specification
In order for a software application to interact with the Ironfish blockchain - either by reading blockchain data or sending transactions to the network - it must connect to an Ironfish node.

For this purpose, Ironfish node implements a [RPC specification](https://github.com/iron-fish/ironfish/blob/master/ironfish/src/rpc/adapters/socketAdapter/protocol.ts), so there are a uniform set of methods that applications can rely on regardless of the specific node. Actually, Ironfish node offers the full feature set of its capabilities through RPC.

This repository contains the Ironfish Rpc protocol specification and ways to perform rpc requests with Ironfish node.

- [1. RPC Overview](./Rpc-Overview.md#1-rpc-overview)
- [2. RPC Methods](./Rpc-Methods.md#2-rpc-methods)
