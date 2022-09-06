# Send Notifications

### **sending notification**

_**NOTE on generating the "signer" object for different platforms:**_

When using in SERVER-SIDE code:

```typescript
const ethers = require('ethers');
const PK = 'your_channel_address_secret_key';
const Pkey = `0x${PK}`;
const signer = new ethers.Wallet(Pkey);
```

When using in FRONT-END code:

```typescript
// any other web3 ui lib is also acceptable
import { useWeb3React } from "@web3-react/core";
.
.
.
const { account, library, chainId } = useWeb3React();
const signer = library.getSigner(account);
```

### DIFFERENT USE CASES FOR `sendNotification()`

#### **direct payload for a single recipient(target)**

```typescript
// apiResponse?.status === 204, if sent successfully!
const apiResponse = await EpnsAPI.payloads.sendNotification({
  signer,
  type: 3, // target
  identityType: 2, // direct payload
  notification: {
    title: `[SDK-TEST] notification TITLE:`,
    body: `[sdk-test] notification BODY`
  },
  payload: {
    title: `[sdk-test] payload title`,
    body: `sample msg body`,
    cta: '',
    img: ''
  },
  recipients: 'eip155:42:0xD8634C39BBFd4033c0d3289C4515275102423681', // recipient address
  channel: 'eip155:42:0xD8634C39BBFd4033c0d3289C4515275102423681', // your channel address
  env: 'staging'
});
```

#### **direct payload for a group of recipients(subset)**

```typescript
// apiResponse?.status === 204, if sent successfully!
const apiResponse = await EpnsAPI.payloads.sendNotification({
  signer,
  type: 4, // subset
  identityType: 2, // direct payload
  notification: {
    title: `[SDK-TEST] notification TITLE:`,
    body: `[sdk-test] notification BODY`
  },
  payload: {
    title: `[sdk-test] payload title`,
    body: `sample msg body`,
    cta: '',
    img: ''
  },
  recipients: ['eip155:42:0xD8634C39BBFd4033c0d3289C4515275102423681', 'eip155:42:0xCdBE6D076e05c5875D90fa35cc85694E1EAFBBd1'], // recipients addresses
  channel: 'eip155:42:0xD8634C39BBFd4033c0d3289C4515275102423681', // your channel address
  env: 'staging'
});
```

#### **direct payload for all recipients(broadcast)**

```typescript
// apiResponse?.status === 204, if sent successfully!
const apiResponse = await EpnsAPI.payloads.sendNotification({
  signer,
  type: 1, // broadcast
  identityType: 2, // direct payload
  notification: {
    title: `[SDK-TEST] notification TITLE:`,
    body: `[sdk-test] notification BODY`
  },
  payload: {
    title: `[sdk-test] payload title`,
    body: `sample msg body`,
    cta: '',
    img: ''
  },
  channel: 'eip155:42:0xD8634C39BBFd4033c0d3289C4515275102423681', // your channel address
  env: 'staging'
});
```

#### **IPFS payload for single recipient(target)**

```typescript
// apiResponse?.status === 204, if sent successfully!
const apiResponse = await EpnsAPI.payloads.sendNotification({
  signer,
  type: 3, // target
  identityType: 1,
  ipfsHash: 'bafkreicuttr5gpbyzyn6cyapxctlr7dk2g6fnydqxy6lps424mcjcn73we', // IPFS hash of the payload
  recipients: 'eip155:42:0xCdBE6D076e05c5875D90fa35cc85694E1EAFBBd1', // recipient address
  channel: 'eip155:42:0xD8634C39BBFd4033c0d3289C4515275102423681', // your channel address
  env: 'staging'
});
```

#### **IPFS payload for group of recipients(subset)**

```typescript
// apiResponse?.status === 204, if sent successfully!
const apiResponse = await EpnsAPI.payloads.sendNotification({
  signer,
  type: 4, // subset
  identityType: 1,
  ipfsHash: 'bafkreicuttr5gpbyzyn6cyapxctlr7dk2g6fnydqxy6lps424mcjcn73we', // IPFS hash of the payload
  recipients: ['eip155:42:0xCdBE6D076e05c5875D90fa35cc85694E1EAFBBd1', 'eip155:42:0x52f856A160733A860ae7DC98DC71061bE33A28b3'], // recipients addresses
  channel: 'eip155:42:0xD8634C39BBFd4033c0d3289C4515275102423681', // your channel address
  env: 'staging'
});
```

#### **IPFS payload for all recipients(broadcast)**

```typescript
// apiResponse?.status === 204, if sent successfully!
const apiResponse = await EpnsAPI.payloads.sendNotification({
  signer,
  type: 1, // broadcast
  identityType: 1, // direct payload
  ipfsHash: 'bafkreicuttr5gpbyzyn6cyapxctlr7dk2g6fnydqxy6lps424mcjcn73we', // IPFS hash of the payload
  channel: 'eip155:42:0xD8634C39BBFd4033c0d3289C4515275102423681', // your channel address
  env: 'staging'
});
```

#### **minimal payload for single recipient(target)**

```typescript
// apiResponse?.status === 204, if sent successfully!
const apiResponse = await EpnsAPI.payloads.sendNotification({
  signer,
  type: 3, // target
  identityType: 0, // Minimal payload
  notification: {
    title: `[SDK-TEST] notification TITLE:`,
    body: `[sdk-test] notification BODY`
  },
  payload: {
    title: `[sdk-test] payload title`,
    body: `sample msg body`,
    cta: '',
    img: ''
  },
  recipients: 'eip155:42:0xCdBE6D076e05c5875D90fa35cc85694E1EAFBBd1', // recipient address
  channel: 'eip155:42:0xD8634C39BBFd4033c0d3289C4515275102423681', // your channel address
  env: 'staging'
});
```

#### **minimal payload for a group of recipient(subset)**

```typescript
// apiResponse?.status === 204, if sent successfully!
const apiResponse = await EpnsAPI.payloads.sendNotification({
  signer,
  type: 4, // subset
  identityType: 0, // Minimal payload
  notification: {
    title: `[SDK-TEST] notification TITLE:`,
    body: `[sdk-test] notification BODY`
  },
  payload: {
    title: `[sdk-test] payload title`,
    body: `sample msg body`,
    cta: '',
    img: ''
  },
  recipients: ['eip155:42:0xCdBE6D076e05c5875D90fa35cc85694E1EAFBBd1', 'eip155:42:0x52f856A160733A860ae7DC98DC71061bE33A28b3'], // recipients address
  channel: 'eip155:42:0xD8634C39BBFd4033c0d3289C4515275102423681', // your channel address
  env: 'staging'
});
```

#### **minimal payload for all recipients(broadcast)**

```typescript
// apiResponse?.status === 204, if sent successfully!
const apiResponse = await EpnsAPI.payloads.sendNotification({
  signer,
  type: 1, // broadcast
  identityType: 0, // Minimal payload
  notification: {
    title: `[SDK-TEST] notification TITLE:`,
    body: `[sdk-test] notification BODY`
  },
  payload: {
    title: `[sdk-test] payload title`,
    body: `sample msg body`,
    cta: '',
    img: ''
  },
  channel: 'eip155:42:0xD8634C39BBFd4033c0d3289C4515275102423681', // your channel address
  env: 'staging'
});
```

### **Graph Payloads**

#### **graph payload for a single recipient(target)**

{% hint style="info" %}
_Ensure that the channel has the **graphId** being provided._
{% endhint %}

```typescript
// apiResponse?.status === 204, if sent successfully!
const apiResponse = await EpnsAPI.payloads.sendNotification({
  signer,
  type: 3, // target
  identityType: 3, // Subgraph payload
  graph: {
    id: '_your_graph_id',
    counter: 3
  },
  recipients: 'eip155:42:0xCdBE6D076e05c5875D90fa35cc85694E1EAFBBd1', // recipient address
  channel: 'eip155:42:0xD8634C39BBFd4033c0d3289C4515275102423681', // your channel address
  env: 'staging'
});
```

#### **graph payload for a group of recipients(subset)**

{% hint style="info" %}
_Ensure that the channel has the **graphId** being provided._
{% endhint %}

```typescript
// apiResponse?.status === 204, if sent successfully!
const apiResponse = await EpnsAPI.payloads.sendNotification({
  signer,
  type: 4, // subset
  identityType: 3, // graph payload
  graph: {
    id: '_your_graph_id',
    counter: 3
  },
  recipients: ['eip155:42:0xCdBE6D076e05c5875D90fa35cc85694E1EAFBBd1', 'eip155:42:0x52f856A160733A860ae7DC98DC71061bE33A28b3'], // recipients addresses
  channel: 'eip155:42:0xD8634C39BBFd4033c0d3289C4515275102423681', // your channel address
  env: 'staging'
});
```

**graph payload for all recipients(broadcast)**

{% hint style="info" %}
_Ensure that the channel has the **graphId** being provided._
{% endhint %}

```typescript
// apiResponse?.status === 204, if sent successfully!
const apiResponse = await EpnsAPI.payloads.sendNotification({
  signer,
  type: 1, // broadcast
  identityType: 3, // graph payload
  graph: {
    id: '_your_graph_id',
    counter: 3
  },
  channel: 'eip155:42:0xD8634C39BBFd4033c0d3289C4515275102423681', // your channel address
  env: 'staging'
});
```

Allowed Options (params with \* are mandatory)

| Param                        | Type                | Default | Remarks                                                                                                                                           |
| ---------------------------- | ------------------- | ------- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| signer\*                     | -                   | -       | Signer object                                                                                                                                     |
| channel\*                    | string              | -       | channel address (CAIP)                                                                                                                            |
| type\*                       | number              | -       | <p>Notification Type<br>Target = 3 (send to 1 address),<br>Subset = 4 (send to 1 or more addresses),<br>Broadcast = 1 (send to all addresses)</p> |
| identityType\*               | number              | -       | <p>Identity Type<br>Minimal = 0,<br>IPFS = 1,<br>Direct Payload = 2,<br>Subgraph = 3 }</p>                                                        |
| recipients\*                 | string or string\[] | -       | <p>for Notification Type = Target it is 1 address,<br>for Notification Type = Subset, Broadcast it is an array of addresses (CAIP)</p>            |
| notification.title\*         | string              | -       | Push Notification Title (not required for identityType IPFS, Subgraph)                                                                            |
| notification.body\*          | string              | -       | Push Notification Body(not required for identityType IPFS, Subgraph)                                                                              |
| payload.title                | string              | -       | Notification Title(not required for identityType IPFS, Subgraph)                                                                                  |
| payload.body                 | string              | -       | Notification Body(not required for identityType IPFS, Subgraph)                                                                                   |
| payload.cta                  | string              | -       | Notification Call To Action url(not required for identityType IPFS, Subgraph)                                                                     |
| payload.img                  | string              | -       | Notification Media url(not required for identityType IPFS, Subgraph)                                                                              |
| payload.sectype              | string              | -       | If Secret Notification then pass(not required for identityType IPFS, Subgraph)                                                                    |
| [graph.id](http://graph.id/) | string              | -       | graph id, required only if the identityType is 3                                                                                                  |
| graph.counter                | string              | -       | graph counter, required only if the identityType is 3                                                                                             |
| ipfsHash                     | string              | -       | ipfsHash, required only if the identityType is 1                                                                                                  |
| expiry                       | number              | -       | (optional) epoch value if the notification has an expiry                                                                                          |
| hidden                       | boolean             | false   | (optional) true if we want to hide the notification                                                                                               |
| env                          | string              | ‘prod’  | API env - ‘prod’, ‘staging’, ‘dev’                                                                                                                |
