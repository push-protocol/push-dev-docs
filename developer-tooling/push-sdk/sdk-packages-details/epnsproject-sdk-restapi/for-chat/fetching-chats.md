# Fetching Chats

## **Fetching list of user chats**

A user generally will have multiple chats associated with them (ie: them talking to multiple wallets). Use the following function to get information about all the chats that they are a part of.

{% code overflow="wrap" %}
```typescript
// pre-requisite API calls that should be made before
// need to get user and through it, the encryptedPvtKey of the user
const user = await PushAPI.user.get(account: 'eip155:0xFe6C8E9e25f7bcF374412c5C81B2578aC473C0F7');
  
// need to decrypt the encryptedPvtKey to pass in the api using helper function
const pgpDecryptedPvtKey = await PushAPI.chat.decryptPGPKey(encryptedPGPPrivateKey: user.encryptedPrivateKey, signer: _signer);
  
// actual api
const chats = await PushAPI.chat.chats({
    account: 'eip155:0xFe6C8E9e25f7bcF374412c5C81B2578aC473C0F7',
    toDecrypt: true,
    pgpPrivateKey: pgpDecryptedPvtKey
});
```
{% endcode %}

#### Allowed Options (params with \* are mandatory)

| Param         | Type    | Default | Remarks                                                                |
| ------------- | ------- | ------- | ---------------------------------------------------------------------- |
| account\*     | string  | -       | user address (Partial CAIP)                                            |
| toDecrypt     | boolean | false   | if "true" the method will return decrypted message content in response |
| pgpPrivateKey | string  | null    | mandatory for users having pgp keys                                    |
| env           | string  | 'prod'  | API env - 'prod' or 'staging'                                          |

{% embed url="https://www.npmjs.com/package/@pushprotocol/restapi#fetching-list-of-user-chats" %}
Fetching list of chats of a user
{% endembed %}

## **Fetching list of user chat requests**

Until the user approves a chat, it shows up in their chat requests folder. Use this function to retrieve the list of user chat requests.

{% code overflow="wrap" %}
```typescript
// pre-requisite API calls that should be made before
// need to get user and through that encryptedPvtKey of the user
const user = await PushAPI.user.get(account: 'eip155:0xFe6C8E9e25f7bcF374412c5C81B2578aC473C0F7');
  
// need to decrypt the encryptedPvtKey to pass in the api using helper function
const pgpDecryptedPvtKey = await PushAPI.chat.decryptPGPKey(encryptedPGPPrivateKey: user.encryptedPrivateKey, signer: _signer);
  
// actual api
const chats = await PushAPI.chat.requests({
    account: 'eip155:0xFe6C8E9e25f7bcF374412c5C81B2578aC473C0F7',
    toDecrypt: true,
    pgpPrivateKey: pgpDecryptedPvtKey
});
```
{% endcode %}

#### Allowed Options (params with \* are mandatory)

| Param         | Type    | Default | Remarks                                                                |
| ------------- | ------- | ------- | ---------------------------------------------------------------------- |
| account\*     | string  | -       | user address (Partial CAIP)                                            |
| toDecrypt     | boolean | false   | if "true" the method will return decrypted message content in response |
| pgpPrivateKey | string  | null    | mandatory for users having pgp keys                                    |
| env           | string  | 'prod'  | API env - 'prod' or 'staging'                                          |

{% embed url="https://www.npmjs.com/package/@pushprotocol/restapi#fetching-list-of-user-chat-requests" %}
To fetch chat requests of the user
{% endembed %}

## **Fetching conversation hash between two users (or group)**

Conversation hash (also known as threadhash or link) is the conversation pointer between two users of Push chat. It's used to get the chat message hash between two users.

```javascript
// conversation hash is also called link inside chat messages
const conversationHash = await PushAPI.chat.conversationHash({
  account: 'eip155:0xb340E384FC4549591bc7994b0f90074753dEC72a',
  conversationId: 'eip155:0x0F1AAC847B5720DDf01BFa07B7a8Ee641690816d' // receiver's address or chatId of a group
});
```

#### Allowed Options (params with \* are mandatory)

| Param            | Type   | Default | Remarks                                                |
| ---------------- | ------ | ------- | ------------------------------------------------------ |
| account\*        | string | -       | user address (partial CAIP)                            |
| conversationId\* | string | -       | receiver's address (partial CAIP) or chatId of a group |
| env              | string | 'prod'  | API env - 'prod' or 'staging'                          |

{% embed url="https://www.npmjs.com/package/@pushprotocol/restapi#fetching-conversation-hash-between-two-users" %}
To fetch a conversation hash between two users / group
{% endembed %}

## **Fetching latest chat between two users (or group)**

Enables fetching the latest chat message between two users (or user and group). Usually useful to display the latest message in the preview window of the chat sidebar of a dApp or crypto wallet.

```javascript
// pre-requisite API calls that should be made before
// need to get user and through that encryptedPvtKey of the user
const user = await PushAPI.user.get(account: 'eip155:0xFe6C8E9e25f7bcF374412c5C81B2578aC473C0F7');
  
// need to decrypt the encryptedPvtKey to pass in the api using helper function
const pgpDecryptedPvtKey = await PushAPI.chat.decryptPGPKey(encryptedPGPPrivateKey: user.encryptedPrivateKey, signer: _signer);

// conversation hash are also called link inside chat messages
const conversationHash = await PushAPI.chat.conversationHash({
  account: 'eip155:0xFe6C8E9e25f7bcF374412c5C81B2578aC473C0F7',
  conversationId: 'eip155:0x0F1AAC847B5720DDf01BFa07B7a8Ee641690816d' // receiver's address or chatId of a group
});
  
// actual api
const chatHistory = await PushAPI.chat.latest({
  threadhash: conversationHash.threadHash,
  account: 'eip155:0xFe6C8E9e25f7bcF374412c5C81B2578aC473C0F7',
  toDecrypt: true,
  pgpPrivateKey: decryptedPvtKey
});
```

#### Allowed Options (params with \* are mandatory)

| Param         | Type    | Default | Remarks                                                                |
| ------------- | ------- | ------- | ---------------------------------------------------------------------- |
| account\*     | string  | -       | user address (Partial CAIP)                                            |
| threadhash\*  | string  | -       | conversation hash between two users                                    |
| toDecrypt     | boolean | false   | if "true" the method will return decrypted message content in response |
| pgpPrivateKey | string  | null    | mandatory for users having pgp keys                                    |
| env           | string  | 'prod'  | API env - 'prod' or 'staging'                                          |

{% embed url="https://www.npmjs.com/package/@pushprotocol/restapi#fetching-latest-chat-between-two-users" %}
Fetching latest chat betweek users / group
{% endembed %}

## **Fetching chat history between two users (or group)**

Enables fetching chat history between two users (or group), useful in displaying the history of conversation between two users.

```javascript
// pre-requisite API calls that should be made before
// need to get user and through that encryptedPvtKey of the user
const user = await PushAPI.user.get(account: 'eip155:0xFe6C8E9e25f7bcF374412c5C81B2578aC473C0F7');
  
// need to decrypt the encryptedPvtKey to pass in the api using helper function
const pgpDecryptedPvtKey = await PushAPI.chat.decryptPGPKey(encryptedPGPPrivateKey: user.encryptedPrivateKey, signer: _signer);

// get threadhash, this will fetch the latest conversation hash
// you can also use older conversation hash (called link) by iterating over to fetch more historical messages
// conversation hash are also called link inside chat messages
const conversationHash = await PushAPI.chat.conversationHash({
  account: 'eip155:0xFe6C8E9e25f7bcF374412c5C81B2578aC473C0F7',
  conversationId: 'eip155:0x0F1AAC847B5720DDf01BFa07B7a8Ee641690816d' // receiver's address or chatId of a group
});
  
// actual api
const chatHistory = await PushAPI.chat.history({
  threadhash: conversationHash.threadHash,
  account: 'eip155:0xFe6C8E9e25f7bcF374412c5C81B2578aC473C0F7',
  limit: 2,
  toDecrypt: true,
  pgpPrivateKey: pgpDecryptedPvtKey
});
```

#### Allowed Options (params with \* are mandatory)

| Param         | Type    | Default | Remarks                                                                |
| ------------- | ------- | ------- | ---------------------------------------------------------------------- |
| account\*     | string  | -       | user address                                                           |
| threadhash\*  | string  | -       | conversation hash between two users                                    |
| toDecrypt     | boolean | false   | if "true" the method will return decrypted message content in response |
| limit         | number  | 10      | number of messages between two users                                   |
| pgpPrivateKey | string  | null    | mandatory for users having pgp keys                                    |
| env           | string  | 'prod'  | API env - 'prod', 'staging', 'dev'                                     |

{% embed url="https://www.npmjs.com/package/@pushprotocol/restapi#fetching-chat-history-between-two-users" %}
Fetching chat history between two users / group
{% endembed %}
