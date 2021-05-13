# 使用 everPay 完成 AR 跨链
# Understanding How everPay Transfers AR Cross-chain

## 概述
## Overview

everPay 可以作为跨链桥将 Arweave 上的 AR 代币等额映射到 Ethereum 上。
everPay can serve as a cross-chain bridge to map the AR tokens on Arweave one to one onto Ethereum.

everPay 是一个基于 Arweave 构建的跨链的代币支付结算协议（[everPay 原理](https://medium.com/everfinance/everpay-a-trusted-cross-chain-payment-protocol-eba4a0af7d66)）。everPay 的数据共识由 Arweave 进行保障，保管人（原来叫资管者 Asset Manager）通过验证在 Arweave 上的 everPay 交易对跨链进行签名。
everPay is a cross-chain token payment and settlement protocol built on Arweave (Click [this link](https://medium.com/everfinance/everpay-a-trusted-cross-chain-payment-protocol-eba4a0af7d66) to read the everPay documentation). The data consensus of everPay is secured by Arweave. Custodian (originally called Asset Manager) verify the everPay transactions on Arweave and sign cross-chain transactions.

## 工作原理
## How It Works

everPay 的所有账本透明的记录在 Arweave 上，由多个保管人共同维护账本。
In everPay, all the ledgers are recorded on Arweave in a transparent way and maintained by multiple custodians.

ethLocker 是 everPay 在以太坊上的多签资管合约，ethLocker 可以执行任意的以太坊操作。保管人根据 Arweave 上的交易生成提案执行以太坊多签。
ethLocker is everPay's multisig asset manager contract on Ethereum. It can be used to execute arbitrary operations on Ethereum. Custodians execute the proposal generated according to the Arweave transactions through the multisig scheme of Ethereum.

一种最常见的提案是用户提现 ETH/ERC20，提案在第 4 步产生：
A common proposal is an ETH/ERC20 withdrawal. It is created in step 4:

1. 用户1 将 ETH 转入 ethLocker
2. everPay 为用户1 映射出等值的 ETH（ever）
3. 用户1 将 ETH（ever）转给用户2
4. 用户2 Burn ETH（ever）

1. User 1 sends some ETH to ethLocker.
2. ethLocker maps the same amount of ETH from Ethereum onto everPay. 
3. User 1 sends ETH (ever) to users.
4. User 2 burns ETH (ever). 

1-4 的每笔交易都会序列化存储在 Arweave 上。第 4 步用户2 的 Burn 事件形成了一个 ETH 提现的提案。每个保管人都通过 Arweave 的数据验证所有交易过程，并确保 Burn 提案的正确性，然后才签署一份提现指令发送到 ethLocker 合约。
Each transaction above is stored on Arweave in a serialized way. In step 4, an ETH withdrawal proposal is created according to the burn event. Every custodian verifies all the transaction procedures according to the data on Arweave to ensure the validity of the burn proposal, and then sends a signed withdrawal instruction to the ethLocker contract.

### AR（ever）

与 Ethereum 上的多签不一样的是，everPay 使用门限签名技术保管 AR（Arweave），将 AR（Arweave）映射到 everPay 中生成等额的 AR（ever）。
Different from the multisig scheme adopted by Ethereum, everPay uses the threshold signature scheme to secure AR (Arweave) and map AR (Arweave) onto everPay to create the same amount of AR (ever).

### AR（Arweave）到 AR（Ethereum）

### From AR (Arweave) to AR (Ethereum)

原有的 AR 提现提案是：
The original AR withdrawal proposal was:

1. 用户1 将 AR 转入 ArLocker
2. everPay 为用户1 映射出等值的 AR（ever）
3. 用户1 Burn AR（ever）

1. User 1 sends AR to ArLocker.
2. ArLocker maps the AR (Arweave) onto everPay to create the same amount of AR (ever).
3. User 1 burns AR (ever).

保管人验证 Arweave 数据，获得 AR Burn 提案，多签释放  ArLocker 中的 AR。
Custodians verify Arweave data, obtain the AR Burn proposal, and then unlock the AR in ArLocker through the multisig scheme.

#### 跨链改进
#### Improved Cross-chain Solution

![](img1.png)

部署一个 AR（ERC20）到 Ethereum，ERC20 的 mint owner 为 ethLocker。
Deploy an AR (ERC20) contract to Ethereum. The mint owner of AR (ERC20) is ethLocker. 

改进原 AR 提案，新增一个 BurnToEthereum 提案：
Add a BurnToEthereum proposal to improve the original AR proposal:

1. 用户1 将 AR 转入 ArLocker
2. everPay 为用户1 映射出等值的 AR（ever）
3. 用户1 BurnToEthereum AR（ever）

1. User 1 sends AR to ArLocker.
2. ArLocker maps the AR (Arweave) onto everPay to create the same amount of AR (ever).
3. User 1 burns AR (ever) to obtain AR (Ethereum).

保管人验证 Arweave 数据，获得 BurnToEthereum 提案，签署指令： `ethLocker 调用 AR（ERC20）mint 资产到用户1`。
Custodians verify Arweave data, obtain the BurnToEthereum proposal, and then sign the instruction `ethLocker calls the AR (ERC20) contract to mint AR (Ethereum) for user 1`.

### AR（Ethereum）到 AR（Arweave）
### From AR (Ethereum) to AR (Arweave)

![](img2.png)

将 AR（Ethereum）充值到 EthLocker 即可获得 AR（ever），用户可以自行选择将 AR（ever）提现到 Arweave 或者 Ethereum。
As long as you send AR (Ethereum) to EthLocker, you can obtain AR (ever). You are free to withdraw AR (ever) to Arweave or Ethereum.

# 其他
# Note

保管人目前有由 everFinance 和 Goblin Pool（GRIN）提供。
Custodians are currently provided by everFinance and Goblin Pool (GRIN).
