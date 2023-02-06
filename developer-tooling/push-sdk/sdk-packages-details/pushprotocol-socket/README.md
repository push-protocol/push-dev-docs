---
description: >-
  This package helps to connect to Push backend using websockets built on top of
  Socket.IO
---

# @pushprotocol/socket

## Introduction

The `@pushprotocol/socket` package contains all the WebSocket implementation for Push Protocol.

The socket client will allow you to receive both notifications and chat messages.

### Events

The Push SDK contains an `EVENT` object that contains all the supported events. Here below are they:

* `EVENT.CONNECT`: Once the connection is established
* `EVENT.DISCONNECT`: Once the socket is disconnected
* `EVENT.USER_FEEDS`: Whenever a new notification is received by the user after the last socket connection
* `EVENT.USER_SPAM_FEEDS`: Whenever a new spam notification is received by the user after the last socket connection.
* `EVENT.CHAT_RECEIVED_MESSAGE`: To receive new messages or intents

{% hint style="info" %}
**Note on Addresses:**

_PUSH SDK uses **CAIP10** format but defaults to Ethereum address format._

_CAIP 10 format is a way to identify multichain addresses which are extended from CAIP 2. Any blockchain address becomes namespace + “:” + chain\_id + “:” + account\_address._



For instance:

* The Polygon mainnet **namespace is** **eip155** _and its_ **chain\_id is 137** for the mainnet. Therefore, its CAIP10 address shall be&#x20;
  * P_olygon mainnet:_ **eip155:137:0x0000000000000000000000000000000000000000**
  * Ethereum mainnet:\
    **eip155:1:0x0000000000000000000000000000000000000000**

__\
_0x0000000000000000000000000000000000000000 can be replaced by any address of the user through which you are communicating._\
__

In any of the `restapi` methods (unless explicitly stated otherwise) we accept either -

* [CAIP format](https://github.com/ChainAgnostic/CAIPs/blob/master/CAIPs/caip-10.md#test-cases): **We strongly recommend using this address format for any on-chain addresses**. (Example : `eip155:1:0xab16a96d359ec26a11e2c2b3d8f8b8942d5bfcdb`)\


For chat-related restapis, the address is in the format: \
**eip155:\<address>** instead of **eip155:\<chainId>:\<address>**

ETH address format: only for backward compatibility. \
(Example: 0xab16a96d359ec26a11e2c2b3d8f8b8942d5bfcdb)
{% endhint %}

### Integration

For now, when instantiating the socket client, you should define the socket type: `notification` or `chat`, with the notification being the default.

#### Installation

```
yarn add @pushprotocol/socket ethers
```

or

```
npm install @pushprotocol/socket ethers
```

#### For notification

```typescript
const pushSDKSocket = createSocketConnection({
  user: 'eip155:5:0xab16a96d359ec26a11e2c2b3d8f8b8942d5bfcdb', // CAIP, see below
  env: 'staging',
  socketOptions: { autoConnect: false }
});
```

#### For Chat&#x20;

```typescript
const pushSDKSocket = createSocketConnection({
    user: 'eip155:0xFd6C2fE69bE13d8bE379CCB6c9306e74193EC1A9',
    env: 'staging',
    apiKey: 'jVPMCRom1B.iDRMswdehJG7NpHDiECIHwYMMv6k2KzkPJscFIDyW8TtSnk4blYnGa8DIkfuacU0',
    socketType: 'chat',
    socketOptions: { autoConnect: true, reconnectionAttempts: 3 }
});
```

{% hint style="warning" %}
**`autoConnect`**: Generally, if we don't pass `autoConnect: false` then the socket connection is automatic once the object is created.

Since we may or may not have the account address handy and wish to start the connection during instantiation, this option makes it easier for us to choose when we want to `connect` or not.
{% endhint %}

### Subscribing to Socket Events

```jsx
pushSDKSocket?.on(EVENTS.CONNECT, () => { //Your logic here })
pushSDKSocket?.on(EVENTS.DISCONNECT, (err) => console.log(err));
pushSDKSocket?.on(EVENTS.CHAT_RECEIVED_MESSAGE, (message) => console.log(message))
pushSDKSocket?.on(EVENTS.USER_FEEDS, (notification) => console.log(notification))
pushSDKSocket?.on(EVENTS.USER_SPAM_FEEDS, (spam) => console.log(spam))
```

### Integration with Push Chat

The **`EVENTS.CHAT_RECEIVED_MESSAGE`** event for new messages on Push Chat will only send the new messages after the connection was established. If you want to display the previous messages, use the `@pushprotocol/restapi` package to first fetch all the previous messages from a conversation between two wallets then use them `socket` to display the latest messages received.
