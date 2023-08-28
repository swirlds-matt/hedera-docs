---
description: >-
  How to configure a JSON-RPC endpoint that enables communication between EVM-compatible developer tools using Hashio
---

# Configuring Hashio RPC endpoints

[Hashio](https://swirldslabs.com/hashio/) is a public RPC endpoint hosted by Swirlds Labs. As a _public_ endpoint, it:

* Is free to use
* Does not have any sign-up requirements
* Has significantly restrictive rate limits

While this combination may be considered less reliable, it offers the highest levels of ease of use among RPC endpoints.

To connect to the Hedera networks via Hashio, simply use this URL when initializing the wallet or web3 provider instance:

Update the custom fees for a given token. If the token does not have a fee schedule, the network response returned will be `CUSTOM_SCHEDULE_ALREADY_HAS_NO_FEES`. You will need to sign the transaction with the fee schedule key to update the fee schedule for the token. If you do not have a fee schedule key set for the token, you will not be able to update the fee schedule.
{% tab title="Hedera Mainnet" %}
```
https://mainnet.hashio.io/api
```
{% endtab %}

{% tab title="Hedera Testnet" %}
```
https://testnet.hashio.io/api
```
{% endtab %}

{% tab title="Hedera Previewnet" %}
```
https://previewnet.hashio.io/api
```
{% endtab %}
{% endtabs %}

No further settings or configurations are needed!
