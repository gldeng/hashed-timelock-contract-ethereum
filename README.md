# hashed-timelock-contract-ethereum

[![NPM Package](https://img.shields.io/npm/v/ethereum-htlc.svg?style=flat-square)](https://www.npmjs.org/package/ethereum-htlc)
[![Build Status](https://travis-ci.org/chatch/hashed-timelock-contract-ethereum.svg?branch=master)](https://travis-ci.org/chatch/hashed-timelock-contract-ethereum)

[Hashed Timelock Contracts](https://en.bitcoin.it/wiki/Hashed_Timelock_Contracts) (HTLCs) for Ethereum:

- [HashedTimelockERC20.sol](contracts/HashedTimelockERC20.sol) - HTLC for ERC20 tokens

Use these contracts for creating HTLCs on the Ethereum side of a cross chain atomic swap (for example the [xcat](https://github.com/chatch/xcat) project).

## Run Tests
* Install dependencies
* Start [Ganache](https://www.trufflesuite.com/ganache) with network ID `4447`
* Run the [Truffle](https://www.trufflesuite.com/truffle) tests

```
$ npm install
$ npm run ganache-start
$ npm run test
Using network 'test'.

> Compiling ./test/helper/AliceERC20.sol
> Compiling ./test/helper/AliceERC721.sol
> Compiling ./test/helper/BobERC20.sol
> Compiling ./test/helper/BobERC721.sol

  Contract: HashedTimelock swap between two ERC20 tokens
    ✓ Step 1: Alice sets up a swap with Bob in the AliceERC20 contract (277ms)
    ✓ Step 2: Bob sets up a swap with Alice in the BobERC20 contract (256ms)
    ✓ Step 3: Alice as the initiator withdraws from the BobERC20 with the secret (146ms)
    ✓ Step 4: Bob as the counterparty withdraws from the AliceERC20 with the secret learned from Alice's withdrawal (146ms)
    Test the refund scenario:
      ✓ the swap is set up with 5sec timeout on both sides (3609ms)


  5 passing (5s)
```

## Protocol - Native ETH

### Main flow

![](docs/sequence-diagram-htlc-eth-success.png?raw=true)

### Timelock expires

![](docs/sequence-diagram-htlc-eth-refund.png?raw=true)

## Protocol - ERC20

### Main flow

![](docs/sequence-diagram-htlc-erc20-success.png?raw=true)

### Timelock expires

![](docs/sequence-diagram-htlc-erc20-refund.png?raw=true)

## Interface

### HashedTimelockERC20

1.  `newContract(receiverAddress, hashlock, timelock, tokenContract, amount)` create new HTLC with given receiver, hashlock, expiry, ERC20 token contract address and amount of tokens
2.  `withdraw(contractId, preimage)` claim funds revealing the preimage
3.  `refund(contractId)` if withdraw was not called the contract creator can get a refund by calling this some time after the time lock has expired.

See [test/htlcERC20ToERC20.js](test/htlcERC20ToERC20.js) for examples of interacting with the contract from javascript.


