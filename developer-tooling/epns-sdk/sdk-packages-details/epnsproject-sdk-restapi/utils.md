---
description: Helper functions
---

# Utils

## **Notifications**

### **Parse notifications**

Utils method to parse raw Push Feeds API response into a pre-defined shape as below.

```typescript
// fetch some raw feeds data
const apiResponse = await PushAPI.user.getFeeds({
  user: 'eip155:5:0xD8634C39BBFd4033c0d3289C4515275102423681', // user address
  raw: true,
  env: 'staging'
});
// parse it to get a specific shape of object.
const parsedResults = PushAPI.utils.parseApiResponse(apiResponse);

const [oneNotification] = parsedResults;

// Now this object can be directly used by for e.g. "@pushprotocol/uiweb"  NotificationItem component as props.
const {
  cta,
  title,
  message,
  app,
  icon,
  image,
  url,
  blockchain,
  secret,
  notification
} = oneNotification;
```

_We get the above `keys` after the parsing of the API response._

## Chat

### **Decrypting encrypted pgp private key**

```typescript
import { IUser } from '@pushprotocol/restapi';

const decryptedPvtKey = await PushAPI.chat.decryptWithWalletRPCMethod(
          (connectedUser as IUser).encryptedPrivateKey, //encrypted private key 
          '0xFe6C8E9e25f7bcF374412c5C81B2578aC473C0F7' //user address
        );
```

### **Decrypting messages**

```typescript
import { IUser } from '@pushprotocol/restapi';

const decryptedChat = await PushAPI.chat.decryptConversation({
    messages: chatHistory, //array of message object fetched from chat.history method
    connectedUser, // user meta data object fetched from chat.get method
    pgpPrivateKey:decryptedPvtKey, //decrypted private key
    env:'staging',
  });
```

| Param           | Type   | Default  | Remarks                                                  |
| --------------- | ------ | -------- | -------------------------------------------------------- |
| env             | string | `'prod'` | API env: `'prod'`, `'staging'`, `'dev'`                  |
| messages\*      | string | -        | array of message object fetched from chat.history method |
| connectedUser\* | IUser  | `false`  | user meta data object                                    |
| pgpPrivateKey   | string | `null`   | mandatory for users having pgp keys                      |
