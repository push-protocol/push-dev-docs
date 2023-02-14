---
description: This package gives access to Push ptrotocol backend APIs
---

# @pushprotocol/restapi

### Installation <a href="#installation" id="installation"></a>

```bash
  yarn add @pushprotocol/restapi ethers
```

or

```bash
  npm install @pushprotocol/restapi ethers 
```

I**mport in your file**

```typescript
import * as PushAPI from "@pushprotocol/restapi";
```

{% hint style="info" %}
**Note on Addresses:**

PUSH SDK uses [**CAIP-10**](https://github.com/ChainAgnostic/CAIPs/blob/master/CAIPs/caip-10.md) format but defaults to Ethereum address format.

CAIP-10 format is a way to identify multichain addresses. Any blockchain address becomes`namespace + “:” + chain_id + “:” + account_address`



**Example:**

For both Ethereum and Polygon, the namespace is `eip155` and the chain\_id is `1` and `5` respectively:

* Polygon mainnet:

`eip155:137:0x20E0e045F1baB5CD9284602bdf24D050Dc8CB719`

* Ethereum mainnet:

`eip155:1:0x20E0e045F1baB5CD9284602bdf24D050Dc8CB719`

For Goerli and Mumbai testnets, the namespace is `eip155` and the chain\_id is `5` and `80001` respectively:

* Goerli testnet:

`eip155:5:0x20E0e045F1baB5CD9284602bdf24D050Dc8CB719`

* Mumbai testnet:

`eip155:80001:0x20E0e045F1baB5CD9284602bdf24D050Dc8CB719`

``

In any of the `restapi` methods (unless explicitly stated otherwise) we accept either -

* [CAIP format](https://github.com/ChainAgnostic/CAIPs/blob/master/CAIPs/caip-10.md#test-cases): for any on-chain addresses _**We strongly recommend using this address format**_. (Example : `eip155:1:0xab16a96d359ec26a11e2c2b3d8f8b8942d5bfcdb`)
* ETH address format: only for backwards compatibility. (Example: `0xab16a96d359ec26a11e2c2b3d8f8b8942d5bfcdb`)
{% endhint %}

### Features

As of now, the @pushprotocol/restapi package provides us with 3 imperative types of features:

1. **Fetching User/Channel Details**&#x20;

It helps us with fetching the following data:

* &#x20;User _notifications_
* _User Spam Notifications_
* _User Subscriptions_
* _Channel Details_
* _Channel(s) search_

**2. Opt-In and Opt-Out**&#x20;

**3. Sending Notifications**

_Let's explore each one of these._

{% content-ref url="../../../epns-sdk/sdk-packages-details/epnsproject-sdk-restapi/fetching-user-channel-details.md" %}
[fetching-user-channel-details.md](../../../epns-sdk/sdk-packages-details/epnsproject-sdk-restapi/fetching-user-channel-details.md)
{% endcontent-ref %}

{% content-ref url="opt-in-and-opt-out.md" %}
[opt-in-and-opt-out.md](opt-in-and-opt-out.md)
{% endcontent-ref %}

{% content-ref url="../../../epns-sdk/sdk-packages-details/epnsproject-sdk-restapi/send-notifications.md" %}
[send-notifications.md](../../../epns-sdk/sdk-packages-details/epnsproject-sdk-restapi/send-notifications.md)
{% endcontent-ref %}

{% content-ref url="../../../epns-sdk/sdk-packages-details/epnsproject-sdk-restapi/utils.md" %}
[utils.md](../../../epns-sdk/sdk-packages-details/epnsproject-sdk-restapi/utils.md)
{% endcontent-ref %}
