# Understanding How everPay Transfers AR Cross-chain

## Overview

everPay can serve as a cross-chain bridge to map the AR tokens on Arweave one to one onto Ethereum.

everPay is a cross-chain token payment and settlement protocol built on Arweave (Click [this link](https://medium.com/everfinance/everpay-a-trusted-cross-chain-payment-protocol-eba4a0af7d66) to read the everPay documentation). The data consensus of everPay is secured by Arweave. Watchmen verify the everPay transactions on Arweave and sign cross-chain transactions.

## How It Works

In everPay, all the ledgers are recorded on Arweave in a transparent way and maintained by multiple watchmen.

ethLocker is everPay's multisig asset manager contract on Ethereum. It can be used to execute arbitrary operations on Ethereum. Watchmen execute the multisig scheme of Ethereum with the proposal generated according to the Arweave transactions.

A common proposal is an ETH/ERC20 withdrawal. It is created in step 4:

1. User 1 sends some ETH to ethLocker.
2. ethLocker maps the same amount of ETH from Ethereum onto everPay. 
3. User 1 sends ETH (ever) to user 2.
4. User 2 burns ETH (ever). 

Each transaction above is stored on Arweave in a serialized way. In step 4, an ETH withdrawal proposal is created according to the burn event. Every watchmen verifies all the transaction procedures according to the data on Arweave to ensure the validity of the burn proposal, and then sends a signed withdrawal instruction to the ethLocker contract.

### AR（ever）

Different from the multisig scheme adopted by Ethereum, everPay uses the threshold signature scheme to secure AR (Arweave) and map AR (Arweave) onto everPay to create the same amount of AR (ever).

### From AR (Arweave) to AR (Ethereum)

The original AR withdrawal proposal was:

1. User 1 sends AR to ArLocker.
2. ArLocker maps the AR (Arweave) onto everPay to create the same amount of AR (ever).
3. User 1 burns AR (ever).

Watchmen verify Arweave data, obtain the AR Burn proposal, and then unlock the AR in ArLocker through the multisig scheme.

#### Improved Cross-chain Solution

![](img1.png)

Deploy an AR (ERC20) contract to Ethereum. The mint owner of AR (ERC20) is ethLocker. 

Add a BurnToEthereum proposal to improve the original AR proposal:

1. User 1 sends AR to ArLocker.
2. ArLocker maps the AR (Arweave) onto everPay to create the same amount of AR (ever).
3. User 1 burns AR (ever) to obtain AR (Ethereum).

Watchmen verify Arweave data, obtain the BurnToEthereum proposal, and then sign the instruction `ethLocker calls the AR (ERC20) contract to mint AR (Ethereum) for user 1`.

### From AR (Ethereum) to AR (Arweave)

![](img2.png)

As long as you send AR (Ethereum) to EthLocker, you can obtain AR (ever), which can be withdrawn to Arweave or Ethereum at your will.

# Note

Watchmen are currently provided by everFinance and Goblin Pool (GRIN).
