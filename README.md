# @looksrare/sdk V2

A collection of tools to interact with LooksRare protocol V2 smart contracts.

## Usage

### Install

This package has a peer dependency on [etherjs](https://docs.ethers.io/v5/).

```bash
yarn add @looksrare/sdk-v2 ethers
```

```bash
npm install @looksrare/sdk-v2 ethers --save
```

### Getting Started

The SDK expose a main class used to perform all the onchain operations:

```ts
import { SupportedChainId } from "@looksrare/sdk-v2";
const looksrare = new LooksRare(SupportedChainId.MAINNET, provider, signer);
```

The signer is optional if you need access to read only data (:warning: Calls to function that need a signer will throw a `Signer is undefined` exception):

```ts
import { SupportedChainId } from "@looksrare/sdk-v2";
const looksrare = new LooksRare(SupportedChainId.MAINNET, provider);
```

If you work on a hardhat setup, you can override the addresses as follow:

```ts
import { Addresses } from "@looksrare/sdk-v2";
const addresses: Addresses = {...};
const looksrare = new LooksRare(SupportedChainId.HARDHAT, provider, signer, addresses);
```

:information_source: You can access the multicall provider after the instance creation:

```ts
const multicallProvider = looksrare.provider;
```

See [0xsequence doc](https://github.com/0xsequence/sequence.js/tree/master/packages/multicall) to understand how to use it.

### References

Read the [complete documentation](./doc).

### Guides

Read the [guides](./guides) if you need help with the implementation.

### FAQ

#### How to use ABIs

The SDK exposes most of the LooksRare contract [ABIs](https://github.com/LooksRare/sdk-v2/tree/master/src/abis). For convenience, some commonly used ABIs are also exported.

```ts
import { LooksRareProtocol } from "@looksrare/sdk-v2";
```

You can also export the JSON file directly:

```js
import LooksRareProtocol from "@looksrare/sdk-v2/dist/abis/LooksRareProtocol.json";
```

#### How to retrieve order nonce

Call the public api endpoint [/orders/nonce](https://looksrare.dev/reference/getordernonce), and use this nonce directly.

#### What to do when the order is created and signed ?

Use the public api entpoints [/orders](https://looksrare.dev/reference/createorder) to push the order to the database. After that, the order will be visible by everyone using the API (looksrare.org, aggregators, etc..).

#### When should I use merkle tree orders ?

Merkle tree orders are used to create several orders with a single signature. You shouldn't use them when using a bot. Their main purpose is to facilitate orders creation using a web interface.

### Why do I need to call grantTransferManagerApproval ?

When you approve a collection to be traded on LooksRare, you approve the TransferManager instead of the exchange. Calling `grantTransferManagerApproval` gives the exchange contract the right to call the transfer function on the TransferManager. You need to call this function only once, the first time you use the V2.

## Resources

- [Developer documentation](https://docs.looksrare.org/developers/welcome)
- [Public API documentation](https://looksrare.dev/reference/important-information)
- [Developer discord](https://discord.gg/jJA4qM5dXz)
