---
description: This package gives access to EPNS backend APIs
---

# @epnsproject/sdk-restapi

### Installation <a href="#installation" id="installation"></a>

```
  yarn add @epnsproject/sdk-restapi ethers
```

or

```
  npm install @epnsproject/sdk-restapi ethers
```

I**mport in your file**

```
import * as EpnsAPI from "@epnsproject/sdk-restapi";
```

{% hint style="info" %}
**Note on Addresses:**\
****

_EPNS SDK uses **CAIP10** format but defaults to Ethereum address format._\
__\
_CAIP 10 format is a way to identify multichain addresses which are extended from CAIP 2. Any blockchain address becomes namespace + “:” + chain\_id + “:” + account\_address._\
__\
_For instance:_

* The Polygon mainnet **namespace is** **eip155** _and its_ **chain\_id is 137** for the mainnet. Therefore, its CAIP10 address shall be
  * P_olygon mainnet:_\
    __**eip155:137:0x0000000000000000000000000000000000000000**
  * _Ethereum mainnet:_\
    **eip155:1:0x0000000000000000000000000000000000000000**

_0x0000000000000000000000000000000000000000 can be replaced by any address of the user through which you are communicating_\
__

In any of the `restapi` methods (unless explicitly stated otherwise) we accept either -

* [CAIP format](https://github.com/ChainAgnostic/CAIPs/blob/master/CAIPs/caip-10.md#test-cases): for any on-chain addresses _**We strongly recommend using this address format**_. (Example : `eip155:1:0xab16a96d359ec26a11e2c2b3d8f8b8942d5bfcdb`)
* ETH address format: only for backwards compatibility. (Example: `0xab16a96d359ec26a11e2c2b3d8f8b8942d5bfcdb`)
{% endhint %}

### Features

As of now, the @epnsproject/sdk-restapi package provides us with 3 imperative types of features:

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
