---
description: Send gasless notifications to wallet addresses
---

# Send Notifications

{% hint style="danger" %}
All SDK functions require passing the parameter **env** which should either pass **staging** or **prod** based on the demand. Passing anything else in this param might result in unexpected results.
{% endhint %}

{% hint style="warning" %}
The latest version of Ethers (v6) introduces some breaking changes, for best results use Ethers v5 (ethers@^5.6)
{% endhint %}

## Introduction

Once you have created a channel on Push, you can send notifications to your subscribers. There are 3 types of notifications:

* **Broadcast**: Send notifications to all the subscribers of your channel
* **Targeted**: Send notification to only one subscriber
* **Subset**: Send notifications to an array of subscribers

## **Sending Notification**

```typescript
async function sendNotification(options: {
  senderType?: 0 | 1;
  signer: any;
  type: NOTIFICATION_TYPE;
  identityType: IDENTITY_TYPE;
  notification?: {
    title: string;
    body: string;
  };
  payload?: {
    sectype?: string;
    title: string;
    body: string;
    cta: string;
    img: string;
    metadata?: any;
    additionalMeta?: any;
  };
  recipients?: string | string[]; // CAIP or plain ETH
  channel: string; // CAIP or plain ETH
  expiry?: number;
  hidden?: boolean;
  graph?: {
    id: string;
    counter: number;
  };
  ipfsHash?: string;
  env?: ENV;
  chatId?: string;
  pgpPrivateKey?: string;
})
```

#### Allowed Options (params with \* are mandatory)

<table><thead><tr><th width="210">Param</th><th width="112">Type</th><th width="101">Default</th><th>Remarks</th></tr></thead><tbody><tr><td>signer*</td><td>-</td><td>-</td><td>Signer object</td></tr><tr><td>senderType*</td><td>number</td><td>0</td><td>0 for channel notification. 1 for chat notification</td></tr><tr><td>channel*</td><td>string</td><td>-</td><td>channel address (CAIP)</td></tr><tr><td>type*</td><td>number</td><td>-</td><td>Notification Type<br>Target = 3 (send to 1 address),<br>Subset = 4 (send to 1 or more addresses),<br>Broadcast = 1 (send to all addresses)</td></tr><tr><td>identityType*</td><td>number</td><td>-</td><td>Identity Type<br>Minimal = 0,<br>IPFS = 1,<br>Direct Payload = 2,<br>Subgraph = 3 }</td></tr><tr><td>recipients*</td><td>string or string[]</td><td>-</td><td>for Notification Type = Target it is 1 address,<br>for Notification Type = Subset, Broadcast it is an array of addresses (CAIP)</td></tr><tr><td>notification.title*</td><td>string</td><td>-</td><td>Push Notification Title (not required for identityType IPFS, Subgraph)</td></tr><tr><td>notification.body*</td><td>string</td><td>-</td><td>Push Notification Body(not required for identityType IPFS, Subgraph)</td></tr><tr><td>payload.title</td><td>string</td><td>-</td><td>Notification Title(not required for identityType IPFS, Subgraph)</td></tr><tr><td>payload.body</td><td>string</td><td>-</td><td>Notification Body(not required for identityType IPFS, Subgraph)</td></tr><tr><td>payload.cta</td><td>string</td><td>-</td><td>Notification Call To Action url(not required for identityType IPFS, Subgraph)</td></tr><tr><td>payload.img</td><td>string</td><td>-</td><td>Notification Media url(not required for identityType IPFS, Subgraph)</td></tr><tr><td>payload.sectype</td><td>string</td><td>-</td><td>If Secret Notification then pass(not required for identityType IPFS, Subgraph)</td></tr><tr><td>payload.additionalMeta</td><td>{type: string; data: any; domain: string}</td><td>-</td><td>Additional information in case necessary</td></tr><tr><td>graph.id</td><td>string</td><td>-</td><td>graph id, required only if the identityType is 3</td></tr><tr><td>graph.counter</td><td>string</td><td>-</td><td>graph counter, required only if the identityType is 3</td></tr><tr><td>ipfsHash</td><td>string</td><td>-</td><td>ipfsHash, required only if the identityType is 1</td></tr><tr><td>expiry</td><td>number</td><td>-</td><td>(optional) epoch value if the notification has an expiry</td></tr><tr><td>hidden</td><td>boolean</td><td>false</td><td>(optional) true if we want to hide the notification</td></tr><tr><td>pgpPrivateKey</td><td>string</td><td>-</td><td>(optional) pgp private key for new notification verification proof</td></tr><tr><td>env</td><td>string</td><td>‘prod’</td><td>API env - ‘prod’, ‘staging’</td></tr></tbody></table>

1. Requirements before using SDK calls, derive the `signer`

{% tabs %}
{% tab title="When using Frontend" %}
```typescript
// any other web3 ui lib is also acceptable
import { useWeb3React } from "@web3-react/core";
.
.
.
const { account, library, chainId } = useWeb3React();
const signer = library.getSigner(account);
```
{% endtab %}

{% tab title="When using Backend" %}
```typescript
const ethers = require('ethers');
const PK = 'your_channel_address_secret_key';
const Pkey = `0x${PK}`;
const signer = new ethers.Wallet(Pkey);
```
{% endtab %}
{% endtabs %}

2. Call the appropriate SDK function

{% tabs %}
{% tab title="Direct Payload" %}
### Targeted Notification

```typescript
const apiResponse = await PushAPI.payloads.sendNotification({
  signer: _signer,
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

### Subset Notification

```typescript
const apiResponse = await PushAPI.payloads.sendNotification({
  signer: _signer,
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

### **Broadcast Notification**

```typescript
const apiResponse = await PushAPI.payloads.sendNotification({
  signer: _signer,
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
{% endtab %}

{% tab title="IPFS Payload" %}
### Targeted Notification

```typescript
const apiResponse = await PushAPI.payloads.sendNotification({
  signer: _signer,
  type: 3, // target
  identityType: 1, // ipfs payload
  ipfsHash: 'bafkreicuttr5gpbyzyn6cyapxctlr7dk2g6fnydqxy6lps424mcjcn73we', // IPFS hash of the payload
  recipients: 'eip155:5:0xCdBE6D076e05c5875D90fa35cc85694E1EAFBBd1', // recipient address
  channel: 'eip155:5:0xD8634C39BBFd4033c0d3289C4515275102423681', // your channel address
  env: 'staging'
});
```



### Subset Notification

```typescript
const apiResponse = await PushAPI.payloads.sendNotification({
  signer: _signer,
  type: 4, // subset
  identityType: 1, // ipfs payload
  ipfsHash: 'bafkreicuttr5gpbyzyn6cyapxctlr7dk2g6fnydqxy6lps424mcjcn73we', // IPFS hash of the payload
  recipients: ['eip155:5:0xCdBE6D076e05c5875D90fa35cc85694E1EAFBBd1', 'eip155:5:0x52f856A160733A860ae7DC98DC71061bE33A28b3'], // recipients addresses
  channel: 'eip155:5:0xD8634C39BBFd4033c0d3289C4515275102423681', // your channel address
  env: 'staging'
});
```



### **Broadcast Notification**

```typescript
const apiResponse = await PushAPI.payloads.sendNotification({
  signer: _signer,
  type: 1, // broadcast
  identityType: 1, // direct payload
  ipfsHash: 'bafkreicuttr5gpbyzyn6cyapxctlr7dk2g6fnydqxy6lps424mcjcn73we', // IPFS hash of the payload
  channel: 'eip155:5:0xD8634C39BBFd4033c0d3289C4515275102423681', // your channel address
  env: 'staging'
});
```
{% endtab %}

{% tab title="Minimal Payload" %}
### Targeted Notification

```typescript
const apiResponse = await PushAPI.payloads.sendNotification({
  signer: _signer,
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



### Subset Notification

```typescript
const apiResponse = await PushAPI.payloads.sendNotification({
  signer: _signer,
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



### **Broadcast Notification**

```typescript
const apiResponse = await PushAPI.payloads.sendNotification({
  signer: _signer,
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
{% endtab %}

{% tab title="Subgraph Payload" %}
{% hint style="info" %}
Ensure that the channel has the `graphId` being provided
{% endhint %}

### Targeted Notification

```typescript
const apiResponse = await PushAPI.payloads.sendNotification({
  signer: _signer,
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



### Subset Notification

```typescript
const apiResponse = await PushAPI.payloads.sendNotification({
  signer: _signer,
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



### **Broadcast Notification**

```typescript
const apiResponse = await PushAPI.payloads.sendNotification({
  signer: _signer,
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
{% endtab %}
{% endtabs %}
