---
description: Send and get chat messages
---

# Chat

## Create User

To start messaging wallet addresses, you need to create your encryption keys.

```typescript
const user = await PushAPI.user.create({
   account: '0xFe6C8E9e25f7bcF374412c5C81B2578aC473C0F7'
});
```

## Get User Data

Get user data such as name, profile picture, and encryption keys.

```typescript
const user = await PushAPI.user.get({
   account: '0xFe6C8E9e25f7bcF374412c5C81B2578aC473C0F7'
});
```

## Get Chats

Return the **latest message** from all wallet addresses you have talked to. This is used when building the inbox page.

```typescript
const chats = await PushAPI.chat.chats({
    account: 0xFe6C8E9e25f7bcF374412c5C81B2578aC473C0F7,
    pgpPrivateKey: decryptedPvtKey,
    toDecrypt: true
});
```

Allowed Options (params with \* are mandatory)

| Param         | Type    | Default  | Remarks                                                                |
| ------------- | ------- | -------- | ---------------------------------------------------------------------- |
| account\*     | string  | -        | user address                                                           |
| env           | string  | `'prod'` | API env: `'prod'`, `'staging'`, `'dev'`                                |
| toDecrypt     | boolean | `false`  | if `true` the method will return decrypted message content in response |
| pgpPrivateKey | string  | `null`   | mandatory for users having pgp keys                                    |

## Get Requests

The first time an address wants to send a message to another peer, the address sends an intent request. This first message shall not land in this peer Inbox but in its Request box.  &#x20;

This function will return all the chats that landed on the address' Request box. The user can then `approve` the request or ignore it for now.

```typescript
const chats = await PushAPI.chat.requests({
    account: 0xFe6C8E9e25f7bcF374412c5C81B2578aC473C0F7,
    pgpPrivateKey: decryptedPvtKey,
    toDecrypt: true
});
```

Allowed Options (params with \* are mandatory)

| Param         | Type    | Default  | Remarks                                                                |
| ------------- | ------- | -------- | ---------------------------------------------------------------------- |
| account\*     | string  | -        | user address                                                           |
| env           | string  | `'prod'` | API env: `'prod'`, `'staging'`, `'dev'`                                |
| toDecrypt     | boolean | `false`  | if `true` the method will return decrypted message content in response |
| pgpPrivateKey | string  | `null`   | mandatory for users having pgp keys                                    |

## Get conversation hash between users

All chat messages are stored on IPFS. This function will return the latest message's CID (Content Identifier on IPFS). Whenever a new message is sent, this CID will change.

```typescript
const threadhash = await PushAPI.chat.conversationHash({
        account: '20x18C0Ab0809589c423Ac9eb42897258757b6b3d3d',
        conversationId: '0xFA3F8E79fb9B03e7a04295594785b91588Aa4DC8', // receiver's address or chatId of a group
});
```

## Get chat history between users

Get all the messages exchanged between users after the `threadhash.`

```typescript
const chatHistory = await PushAPI.chat.history({
  threadhash:threadhash.threadHash,
  account: '0xFe6C8E9e25f7bcF374412c5C81B2578aC473C0F7',
  pgpPrivateKey: decryptedPvtKey,
  limit:2,
  toDecrypt:true
});
```

| Param         | Type    | Default	 | Remarks                                                                |
| ------------- | ------- | -------- | ---------------------------------------------------------------------- |
| account\*     | string  | -        | user address                                                           |
| env           | string  | '`prod'` | API env: `'prod'`, `'staging'`, `'dev'`                                |
| threadhash\*  | string  | -        | conversation hash between two users                                    |
| toDecrypt     | boolean | false    | if "true" the method will return decrypted message content in response |
| pgpPrivateKey | string  | null     | mandatory for users having pgp keys                                    |
| limit         | number  | 10       | number of messages between two users                                   |

## Fetch the latest chat message between user

```typescript
const chatHistory = await PushAPI.chat.latest({
    threadhash:threadhash.threadHash,
    account: '0xFe6C8E9e25f7bcF374412c5C81B2578aC473C0F7',
    pgpPrivateKey: decryptedPvtKey,
    limit:2,
    toDecrypt:true
});
```

## Approve Chat Request

```typescript
const response = await PushAPI.chat.approve({
        status: 'Approved',
        account: '0x18C0Ab0809589c423Ac9eb42897258757b6b3d3d',
        senderAddress : '0x873a538254f8162377296326BB3eDDbA7d00F8E9', // receiver's address or chatId of a group
});
```

## Send Message

```typescript
const response = await PushAPI.chat.send({
        messageContent: 'Hi',
        messageType: 'Text',
        receiverAddress: '0x08E834a388Cee21d4d7571075146841C8eE621a4', // receiver's address or chatId of a group
        account: '0x57eAd5826B1E0A7074E1aBf1A062714A2dE0f8B4',
        pgpPrivateKey: decryptedPvtKey,
        apiKey:"tAWEnggQ9Z.UaDBNjrvlJZx3giBTIQDcT8bKQo1O1518uF1Tea7rPwfzXv2ouV5rX9ViwgJUrXm"
});
```

| Param           | Type                              | Default  | Remarks                                 |
| --------------- | --------------------------------- | -------- | --------------------------------------- |
| account\*       | string                            | -        | user address                            |
| env             | string                            | `'prod'` | API env: `'prod'`, `'staging'`, `'dev'` |
| senderAddress\* | string                            | -        | receiver's address or chatId of a group |
| messageContent  | string                            | `''`     | message to be sent                      |
| messageType     | `'Text'` \| `'Image'` \| `'File'` | `'Text'` | type of messageContent                  |
| pgpPrivateKey   | string                            | `null`   | mandatory for users having pgp keys     |
| apiKey          | string                            | `''`     | apiKey for using chat                   |

## Create Group

Create Group Chats to interact with your friends and community

```typescript
const response = await PushAPI.chat.createGroup({
    groupName: 'Push Protocol group',
    groupDescription: 'This is the oficial group for Push Protocol',
    members: ['0x9e60c47edF21fa5e5Af33347680B3971F2FfD464','0x3829E53A15856d1846e1b52d3Bdf5839705c29e5'],
    groupImage: 'group image link',
    admins: ['0x3829E53A15856d1846e1b52d3Bdf5839705c29e5'],
    isPublic: true,
    groupCreator: '0xD993eb61B8843439A23741C0A3b5138763aE11a4',
    account: '0xD993eb61B8843439A23741C0A3b5138763aE11a4',
    pgpPrivateKey: decryptedPvtKey //decrypted private key
});
```

## Get Group

Get Group information

```typescript
const response = await PushAPI.chat.getGroup({
  chatId: '190591e84108cdf12e62eecabf02ddb123ea92f1c06fb98ee9b5cf3871f46fa9'
});
```

## Update Group

```typescript
const response = await PushAPI.chat.updateGroup({
        groupName:'Push Chat group',
        groupDescription:'This is the updated description for Push Chat,
        members: ['0x2e60c47edF21fa5e5A333347680B3971F1FfD456','0x3829E53A15856d1846e1b52d3Bdf5839705c29e5'],
        groupImage: &lt;group image link&gt; ,
        admins: ['0x3829E53A15856d1846e1b52d3Bdf5839705c29e5'],
        account: '0xD993eb61B8843439A23741C0A3b5138763aE11a4',
        pgpPrivateKey: decryptedPvtKey, //decrypted private key
});
```

| Param              | Type   | Default	 | Remarks                                                        |
| ------------------ | ------ | -------- | -------------------------------------------------------------- |
| account\*          | string | -        | user address                                                   |
| env                | string | `'prod'` | API env: `'prod'`, `'staging'`, `'dev'`                        |
| groupName\*        | string | -        | group name                                                     |
| groupDescription\* | string | -        | group description                                              |
| groupImage\*       | string | -        | group image link                                               |
| members\*          | Array  | -        | wallet addresses of all members except admins and groupCreator |
| admins\*           | Array  | -        | wallet addresses of all admins except members and groupCreator |
| pgpPrivateKey      | string | `null`   | mandatory for users having pgp keys                            |
