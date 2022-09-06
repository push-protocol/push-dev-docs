---
description: >-
  This is an introductory guide to take you through the entire process of how to
  send notifications from a Subgraph using EPNS.
---

# Using Subgraph (Gasless)

## Introduction: The Graph Protocol **&** Subgraphs

**The Graph** is a decentralized protocol for indexing and querying data from blockchains, starting with Ethereum. It makes it possible to query data that is difficult to query directly.

A **Subgraph** defines which data **The Graph** will index from a blockchain, and how it will store it. Once deployed, it will form a part of a global graph of blockchain data which you can retrieve using GraphQL.

Currently, EPNS only supports the subgraphs deployed on **Hosted Service of The Graph Protocol**. Providing support to Subgraph Studio would be part of the next iteration.

For more information on how to deploy a subgraph on the hosted service for your smart contract or dApp, check out [this documentation](https://thegraph.com/docs/en/hosted-service/deploy-subgraph-hosted/).

## Notifications from Subgraphs 💡

Subgraphs retrieve and store data from the blockchain for a particular smart contract. This data can be used to analyze a variety of things related to the smart contract.

For example, the [**Uniswap Subgraph**](https://thegraph.com/hosted-service/subgraph/uniswap/uniswap-v2) stores data related to the total volume across all trading pairs, volume data per trading pair, and even data for a particular token.

**What if you intelligently fetch the data from a Subgraph and generate useful alerts 🤔?**

This will be extremely helpful for the end-users of your dApp and entities connected to your smart contract making the user experience smoother.

## Sending Notifications using EPNS

EPNS has developed an in-house `Helper Function` specifically for The Graph Protocol that allows you to read events from the Subgraph and define notifications accordingly. Once defined, they will be stored on the Subgraph in a `Long String` format.

EPNS Push Nodes can, later on, fetch the notifications defined on a Subgraph and push them accordingly to Subscribers of the Channel.

![Flow diagram of how notifications are sent ](<../../.gitbook/assets/image (10).png>)

## Setting up EPNS with Subgraph

In order to integrate EPNS with your Subgraph, you will need:

1. Your Subgraph ID
2. A Channel deployed on EPNS

In case you don’t have the Subgraph ID, feel free to create your own Subgraph by following the step-by-step guide available here at [Create a Subgraph](https://thegraph.com/docs/en/developer/create-subgraph-hosted/).

Once you have published your Subgraph, you can start sending notifications based on events generated from the Smart Contract to the users.

Enough talking, let’s get into Building stuff right away by following the imperative steps below.

### 1. **Initialize Subgraph with EPNS**

* Navigate to the Subgraph directory and you’ll find `schema.graphql` file. Open in an editor of your choice and include the following EPNS Schema:

```graphql
type EpnsNotificationCounter @entity {
  id: ID!
  totalCount: BigInt!
}

type EpnsPushNotification @entity {
  id: ID!
  notificationNumber: BigInt!
  recipient: String!
  notification: String!
}
```

* In `mapping.ts` file under `src/` directory, export the Subgraph ID:

```typescript
//Note: EPNS only supports The Graph Hosted Service at present

export const subgraphID = "GithubID/subgraph-slug"
```

> _Note: Make sure the above step is complete, as Subgraph ID will be imported in the next step!_

* Create a file named `EPNSNotification.ts` in the `src/` folder. We’ll call this our Helper File. Now, copy the below-provided TypeScript code and paste it into the newly created Helper file:

```typescript
import { BigInt, log } from "@graphprotocol/graph-ts"
import { EpnsNotificationCounter, EpnsPushNotification } from '../generated/schema'
import { subgraphID } from "./mapping"

export function sendEPNSNotification(recipient: string, notification: string): void 
{
  let id1 = subgraphID
  log.info('New id of EpnsNotificationCounter is: {}', [id1])

  let epnsNotificationCounter = EpnsNotificationCounter.load(id1)
  if (epnsNotificationCounter == null) {
    epnsNotificationCounter = new EpnsNotificationCounter(id1)
    epnsNotificationCounter.totalCount = BigInt.fromI32(0)
  }
  epnsNotificationCounter.totalCount = (epnsNotificationCounter.totalCount).plus(BigInt.fromI32(1))

  let count = epnsNotificationCounter.totalCount.toHexString()
  let id2 = `${subgraphID}+${count}`
  log.info('New id of EpnsPushNotification is: {}', [id2])

  let epnsPushNotification = EpnsPushNotification.load(id2)
  if (epnsPushNotification == null) {
    epnsPushNotification = new EpnsPushNotification(id2)
  }

  epnsPushNotification.recipient = recipient
  epnsPushNotification.notification = notification
  epnsPushNotification.notificationNumber = epnsNotificationCounter.totalCount

  epnsPushNotification.save()
  epnsNotificationCounter.save()
}
```

* In `mapping.ts` present in `src/`, import the Helper File:

```typescript
import { sendEPNSNotification } from "./EPNSNotification"
```

### **2. Define Payload Items**

In the event handler method of `mapping.ts` file, define your notification payload items such as recipient of the notification, type, title, message, etc. These variables will be further used to define our notification variable.

```typescript
let recipient = "0xD8634C39BBFd4033c0d3289C4515275102423681",
	  type = "3",
	  title = "Number changed",
	  body = `Number changed from ${event.params.from} to ${event.params.to}`,
	  subject = "Number changed",
	  message = `Number changed from ${event.params.from} to ${event.params.to}`,
	  image = "null",
	  secret = "null",
	  cta = "https://epns.io/"
```

It’s highly recommended to take a look at [this documentation](https://docs.epns.io/developers/developer-zone/sending-notifications/advanced/notification-payload-types) to understand more about payload items and their definitions.

For a quick reference, the `recipient` differs with the payload type. For example, **broadcast** (type = 1) and special multi-payload notifications have the **channel address** as the `recipient`.

### **3. Define Notification**

The `notification` variable is defined in the given below format 👇🏼

**Format : **<mark style="color:green;">**`{"field" : "value"}`**</mark>

```typescript
notification = `{\"type\": \"${type}\", \"title\": \"${title}\", \"body\": \"${body}\", \"subject\": \"${subject}\", \"message\": \"${message}\", \"image\": \"${image}\", \"secret\": \"${secret}\", \"cta\": \"${cta}\"}`
```

### **4. Call the EPNS Helper Function**

Once the above steps are complete, we need to invoke the EPNS helper function and send the response. To call the EPNS Notification helper function, use the below script;

```typescript
sendEPNSNotification (recipient, notification)
```

## Alright! Let's Add Subgraph to EPNS Channel

Once you have set up EPNS integration into your subgraph, you must add the subgraph to its channel in order to deliver notifications. You will require the **Subgraph ID** for this purpose.

It is a slug usually present at the end of the subgraph URL 😉

```atom
https://thegraph.com/hosted-service/subgraph/aiswaryawalter/graph-poc-sample
```

If you are a **Channel Owner**, follow the below steps;

1. Go to **EPNS Dapp** ([https://staging.epns.io/](https://staging.epns.io/)) → Channel Dashboard → Settings Button → Add Subgraph Details
2. Enter your `Subgraph ID` and `Poll Interval`

Poll Interval (in seconds) defines the time period at which Push Nodes shall ping the subgraph for fetching the latest notifications.

**Note:** _This is an on-chain transaction that stores the above data to **EPNS Core Contract.** So it requires $ETH for gas fees._

> _If you don’t have a channel yet, you can easily create one by following this guide_ [_here_](https://docs.epns.io/developers/developer-zone/create-your-notif-channel)_._

![Add Subgraph details page in Channel Settings section on the EPNS DApp](<../../.gitbook/assets/image (26).png>)

🎉 Congratulations on successfully integrating EPNS Helper Function into your Subgraph, and also adding Subgraph details into your channel.

Once done, **EPNS Push Nodes** will fetch the subgraph info from the Core Contract and start polling the respective subgraph for notifications at regular Poll Intervals.



Alright!, in order to dive in a bit more deeply into this, let's try to set up a notification procedure for an ERC-20 token. Learn more about this example exercise [here](https://docs.epns.io/developers/developer-zone/examples/notification-via-subgraph-example).
