---
description: This section describes how to send a notification using the Push SDK.
---

# Using Push SDK (Gasless)

### Installation

Let's create a basic app by entering the following in the terminal.

```bash
mkdir sdk-quickstart

cd sdk-quickstart

# at sdk-quickstart, hit enter for all if no change from the default intended
yarn init

# or, 

npm init 
```

{% hint style="warning" %}
_**Note**:_ If you wish to use ES6 Modules syntax, then inside `package.json` set **“type” to “module”.**
{% endhint %}

```bash
# install the sdk "restapi" package & its peer dependencies in your app
yarn add @pushprotocol/restapi ethers

# or, 

npm install @pushprotocol/restapi ethers
```

### Sample Usage

{% hint style="warning" %}
While using the SDK, you will need to provide the environment settings in env, the only acceptable values for them are  '**prod**', '**staging**' or '**dev**'. Using other values might result in validation errors.&#x20;
{% endhint %}

The below snippet will help us to send notification to the user **0xCdBE6D076e05c5875D90fa35cc85694E1EAFBBd1** from the channel **0xD8634C39BBFd4033c0d3289C4515275102423681**

```typescript
import * as PushAPI from "@pushprotocol/restapi";
import * as ethers from "ethers";


const PK = 'your_channel_address_secret_key'; // channel private key
const Pkey = `0x${PK}`;
const signer = new ethers.Wallet(Pkey);

const sendNotification = async() => {
  try {
    const apiResponse = await PushAPI.payloads.sendNotification({
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
      recipients: 'eip155:5:0xCdBE6D076e05c5875D90fa35cc85694E1EAFBBd1', // recipient address
      channel: 'eip155:5:0xD8634C39BBFd4033c0d3289C4515275102423681', // your channel address
      env: 'staging'
    });
    
    // apiResponse?.status === 204, if sent successfully!
    console.log('API repsonse: ', apiResponse);
  } catch (err) {
    console.error('Error: ', err);
  }
}

sendNotification();
```

Similarly we have _**different use cases**_ for sending notifications as follows -&#x20;

**direct payload for single recipient(target)**

```typescript
// apiResponse?.status === 204, if sent successfully!
const apiResponse = await PushAPI.payloads.sendNotification({
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
  recipients: 'eip155:5:0xD8634C39BBFd4033c0d3289C4515275102423681', // recipient address
  channel: 'eip155:5:0xD8634C39BBFd4033c0d3289C4515275102423681', // your channel address
  env: 'staging'
});
```

**direct payload for group of recipients(subset)**

```typescript
// apiResponse?.status === 204, if sent successfully!
const apiResponse = await PushAPI.payloads.sendNotification({
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
  recipients: ['eip155:5:0xD8634C39BBFd4033c0d3289C4515275102423681', 'eip155:5:0xCdBE6D076e05c5875D90fa35cc85694E1EAFBBd1'], // recipients addresses
  channel: 'eip155:5:0xD8634C39BBFd4033c0d3289C4515275102423681', // your channel address
  env: 'staging'
});
```

**direct payload for all recipients(broadcast)**

```typescript
// apiResponse?.status === 204, if sent successfully!
const apiResponse = await PushAPI.payloads.sendNotification({
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
  channel: 'eip155:5:0xD8634C39BBFd4033c0d3289C4515275102423681', // your channel address
  env: 'staging'
});
```

**IPFS payload for single recipient(target)**

```typescript
// apiResponse?.status === 204, if sent successfully!
const apiResponse = await PushAPI.payloads.sendNotification({
  signer,
  type: 3, // target
  identityType: 1, // ipfs payload
  ipfsHash: 'bafkreicuttr5gpbyzyn6cyapxctlr7dk2g6fnydqxy6lps424mcjcn73we', // IPFS hash of the payload
  recipients: 'eip155:5:0xCdBE6D076e05c5875D90fa35cc85694E1EAFBBd1', // recipient address
  channel: 'eip155:5:0xD8634C39BBFd4033c0d3289C4515275102423681', // your channel address
  env: 'staging'
});
```

**IPFS payload for group of recipients(subset)**

```typescript
// apiResponse?.status === 204, if sent successfully!
const apiResponse = await PushAPI.payloads.sendNotification({
  signer,
  type: 4, // subset
  identityType: 1, // ipfs payload
  ipfsHash: 'bafkreicuttr5gpbyzyn6cyapxctlr7dk2g6fnydqxy6lps424mcjcn73we', // IPFS hash of the payload
  recipients: ['eip155:5:0xCdBE6D076e05c5875D90fa35cc85694E1EAFBBd1', 'eip155:5:0x52f856A160733A860ae7DC98DC71061bE33A28b3'], // recipients addresses
  channel: 'eip155:5:0xD8634C39BBFd4033c0d3289C4515275102423681', // your channel address
  env: 'staging'
});
```

**IPFS payload for all recipients(broadcast)**

```typescript
// apiResponse?.status === 204, if sent successfully!
const apiResponse = await PushAPI.payloads.sendNotification({
  signer,
  type: 1, // broadcast
  identityType: 1, // direct payload
  ipfsHash: 'bafkreicuttr5gpbyzyn6cyapxctlr7dk2g6fnydqxy6lps424mcjcn73we', // IPFS hash of the payload
  channel: 'eip155:5:0xD8634C39BBFd4033c0d3289C4515275102423681', // your channel address
  env: 'staging'
});
```

**minimal payload for single recipient(target)**

```typescript
// apiResponse?.status === 204, if sent successfully!
const apiResponse = await PushAPI.payloads.sendNotification({
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
  recipients: 'eip155:5:0xCdBE6D076e05c5875D90fa35cc85694E1EAFBBd1', // recipient address
  channel: 'eip155:5:0xD8634C39BBFd4033c0d3289C4515275102423681', // your channel address
  env: 'staging'
});
```

**minimal payload for a group of recipient(subset)**

```typescript
// apiResponse?.status === 204, if sent successfully!
const apiResponse = await PushAPI.payloads.sendNotification({
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
  recipients: ['eip155:5:0xCdBE6D076e05c5875D90fa35cc85694E1EAFBBd1', 'eip155:5:0x52f856A160733A860ae7DC98DC71061bE33A28b3'], // recipients address
  channel: 'eip155:5:0xD8634C39BBFd4033c0d3289C4515275102423681', // your channel address
  env: 'staging'
});
```

**minimal payload for all recipients(broadcast)**

```typescript
// apiResponse?.status === 204, if sent successfully!
const apiResponse = await PushAPI.payloads.sendNotification({
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
  channel: 'eip155:5:0xD8634C39BBFd4033c0d3289C4515275102423681', // your channel address
  env: 'staging'
});
```

**graph payload for single recipient(target)**

{% hint style="warning" %}
_**Make sure the channel has the graph id you are providing!!**_
{% endhint %}

```typescript
// apiResponse?.status === 204, if sent successfully!
const apiResponse = await PushAPI.payloads.sendNotification({
  signer,
  type: 3, // target
  identityType: 3, // Subgraph payload
  graph: {
    id: '_your_graph_id',
    counter: 3
  },
  recipients: 'eip155:5:0xCdBE6D076e05c5875D90fa35cc85694E1EAFBBd1', // recipient address
  channel: 'eip155:5:0xD8634C39BBFd4033c0d3289C4515275102423681', // your channel address
  env: 'staging'
});
```

**graph payload for group of recipients(subset)**

{% hint style="warning" %}
_**Make sure the channel has the graph id you are providing!!**_
{% endhint %}

```typescript
// apiResponse?.status === 204, if sent successfully!
const apiResponse = await PushAPI.payloads.sendNotification({
  signer,
  type: 4, // subset
  identityType: 3, // graph payload
  graph: {
    id: '_your_graph_id',
    counter: 3
  },
  recipients: ['eip155:5:0xCdBE6D076e05c5875D90fa35cc85694E1EAFBBd1', 'eip155:5:0x52f856A160733A860ae7DC98DC71061bE33A28b3'], // recipients addresses
  channel: 'eip155:5:0xD8634C39BBFd4033c0d3289C4515275102423681', // your channel address
  env: 'staging'
});
```

**graph payload for all recipients(broadcast)**

{% hint style="warning" %}
_**Make sure the channel has the graph id you are providing!!**_
{% endhint %}

```typescript
// apiResponse?.status === 204, if sent successfully!
const apiResponse = await PushAPI.payloads.sendNotification({
  signer,
  type: 1, // broadcast
  identityType: 3, // graph payload
  graph: {
    id: '_your_graph_id',
    counter: 3
  },
  channel: 'eip155:5:0xD8634C39BBFd4033c0d3289C4515275102423681', // your channel address
  env: 'staging'
});
```

#### Allowed Options (params with \* are mandatory)

| Param                | Type                | Default | Remarks                                                                                                                                           |
| -------------------- | ------------------- | ------- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| signer\*             | -                   | -       | Signer object                                                                                                                                     |
| channel\*            | string              | -       | channel address (CAIP)                                                                                                                            |
| type\*               | number              | -       | <p>Notification Type<br>Target = 3 (send to 1 address),<br>Subset = 4 (send to 1 or more addresses),<br>Broadcast = 1 (send to all addresses)</p> |
| identityType\*       | number              | -       | <p>Identity Type<br>Minimal = 0,<br>IPFS = 1,<br>Direct Payload = 2,<br>Subgraph = 3 }</p>                                                        |
| recipients\*         | string or string\[] | -       | <p>for Notification Type = Target it is 1 address,<br>for Notification Type = Subset, Broadcast it is an array of addresses (CAIP)</p>            |
| notification.title\* | string              | -       | Push Notification Title     (not required for identityType IPFS, Subgraph)                                                                        |
| notification.body\*  | string              | -       | Push Notification Body(not required for identityType IPFS, Subgraph)                                                                              |
| payload.title        | string              | -       | Notification Title(not required for identityType IPFS, Subgraph)                                                                                  |
| payload.body         | string              | -       | Notification Body(not required for identityType IPFS, Subgraph)                                                                                   |
| payload.cta          | string              | -       | Notification Call To Action url(not required for identityType IPFS, Subgraph)                                                                     |
| payload.img          | string              | -       | Notification Media url(not required for identityType IPFS, Subgraph)                                                                              |
| payload.sectype      | string              | -       | If Secret Notification then pass(not required for identityType IPFS, Subgraph)                                                                    |
| graph.id             | string              | -       | graph id, required only if the identityType is 3                                                                                                  |
| graph.counter        | string              | -       | graph counter, required only if the identityType is 3                                                                                             |
| ipfsHash             | string              | -       | ipfsHash, required only if the identityType is 1                                                                                                  |
| expiry               | number              | -       | (optional) epoch value if the notification has an expiry                                                                                          |
| hidden               | boolean             | false   | (optional) true if we want to hide the notification                                                                                               |
| env                  | string              | 'prod'  | API env - 'prod', 'staging', 'dev'                                                                                                                |
