# ENS Versus contest details
- Total Prize Pool: $36,500 USDC
  - HM awards: $21,000 USDC
  - QA report awards: $0 USDC
  - Gas report awards: $0 USDC
  - Judge + presort awards: $6,000 USDC
  - Scout awards: $500 USDC 
  - Mitigation review contest: $9,000 USDC
- Join [C4 Discord](https://discord.gg/code4rena) to register
- Submit findings [using the C4 form](https://code4rena.com/contests/2022-11-ens-versus-contest/submit)
- [Read our guidelines for more details](https://docs.code4rena.com/roles/wardens)
- Starts November 22, 2022 20:00 UTC
- Ends November 28, 2022 20:00 UTC

## C4udit / Publicly Known Issues

The C4audit output for the contest can be found [here](add link to report) within an hour of contest opening.

*Note for C4 wardens: Anything included in the C4udit output is considered a publicly known issue and is ineligible for awards.*

# About ENS

ENS is a decentralised naming service built on top of Ethereum, and designed to resolve a wide array of resources including blockchain addresses, decentralised content, and user profile information.

Developer documentation can be found [here](https://docs.ens.domains/).

Information on existing ENS deployments can be found [here](https://docs.ens.domains/ens-deployments).

# Developer guide

## Setup

```
git clone https://github.com/code-423n4/2022-11-ens
cd 2022-11-ens
yarn
```

## Running Tests

```
yarn test
```

# Contracts in scope
**Note: Not all contracts in this repository are in-scope for this audit! We have included contracts from the original ens-contracts repo that are dependencies of in-scope contracts here for the convenience of auditors. Only the contracts listed below are in-scope.**

This audit is a second audit intended to cover changes made to the Name Wrapper contracts subsequent to the July C4 public audit.

## contracts/wrapper
### [BytesUtils.sol](https://github.com/code-423n4/2022-11-ens/tree/main/contracts/wrapper/BytesUtils.sol)
**SLOC**: 40

Contains assorted utility functions for manipulating byte strings.

### [ERC1155Fuse.sol](https://github.com/code-423n4/2022-11-ens/tree/main/contracts/wrapper/ERC1155Fuse.sol)
**SLOC**: 285

An implementation of ERC1155 that only supports 1 token per token type, with the owner, fuse/flag information, and an expiration time all packed into a single storage slot for gas-efficiency. Should conform to ERC1155, with the addition of `ownerOf`.

Dependencies:
 - @openzeppelin/contracts/utils/introspection/ERC165.sol
 - @openzeppelin/contracts/token/ERC1155/IERC1155Receiver.sol
 - @openzeppelin/contracts/token/ERC1155/IERC1155.sol
 - @openzeppelin/contracts/token/ERC1155/extensions/IERC1155MetadataURI.sol
 - @openzeppelin/contracts/utils/Address.sol

### [NameWrapper.sol](https://github.com/code-423n4/2022-11-ens/tree/main/contracts/wrapper/NameWrapper.sol)
**SLOC**: 701

A contract that wraps ENS names, providing additional functionality:
 - Makes all names at any level valid ERC1155 NFTs.
 - Stores name preimages (plaintext names) onchain in an efficient format so they can be processed if needed.
 - Allows name owners to permanently or temporarily revoke control over certain aspects of the name, to enable trustless use of ENS names.
 - Acts as a controller for the .eth registrar, so that names can be registered and wrapped in a single operation.
 - Implements ERC721 receiver functionality, so .eth 2LDs can be wrapped in a single transfer operation.

The primary reason for this wrapper is the 'fuse' functionality for revoking permissions over names; this is intended to support applications such as trustless subdomain issuance, and trustless name resolution.

For detailed documentation of the wrapper and its expected behaviour, see [its dedicated README](https://github.com/code-423n4/2022-11-ens/blob/main/contracts/wrapper/README.md).

 Dependencies:
 - ERC1155Fuse.sol
 - Controllable.sol
 - BytesUtil.sol
 - ../registry/ENS.sol
 - ../ethregistrar/IBaseRegistrar.sol
 - @openzeppelin/contracts/token/ERC721/IERC721Receiver.sol
 - @openzeppelin/contracts/access/Ownable.sol

### [INameWrapper.sol](https://github.com/code-423n4/2022-11-ens/tree/main/contracts/wrapper/INameWrapper.sol)
**SLOC**: 113

Public interface for `NameWrapper.sol`.

### [Controllable.sol](https://github.com/code-423n4/2022-11-ens/tree/main/contracts/wrapper/Controllable.sol)
**SLOC**: 17

Mixin for a contract that can have controllers - other accounts that have access to privileged functions.

### [IMetadataService.sol](https://github.com/code-423n4/2022-11-ens/tree/main/contracts/wrapper/IMetadataService.sol)
**SLOC**: 4

Interface for a metadata service - a contract that returns token metadata for a wrapped token.

### [INameWrapperUpgrade.sol](https://github.com/code-423n4/2022-11-ens/tree/main/contracts/wrapper/INameWrapperUpgrade.sol)
**SLOC**: 19

Interface for an upgrade target for the name wrapper - a new version of the wrapper.
