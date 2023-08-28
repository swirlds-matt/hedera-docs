# Custom token fees

When you create a token, you have the ability to set one or many custom fees (up to 10). Custom fees are fees that are distributed to the specified accounts each time the token is transferred programmatically. These custom fees can be either a fixed fee, fractional fee, or royalty fee.\
\
All fee collectors can be exempt from being charged custom fees. To exempt all fee collectors from being charged a custom fee you must set all custom fee collectors from being exempt when creating the custom fees ([HIP-573](https://hips.hedera.com/hip/hip-573)). If this is not enabled then custom fees are only exempt for a fee collector account for which it is the collector.

* A **fixed fee** transfers a specified amount of the token, to the specified collection account(s), each time a token transfer is initiated. The custom token fee does not depend on the amount of the token that is being transferred. You have the option to collect the fee in HBAR or another fungible Hedera token. An NFT cannot be used as a token type to collect the fee. You can set a custom fixed fee for both fungible and non-fungible token types.
* A **fractional fee** transfers the specified fraction of the total value of the tokens that are being transferred to the specified fee-collecting account. Along with setting a custom fractional fee, you can impose minimum and maximum fee limits per transfer transaction. Applicable to fungible tokens only. Along with setting a custom fractional fee, you can impose minimum and maximum fee limits per transfer transaction. Applicable to fungible tokens only.
* A **royalty fee** is a fractional fee that is assessed each time the ownership of an NFT is transferred from person A to person B. The fee collector account ID defined in the royalty fee schedule will receive the royalty fee each time. The royalty fee charged is a fraction of the value exchanged for the NFT. If there is no value exchanged for the NFT, a fallback fee can be used to charge the receiving account. Applicable to non-fungible tokens only. The royalty fee charged is a fraction of the value exchanged for the NFT. If there is no value exchanged for the NFT, a fallback fee can be used to charge the receiving account. Applicable to non-fungible tokens only.

**Token Custom Fee Payment**

* Fractional fees are by default charged to the token transfer receiver. This means the receiving account of the fungible token will receive less than the transfer amount (transfer amount - custom fees). Fractional fees are by default charged to the token transfer receiver. This means the receiving account of the fungible token will receive less than the transfer amount (transfer amount - custom fees). If the `net_of_transfers` field is set to true, the fractional fees are then charged to the sending account. In this case, the receiving account will receive the full amount of the token transfer value. In this case, the receiving account will receive the full amount of the token transfer value.
* Fixed fees are paid by the sending account in the fungible or non-fungible token transfer transaction.
* Royalty fees are paid by the account exchanging the fungible value. Royalty fees are paid by the account exchanging the fungible value. When the NFT sender does not receive any fungible value, the fallback fee is charged to the NFT receiver
  * Users must understand that native royalty fees are _strictly_ a convenience feature and that the network cannot enforce inescapable royalties on the exchange of a non-fractional NFT.
  * For example, if the counterparties agree to split their value transfer and NFT exchange into separate transactions, the network cannot possibly intervene. (And note the counterparties could use a smart contract to make this split transaction atomic if they do not trust each other.) (And note the counterparties could use a smart contract to make this split transaction atomic if they do not trust each other.)
  * Counter-parties that _do_ wish to respect creator royalties should follow the pattern the network recognizes: The NFT sender and receiver should both sign a single `CryptoTransfer` that credits the sender with all the fungible value the receiver is exchanging for the NFT. Similarly, a marketplace using an approved spender account for an escrow transaction should credit the account selling the NFT in the same `CryptoTransfer` that deducts fungible value from the buying account. Similarly, a marketplace using an approved spender account for an escrow transaction should credit the account selling the NFT in the same `CryptoTransfer` that deducts fungible value from the buying account.
  * There is an [open HIP discussion](https://github.com/hashgraph/hedera-improvement-proposal/discussions/578) that proposes to broaden the class of transactions for which the network automatically collects royalties. If this interests or concerns you, please add your voice to that discussion. A custom fee schedule can include a mix of fee types. You can optionally set a token's fee schedule during the [creation of a token](define-a-token.md). If this interests or concerns you, please add your voice to that discussion. A custom fee schedule can include a mix of fee types. You can optionally set a token's fee schedule during the [creation of a token](define-a-token.md).
* The accounts transferring the token to the receiving accounts are responsible for paying the transfer transaction fee in HBAR.

In addition to the custom token fee payment, the sender account is required to pay for the token transfer transaction fee in HBAR.

**Limits**

* You can add up to 10 custom fees for a given token
* A token's treasury account is exempt from paying any custom transaction fees when the token is transferred.
* At most, two "levels" of custom token fees are allowed. At most, two "levels" of custom token fees are allowed. In other words, a token being transferred may have a custom fee schedule (first layer) which requires you to pay fees in another token that has its own fee schedule (second layer). If that’s the case, a token paid as a fee within the second layer cannot have its own fee schedule, otherwise, that would create a third layer. If that’s the case, a token paid as a fee within the second layer cannot have its own fee schedule, otherwise, that would create a third layer.
* Fees cannot be a negative value

**Transaction Fees**

* Please see the transaction and query [fees](../../../networks/mainnet/fees/#transaction-and-query-fees) table for the base transaction fee
* Please use the [Hedera fee estimator](https://hedera.com/fees) to estimate your transaction fee cost

## Custom Fee

### Fixed Fee

* The fee collector account collects the defined custom fee amount
* The amount is the amount of the token the fee collecting account will collect each time the token with the custom fee schedule is transferred
* The denominating token is the token to charge the custom fee in
  * To use the ID of the fungible token generated in the transaction, enter "0.0.0" for the token ID
    * If the token being generated is an NFT, you cannot use it in the fee schedule
  * If this field is left blank, the default token is hbar

| Constructor            | Description                           |
| ---------------------- | ------------------------------------- |
| `new CustomFixedFee()` | Initializes the CustomFixedFee object |

```java
new CustomFixedFee()
```

#### Methods

| Method                                                      | Type                                                                                                                      | Requirement |
| ----------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------- | ----------- |
| `setFeeCollectorAccountId(<accountId>)`               | [AccountId](https://github.com/theekrystallee/hedera-style-guide/blob/sdk-v1/docs/sdks/tokens/broken-reference/README.md) | Required    |
| `setHbarAmount(<amount>)`                             | [HBAR](https://github.com/theekrystallee/hedera-style-guide/blob/sdk-v1/docs/sdks/tokens/broken-reference/README.md)      | Optional    |
| `setAmount(<amount>)`                                 | long                                                                                                                      | Optional    |
| `setDenominatingTokenId(<tokenId>)`                   | [TokenId](https://github.com/theekrystallee/hedera-style-guide/blob/sdk-v1/docs/sdks/tokens/broken-reference/README.md)   | Optional    |
| `setAllCollectorsAreExempt(<allCollectorsAreExempt>)` | boolean                                                                                                                   | Optional    |

{% tabs %}
{% tab title="Java" %}
```java
//Create a custom token fixed fee
new CustomFixedFee()
    .setAmount(1) // 1 token is transferred to the fee collecting account each time this token is transferred
    .setDenominatingTokenId(tokenId) // The token to charge the fee in 
    .setFeeCollectorAccountId(feeCollectorAccountId); // 1 token is sent to this account everytime it is transferred

//Version: 2.0.14
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
//Create a custom token fixed fee
new CustomFixedFee()
    .setAmount(1) // 1 token is transferred to the fee collecting account each time this token is transferred
    .setDenominatingTokenId(tokenId) // The token to charge the fee in
    .setFeeCollectorAccountId(feeCollectorAccountId); // 1 token is sent to this account everytime it is transferred

//Version: 2.0.30
```
{% endtab %}

{% tab title="Go" %}
```go
//Create a custom token fixed fee
[]hedera.Fee{
        hedera.NewCustomFixedFee().
        SetAmount(1). // 1 token is transferred to the fee collecting account each time this token is transferred
        SetDenominatingTokenID(tokenId). // The token to charge the fee in 
        SetFeeCollectorAccountID(feeCollectorAccountId) // 1 token is sent to this account everytime it is transferred
   },
}
//Version: 2.1.16
        SetAmount(1). // 1 token is transferred to the fee collecting account each time this token is transferred
        SetDenominatingTokenID(tokenId). // The token to charge the fee in 
        SetFeeCollectorAccountID(feeCollectorAccountId) // 1 token is sent to this account everytime it is transferred
   },
}
//Version: 2.1.16
```
{% endtab %}
{% endtabs %}

### Fractional Fee

* The fee collector account is the account that will collect the specified fee amount
* The denominator of the fractional fee cannot be zero
* The fractional fee has to be less than or equal to 1
* You can apply a minimum or maximum fee to charge in cases the percentage of the transfer amount is too low or too high
* Cannot exceed the fractional range of a 64-bit signed integer
* If the assessment method field is set, the token's custom fee is charged to the sending account and the receiving account receives the full token transfer amount. If this field is set to false, the receiver pays for the token custom fees and gets the remaining token balance. If this field is set to false, the receiver pays for the token custom fees and gets the remaining token balance.

#### Methods

| Method                                                      | Type                                                              | Requirement |
| ----------------------------------------------------------- | ----------------------------------------------------------------- | ----------- |
| `setFeeCollectorAccountId(<accountId>)`               | [AccountId](../../deprecated/sdks/specialized-types.md#accountid) | Required    |
| `setNumerator(<numerator>)`                           | long                                                              | Required    |
| `setDenominator(<amount>)`                            | long                                                              | Required    |
| `setMax(<max>)`                                       | long                                                              | Optional    |
| `setMin(<min>)`                                       | long                                                              | Optional    |
| `setAssessmentMethod(<assessmentMethod>)`             | FeeAssessmentMethod                                               | Optional    |
| `setAllCollectorsAreExempt(<allCollectorsAreExempt>)` | boolean                                                           | Optional    |

{% tabs %}
{% tab title="Java" %}
```java
//Create a custom token fractional fee
new CustomFractionalFee()
    .setNumerator(1) // The numerator of the fraction
    .setDenominator(10) // The denominator of the fraction
    .setFeeCollectorAccountId(feeCollectorAccountId); // The account collecting the 10% custom fee each time the token is transferred

//Version: 2.0.14
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
//Create a custom token fractional fee
new CustomFractionalFee()
    .setNumerator(1) // The numerator of the fraction
    .setDenominator(10) // The denominator of the fraction
    .setFeeCollectorAccountId(feeCollectorAccountId); // The account collecting the 10% custom fee each time the token is transferred

//Version: 2.0.30    
```
{% endtab %}

{% tab title="Go" %}
```go
//Create a custom token fractional fee
[]hedera.Fee{
        hedera.NewCustomFractionalFee().
        SetNumerator(1). // The numerator of the fraction
        SetDenominator(10). //Create a custom token fractional fee
[]hedera.Fee{
        hedera.NewCustomFractionalFee().
        SetNumerator(1). // The numerator of the fraction
        SetDenominator(10). // The denominator of the fraction
        SetFeeCollectorAccountID(feeCollectorAccountId), // The account collecting the 10% custom fee each time the token is transferred
    }

//Version: 2.1.16
```
{% endtab %}
{% endtabs %}

## Royalty Fee

* Royalty fee charges a fraction of the value exchanged in an NFT transfer transaction. The fractional value is set by designating the numerator and denominator of the fraction. The fractional value is set by designating the numerator and denominator of the fraction.
* The fallback fee is a [fixed fee](custom-token-fees.md#fixed-fee) that is charged to the NFT receiver when there is no fungible value exchanged with the sender of the NFT

{% hint style="info" %}
Royalty fees are only applicable to non-fungible tokens (NFTs).
{% endhint %}

| Constructor              | Description                             |
| ------------------------ | --------------------------------------- |
| `new CustomRoyaltyFee()` | Initializes the CustomRoyaltyFee object |

```java
new CustomRoyaltyFee()
```

#### Methods

| Method                                                      | Type                                                              | Requirement |
| ----------------------------------------------------------- | ----------------------------------------------------------------- | ----------- |
| `setNumerator(<numerator>)`                           | long                                                              | Required    |
| `setDenominator(<denominator>)`                       | long                                                              | Required    |
| `setFallbackFee(<fallbackFee>)`                       | [CustomFixedFee](custom-token-fees.md#fixed-fee)                  | Optional    |
| `setFeeCollectorAccountId(<feeCollectorAccountId>)`   | [AccountId](../../deprecated/sdks/specialized-types.md#accountid) | Required    |
| `setAllCollectorsAreExempt(<allCollectorsAreExempt>)` | boolean                                                           | Optional    |

{% tabs %}
{% tab title="Java" %}
```java
//Create a royalty fee
new CustomRoyaltyFee()
     .setNumerator(1) // The numerator of the fraction
     .setDenominator(10) // The denominator of the fraction
     .setFallbackFee(new CustomFixedFee().setHbarAmount(new Hbar(1)) // The fallback fee
     .setFeeCollectorAccountId(feeCollectorAccountId))) // The account that will receive the royalty fee

// v2.0.14 
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
//Create a royalty fee
new CustomRoyaltyFee()
     .setNumerator(1) // The numerator of the fraction
     .setDenominator(10) // The denominator of the fraction
     .setFallbackFee(new CustomFixedFee().setHbarAmount(new Hbar(1)) // The fallback fee
     .setFeeCollectorAccountId(feeCollectorAccountId))) // The account that will receive the royalty fee

 // v2.0.30 
```
{% endtab %}

{% tab title="Go" %}
```go
//Create a royalty fee 
[]hedera.Fee{
        hedera.NewCustomRoyaltyFee().
        SetFeeCollectorAccountID(feeCollectorAccountId). // The account that will receive the royalty fee
        SetNumerator(1). // The numerator of the fraction
        SetDenominator(10). // The denominator of the fraction
        SetFallbackFee( // The fallback fee
            hedera.NewCustomFixedFee().
            SetFeeCollectorAccountID(feeCollectorAccountId).
            SetAmount(1),
        ),
    }

// v2.1.16
        SetFeeCollectorAccountID(feeCollectorAccountId). // The account that will receive the royalty fee
        SetNumerator(1). // The numerator of the fraction
        SetDenominator(10). // The denominator of the fraction
        SetFallbackFee( // The fallback fee
            hedera.NewCustomFixedFee().
            SetFeeCollectorAccountID(feeCollectorAccountId).
            SetAmount(1),
        ),
    }

// v2.1.16
```
{% endtab %}
{% endtabs %}
