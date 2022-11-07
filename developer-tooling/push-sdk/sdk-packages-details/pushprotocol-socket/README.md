---
description: >-
  This package helps to connect to Push backend using websockets built on top of
  Socket.IO
---

# @pushprotocol/socket

## socket

#### Installation

```
  yarn add @pushprotocol/socket ethers
```

or

```
  npm install @pushprotocol/socket ethers 
```

Import in your file

```
import {
  createSocketConnection,
  EVENTS
} from '@pushprotocol/socket';
```

**First create a socket connection object**

```
const pushSDKSocket = createSocketConnection({
  user: 'eip155:5:0xab16a96d359ec26a11e2c2b3d8f8b8942d5bfcdb', // CAIP, see below
  env: 'staging',
  socketOptions: { autoConnect: false }
});
```

{% hint style="info" %}
**IMPORTANT**: \
_Create the connection object in your app only when you have the `user` address available since it's mandatory._
{% endhint %}

**`autoConnect`**: Generally, if we don't pass `autoConnect: false` then the socket connection is automatic once the object is created.&#x20;

Since we may or may not have the account address handy and wish to start the connection during instantiation so this option makes it easier for us to choose when we want to `connect` or not.

_Allowed Options (params with \* are mandatory)_

| Param         | Type   | Default | Remarks                                                                            |
| ------------- | ------ | ------- | ---------------------------------------------------------------------------------- |
| user\*        | string | -       | user account address (CAIP)                                                        |
| env           | string | 'prod'  | API env - 'prod', 'staging', 'dev'                                                 |
| socketOptions | object | -       | supports the same as [SocketIO Options](https://socket.io/docs/v4/client-options/) |

**Connect the socket connection object**

```
pushSDKSocket.connect();
```

**Disconnect the socket connection object**

```
pushSDKSocket.disconnect();
```

**Subscribing to Socket Events**

```
pushSDKSocket.on(EVENTS.CONNECT, () => {

});

pushSDKSocket.on(EVENTS.DISCONNECT, () => {

});

pushSDKSocket.on(EVENTS.USER_FEEDS, (feedItem) => {
  // feedItem is the notification data when that notification was received
});
```

### Supported EVENTS

| EVENT name               | When is it triggered?                                                                                     |
| ------------------------ | --------------------------------------------------------------------------------------------------------- |
| EVENTS.CONNECT           | whenever the socket is connected                                                                          |
| EVENTS.DISCONNECT        | whenever the socket is disconneted                                                                        |
| EVENTS.USER\_FEEDS       | whenever a new notification is received by the user after the last socket connection                      |
| EVENTS.USER\_SPAM\_FEEDS | <p>whenever a new spam notification is received by the user after the last socket connection.<br><br></p> |

{% hint style="info" %}


**Note on Addresses:**\
****

PUSH _SDK uses **CAIP10** format but defaults to Ethereum address format._\
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
