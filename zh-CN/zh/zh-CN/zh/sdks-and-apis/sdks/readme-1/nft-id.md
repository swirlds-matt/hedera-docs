# NFT ID

The ID of a non-fungible token (NFT). The NFT ID is composed of the [**token ID**](token-id.md) and a **serial number**.

| Constructor                                 | Description                  |
| ------------------------------------------- | ---------------------------- |
| `new NftId(<tokenId>,<serial>)` | Initializes the NftId object |

```java
new NftId()
```

## Methods

| Method                         | Type       | Requirement |
| ------------------------------ | ---------- | ----------- |
| `NftId.fromString(<id>)` | String     | Optional    |
| `NftId.fromBytes(<id>)`  | bytes \[] | Optional    |

Update the custom fees for a given token. If the token does not have a fee schedule, the network response returned will be `CUSTOM_SCHEDULE_ALREADY_HAS_NO_FEES`. You will need to sign the transaction with the fee schedule key to update the fee schedule for the token. If you do not have a fee schedule key set for the token, you will not be able to update the fee schedule.
{% tab title="Java" %}
```java
new NftId(new TokenId(0,0,2), 56562);

// v2.0.11
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
new NftId(new TokenId(0,0,2), 56562);

// v2.0.28 
```
{% endtab %}

{% tab title="Go" %}
```java
nftId := hedera.NftID{
    TokenID: tokenId,
        SerialNumber: serialNum,
}

// v2.1.13
```
{% endtab %}
{% endtabs %}
