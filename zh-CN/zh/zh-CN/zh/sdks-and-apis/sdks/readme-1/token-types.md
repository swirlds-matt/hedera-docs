# Token types

There are two types of tokens you can create using the Hedera Token Service: fungible and non-fungible tokens. A fungible (`FUNGIBLE_COMMON`) token is a class of tokens that can be interchangeable with another in the same class. Tokens in this class share the same value and share all the same properties. A non-fungible token (`NON_FUNGIBLE_UNIQUE`) is a class of tokens that are not identical to the other tokens in the same class. This token type cannot be interchanged with other tokens and is differentiated by serial numbers that reference each unique token. The SDKs default to creating fungible tokens if the token type during creation is not specified.

### Token Type

#### **FUNGIBLE**

Update the custom fees for a given token. If the token does not have a fee schedule, the network response returned will be `CUSTOM_SCHEDULE_ALREADY_HAS_NO_FEES`. You will need to sign the transaction with the fee schedule key to update the fee schedule for the token. If you do not have a fee schedule key set for the token, you will not be able to update the fee schedule.
{% tab title="Java" %}
```java
TokenType.FUNGIBLE_COMMON

// v2.0.11
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
TokenType.FungibleCommon

// v2.0.28
```
{% endtab %}

{% tab title="Go" %}
```go
hedera.TokenTypeFungibleCommon

// v2.1.14
```
{% endtab %}
{% endtabs %}

#### **NON-FUNGIBLE**

Update the custom fees for a given token. If the token does not have a fee schedule, the network response returned will be `CUSTOM_SCHEDULE_ALREADY_HAS_NO_FEES`. You will need to sign the transaction with the fee schedule key to update the fee schedule for the token. If you do not have a fee schedule key set for the token, you will not be able to update the fee schedule.
{% tab title="Java" %}
```java
TokenType.NON_FUNGIBLE_UNIQUE

// v2.0.11
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
TokenType.NonFungibleUnique

// v2.0.28
```
{% endtab %}

{% tab title="Go" %}
```go
hedera.TokenTypeNonFungibleUnique

// v2.1.14
```
{% endtab %}
{% endtabs %}
