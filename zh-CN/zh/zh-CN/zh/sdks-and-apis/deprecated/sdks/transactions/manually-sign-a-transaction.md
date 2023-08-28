# Manually sign a transaction

Sign a transaction using the private key(s) required to sign the transaction. You cannot sign the transaction with a public key. If your client operator account private key is the key used in the key field(s) of a transaction, you do not need to manually sign the transaction. The `execute(client)` method signs the transaction with the client operator account private key before it is submitted to a Hedera network.

Update the custom fees for a given token. If the token does not have a fee schedule, the network response returned will be `CUSTOM_SCHEDULE_ALREADY_HAS_NO_FEES`. You will need to sign the transaction with the fee schedule key to update the fee schedule for the token. If you do not have a fee schedule key set for the token, you will not be able to update the fee schedule.
{% tabs %}
{% tab title="V2" %}
| **Method**                                       | **Type**                     | **Description**                                                                                                                                                   |
| ------------------------------------------------ | ---------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `sign(<privateKey>)`                       | PrivateKey                   | Sign the transaction with an Ed25519 private key                                                                                                                  |
| `signWith(<publicKey, transactionSigner>)` | PublicKey, TransactionSigner | Sign the transaction with a callback that may block waiting for user confirmation.                                                                                |
| `signWithOperator(<client>)`               | Client                       | Sign the transaction with the client                                                                                                                              |
| `signWithSigner(<signer>)`                 |                              | Sign the transaction with a local wallet. Sign the transaction with a local wallet. Local wallet available in Hedera JavaScript SDK only. >=`v2.11.0` >=`v2.11.0` |

{% code title="Java" %}
```java
//Create any transaction
AccountUpdateTransaction transaction = new AccountUpdateTransaction()
     .setAccountId(accountId)
     .setKey(key);

//Freeze the transaction for signing
AccountUpdateTransaction freezeTransaction = transaction.freezeWith(client);

//Sign the transaction with a private key
AccountCreateTransaction signedTransaction = freezeTransaction
    .sign(PrivateKey.fromString("302e020100300506032b65700422042012a4a4add3d885bd61d7ce5cff88c5ef2d510651add00a7f64cb90de3359bc5c");

//v2.0.0    
```
{% endcode %}

{% code title="JavaScript" %}
```javascript
//Create any transaction
const transaction = await new AccountUpdateTransaction()
     .setAccountId(accountId)
     .setKey(key)
     .freezeWith(client);

//Sign the transaction with a private key
const signedTransaction = transaction
    .sign(PrivateKey.fromString("302e020100300506032b65700422042012a4a4add3d885bd61d7ce5cff88c5ef2d510651add00a7f64cb90de3359bc5c");
```
{% endcode %}

{% code title="Go" %}
```go
//Create any transaction
transaction := hedera.NewAccountUpdateTransaction().
        SetAccountID(newAccountId).
        SetKey(updateKey.PublicKey())

//Freeze the transaction for signing
freezeTransaction, err := transaction.FreezeWith(client)
if err != nil {
    panic(err)
}

signedTransaction := freezeTransaction.Sign(hedera.PrivateKeyFromString("302e020100300506032b65700422042012a4a4add3d885bd61d7ce5cff88c5ef2d510651add00a7f64cb90de3359bc5c"))
//v2.0.0
```
{% endcode %}
{% endtab %}

{% tabs %}
{% tab title="V1" %}
| **Method**                 | **Type**          | **Description**                                  |
| -------------------------- | ----------------- | ------------------------------------------------ |
| `sign(<privateKey>)` | Ed25519PrivateKey | Sign the transaction with an Ed25519 private key |

{% code title="Java" %}
```java
//Create any transaction
AccountUpdateTransaction transaction = new AccountUpdateTransaction()
     .setAccountId(accountId)
     .setKey(publicKey);

//Freeze the transaction for signing
Transaction freezeTransaction = transaction.build(client);

//Sign the transaction with a private key
AccountCreateTransaction signedTransaction = freezeTransaction
    .sign(Ed25519PrivateKey.fromString("302e020100300506032b65700422042012a4a4add3d885bd61d7ce5cff88c5ef2d510651add00a7f64cb90de3359bc5c");

//v1.3.2  
```
{% endcode %}

{% code title="JavaScript" %}
```javascript
//Create any transaction
const transaction = new AccountUpdateTransaction()
     .setAccountId(accountId)
     .setKey(publicKey);

//Freeze the transaction for signing
const freezeTransaction = transaction.build(client);

//Sign the transaction with a private key
const signedTransaction = freezeTransaction
    .sign(Ed25519PrivateKey.fromString("302e020100300506032b65700422042012a4a4add3d885bd61d7ce5cff88c5ef2d510651add00a7f64cb90de3359bc5c");

//v1.4.4
```
{% endcode %}
{% endtab %}
{% endtabs %}

##
