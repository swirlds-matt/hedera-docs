# Solidity Variables and Opcodes

The table below defines the mapping of Solidity variables and operation codes to Hedera.

| Solidity                                  | Hedera                                                                                                                                                                                                                              |
| ----------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `address`                                 | The address is a mapping of shard.realm.number (0.0.10) into a 20 byte Solidity address. The address can be a Hedera account ID or contract ID in Solidity format.                                                                  |
| `block.basefee`                           | The `BASEFEE` opcode will return zero. Hedera does not use the Fee Market mechanism this is designed to support.                                                                                                                    |
| `block.chainid`                           | The `CHAINID` opcode will return 295(hex `0x0127`) for mainnet, 296( hex `0x0128`) for testnet, 297( hex `0x0129`) for previewnet, and 298 (`0x12A`) for development networks.                                                      |
| `block.coinbase`                          | The `COINBASE` operation will return the funding account (Hedera transaction fee collecting account `0.0.98`).                                                                                                                      |
| `block.number`                            | The index of the record file (not recommended, use `block.timestamp`).                                                                                                                                                              |
| `block.timestamp`                         | The transaction consensus timestamp.                                                                                                                                                                                                |
| `block.difficulty`                        | Always zero.                                                                                                                                                                                                                        |
| `block.gaslimit`                          | The `GASLIMIT` operation will return the `gasLimit` of the transaction. The transaction `gasLimit` will be the lowest of the gas limit requested in the transaction or a global upper gas limit configured for all smart contracts. |
| `msg.sender`                              | The address of the Hedera contract ID or account ID in Solidity format that called this contract. For the root level or for delegate chains that go to root it is the account ID paying for the transaction.                        |
| `msg.value`                               | The value associated to the transaction associated in tinybar.                                                                                                                                                                      |
| `tx.origin`                               | The account ID paying for the transaction, regardless of depth.                                                                                                                                                                     |
| `tx.gasprice`                             | Fixed (varies with the global fee schedule and exchange rate).                                                                                                                                                                      |
| `selfdestruct(address payable recipient)` | Address will not be reusable due to Hedera’s account numbering policies.                                                                                                                                                            |
| `<address>.code`                          | Precompile contract addresses will report no code, including HTS System contract.                                                                                                                                                   |
| `<address>.codehash`                      | Precompile contract addresses will report the empty code hash.                                                                                                                                                                      |
| `delegateCall`                            | Contracts may no longer use `delegateCall()` to invoke system contracts. Contracts should instead use the `call()` method.                                                                                                          |

| Opcodes    | Hedera                                                                    |
| ---------- | ------------------------------------------------------------------------- |
| `PRNGSEED` | This opcode returns a random number based on the n-3 record running hash. |

{% hint style="warning" %}
### _HBAR decimal places_

_The JSON RPC Relay **`msg.value`** uses 18 decimals when it returns HBAR._ This was to provide an equivalent decimal length for web3 tools used across multiple EVM chains. _As a result, the **`gasPrice`** also uses 18 decimal places since it is only utilized from the JSON RPC Relay. Refer to the_ [_HBAR page_](../../sdks-and-apis/sdks/hbars.md) _for a table of Hedera APIs and the decimal places they return._&#x20;
{% endhint %}
