# Hyperledger Besu EVM

The EthereumJ virtual machine was replaced with the [Hyperledger Besu](https://besu.hyperledger.org/en/stable/) virtual machine in the Hedera Services release [0.19](https://github.com/hashgraph/hedera-services/releases/tag/v0.19.4) as a result of [HIP-26](https://hips.hedera.com/hip/hip-26). This migration enables Hedera to maintain parity with Ethereum Mainnet evolutions, such as the EVM container formats, new opcodes, and precompiled contracts.&#x20;

### Shanghai Hard Fork

The [smart contract](../../support-and-community/glossary.md#smart-contract) platform is upgraded to support the EVM visible changes for the “[Shanghai](https://github.com/ethereum/execution-specs/blob/master/network-upgrades/mainnet-upgrades/shanghai.md)” hard fork. This includes changes introduced in the "[London](https://github.com/ethereum/execution-specs/blob/master/network-upgrades/mainnet-upgrades/london.md)", “[Istanbul](https://github.com/ethereum/execution-specs/blob/master/network-upgrades/mainnet-upgrades/istanbul.md)”, and “[Berlin](https://github.com/ethereum/execution-specs/blob/master/network-upgrades/mainnet-upgrades/berlin.md)” hard forks. Changes relating to block production, data serialization, and the fee market will not be implemented because they are not relevant to Hedera’s architecture.

Starting in the Hedera Services [0.22](https://docs.hedera.com/hedera/networks/release-notes/services#v0.22) release, the \_\_ intrinsic gas cost and input data will be charged. The intrinsic gas cost is a constant that is charged before any code is executed. The intrinsic gas cost is 21,000 gas. The input data is 16 gas per non-zero byte and 4 gas per zero byte. The input data is the data provided to the external contract function parameters when calling a contract. To learn more about gas fees, check out this [page](gas-and-fees.md).

#### 📡  [Ethereum Network Upgrades](https://github.com/ethereum/execution-specs/tree/master/network-upgrades/mainnet-upgrades)

<table><thead><tr><th width="224" align="center">EVM Hardfork Version</th><th width="227" align="center">Hedera Released Version</th><th width="299" align="center">EIPs Supported</th></tr></thead><tbody><tr><td align="center"><a href="https://github.com/ethereum/execution-specs/blob/master/network-upgrades/mainnet-upgrades/shanghai.md"><strong>Shanghai</strong></a></td><td align="center"><a href="https://github.com/hashgraph/hedera-services/releases/tag/v0.38.0">v0.38</a></td><td align="center"><p><a href="https://eips.ethereum.org/EIPS/eip-3651">EIP-3651</a>, <a href="https://eips.ethereum.org/EIPS/eip-3855">EIP-3855</a>,</p><p><a href="https://eips.ethereum.org/EIPS/eip-3860">EIP-3860</a>, <a href="https://eips.ethereum.org/EIPS/eip-4895">EIP-4895</a>,<br><a href="https://eips.ethereum.org/EIPS/eip-6049">EIP-6049</a></p></td></tr><tr><td align="center"><a href="https://github.com/ethereum/execution-specs/blob/master/network-upgrades/mainnet-upgrades/london.md"><strong>London</strong></a></td><td align="center"><a href="https://github.com/hashgraph/hedera-services/releases/tag/v0.19.1">v0.19</a></td><td align="center"><a href="https://eips.ethereum.org/EIPS/eip-1559">EIP-1559</a>, <a href="https://eips.ethereum.org/EIPS/eip-3198">EIP-3198</a>,<br><a href="https://eips.ethereum.org/EIPS/eip-3529">EIP-3529</a>, <a href="https://eips.ethereum.org/EIPS/eip-3541">EIP-3541</a>,<br><a href="https://eips.ethereum.org/EIPS/eip-3554">EIP-3554</a></td></tr><tr><td align="center"><a href="https://github.com/ethereum/execution-specs/blob/master/network-upgrades/mainnet-upgrades/berlin.md"><strong>Berlin</strong></a></td><td align="center"><a href="https://github.com/hashgraph/hedera-services/releases/tag/v0.19.1">v0.19</a></td><td align="center"><p><a href="https://eips.ethereum.org/EIPS/eip-2565">EIP-2565</a>, <a href="https://eips.ethereum.org/EIPS/eip-2929">EIP-2929</a>,</p><p><a href="https://eips.ethereum.org/EIPS/eip-2718">EIP-2718</a>, <a href="https://eips.ethereum.org/EIPS/eip-2930">EIP-2930</a></p></td></tr></tbody></table>

#### Gas Schedule

The Hedera Smart Contract Service will use the Gas Schedule from the "Shanghai" hard fork. Notable changes include warm/cold account access and refund reductions.

#### **Warm and Cold Account and Slot Access**

The "Berlin" hard fork introduced the notion of warm and cold accounts and storage slots. The first access to an account or storage slot in a transaction will need to pay the "cold" costs and all subsequent calls will pay the lower "warm" access costs. [EIP-2929](https://eips.ethereum.org/EIPS/eip-2929) contains the full details of the new cost scheduling.

Hedera does not at this time allow for "pre-warming" addresses and storage slots as part of the transaction as seen in [EIP-2930](https://eips.ethereum.org/EIPS/eip-2929). Future HIPs may support this scheme.

#### **Gas Refund Reductions and Eliminations**

In the Shanghai gas schedule, the amount of gas that can be returned from storage access usage patterns has been significantly reduced. Account refunds from `SELFDESTRUCT` have been completely removed.

**Table of Gas Cost Changes**

The current opcode gas fees are reflective of the [0.22 Hedera Service release](https://docs.hedera.com/hedera/networks/release-notes/services#v0.22).

| Operation                  | Shanghai Cost (Gas)        | Current Hedera (Gas)       |
| -------------------------- | -------------------------- | -------------------------- |
| Code deposit               | 200 \* bytes             | <p>Max of Shanghai<br>or Hedera</p>  |
| <p><code>BALANCE</code><br>(cold account)</p>  | 2600                       | 2600                       |
| <p><code>BALANCE</code><br>(warm account)</p>  | 100                        | 100                        |
| `EXP`                      | 10 + 50/byte               | 10 + 50/byte               |
| <p><code>EXTCODECOPY</code><br>(cold account)</p>  | 2600 + Mem                 | 2600 + Mem                 |
| <p><code>EXTCODECOPY</code><br>(warm account)</p>  | 100 + Mem                  | 100 + Mem                  |
| <p><code>EXTCODEHASH</code><br>(cold account)</p>  | 2600                       | 2600                       |
| <p><code>EXTCODEHASH</code><br>(warm account)</p>  | 100                        | 100                        |
| <p><code>EXTCODESIZE</code><br>(cold account)</p>  | 2600                       | 2600                       |
| <p><code>EXTCODESIZE</code><br>(warm account)</p>  | 100                        | 100                        |
| <p><code>LOG0, LOG1, LOG2,</code><br><code>LOG3, LOG4</code></p> | <p>375 + 375*topics<br>+ data Mem</p> | <p>Max of Shanghai<br>or Hedera</p> |
| <p><code>SLOAD</code><br>(cold slot)</p> | 2100                       | 2100                       |
| <p><code>SLOAD</code><br>(warm slot)</p> | 100                        | 100                        |
| <p><code>SSTORE</code><br>(new slot)</p> | 22,100                     | <p>Max of Shanghai<br>or Hedera</p> |
| <p><code>SSTORE</code><br>(existing slot,<br>cold access)</p> | 2,900                      | 2,900                      |
| <p><code>SSTORE</code><br>(existing slot,<br>warm access)</p> | 100                        | 100                        |
| <p><code>SSTORE</code><br>refund</p> | <p>Only transient<br>storage slots</p> | <p>Only transient<br>storage slots</p> |
| <p><code>CALL</code> <em>et al</em>.<br>(cold recipient)</p> | 2,600                      | 2,600                      |
| <p><code>CALL</code> <em>et al</em>.<br>(warm recipient)</p> | 100                        | 100                        |
| <p><code>CALL</code> <em>et al</em>.<br>Hbar/Eth Transfer Surcharge</p> | 9,000                      | 9,000                      |
| <p><code>CALL</code> <em>et al</em>.<br>New Account Surcharge</p> | 25,000                     | _revert_                   |
| <p><code>SELFDESTRUCT</code><br>(cold beneficiary)</p> | 2600                       | 2600                       |
| <p><code>SELFDESTRUCT</code><br>(warm beneficiary)</p> | 0                          | 0                          |

The terms 'warm' and 'cold' in the above table correspond with whether the account or storage slot has been read or written to within the current smart contract transaction, even if within a child call frame.

'CALL _et al._' includes with limitation `CALL`, `CALLCODE`, `DELEGATECALL`, and `STATICCALL`

Reference [HIP-206](https://hips.hedera.com/hip/hip-206)
