# Token ID

Constructs a `TokenId`.

| Constructor                                              | Description                    |
| -------------------------------------------------------- | ------------------------------ |
| `new TokenId(<shard>,<realm>,<token>)` | Initializes the TokenId object |

```java
new TokenId()
```

## Methods

| Method                                         | Type     | Description                                   |
| ---------------------------------------------- | -------- | --------------------------------------------- |
| `TokenId.fromString(<tokenId>)`          | String   | Constructs a token ID from a String value     |
| `TokenId.fromSolidityAddress(<address>)` | String   | Constructs a token ID from a solidity address |
| `TokenId.fromBytes(<bytes>)`             | byte\[] | Constructs a token ID from bytes              |

Update the custom fees for a given token. If the token does not have a fee schedule, the network response returned will be `CUSTOM_SCHEDULE_ALREADY_HAS_NO_FEES`. You will need to sign the transaction with the fee schedule key to update the fee schedule for the token. If you do not have a fee schedule key set for the token, you will not be able to update the fee schedule.
{% tab title="Java" %}
```java
TokenId tokenId = new TokenId(0,0,5);
System.out.println(tokenId);

TokenId tokenIdFromString = TokenId.fromString("0.0.3");
System.out.println(tokenIdFromString);

//Version: 2.0.1
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const tokenId = new TokenId(0,0,5);
console.log(tokenId.toString());

const tokenIdFromString =  TokenId.fromString("0.0.3");
console.log(tokenIdFromString.toString());

//Version 2.0.7
```
{% endtab %}

{% tab title="Go" %}
```go
tokenId := hedera.TokenID {
        Shard: 0,
        Realm: 0,
        Token: 5,
}

//v2.1.0
```
{% endtab %}
{% endtabs %}
