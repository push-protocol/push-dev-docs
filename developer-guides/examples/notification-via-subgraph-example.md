# Notification via Subgraph example

Alright, now that we have an effective understanding of sending notifications via subgraph and the setup procedure involved for the same, it's time to dive in deep and understand this wonderful feature with an example.

### The objective of this exercise

This is a walkthrough of how one can set up **Subgraph based Push Notifications**. Even if you are not much familiar with Push or The Graph, you could straight away start from here and follow the respective links to dive in.&#x20;

In this example, we integrate PUSH protocol with an ERC-20 contract subgraph and link it to an Push Notification Channel **to send out notifications whenever a Transfer happens**.

### Pre-requisites

1. Have an Push protocol notification Channel ready - see the docs [here](https://docs.epns.io/developers/developer-zone/create-your-notif-channel) to create a channel
2. Install the **graph CLI**

{% tabs %}
{% tab title="npm" %}
```bash
npm install -g @graphprotocol/graph-cli
```
{% endtab %}

{% tab title="yarn" %}
```bash
yarn global add @graphprotocol/graph-clias
```
{% endtab %}
{% endtabs %}

3\. Link your Github to the graph website -[https://thegraph.com/hosted-service/](https://thegraph.com/hosted-service/)

4\. Add a subgraph named XXXX from your Hosted Service Dashboard -[https://thegraph.com/hosted-service/dashboard](https://thegraph.com/hosted-service/dashboard)

### **Contract deployment**

1. Copy the ERC-20 contract -[https://github.com/aiswaryawalter/epns-graph-token-alerter/blob/main/contracts/PushToken.sol](https://github.com/aiswaryawalter/epns-graph-token-alerter/blob/main/contracts/PushToken.sol)
2. Deploy on the Goerli Test Network using the Remix IDE - [https://remix.ethereum.org/](https://remix.ethereum.org/\*\*)

### **Subgraph deployment**

#### Create a subgraph

1. Clone the ERC-20 token subgraph -[https://github.com/aiswaryawalter/erc-20-token-subgraph.git](https://github.com/aiswaryawalter/erc-20-token-subgraph.git)
2. In `subgraph.yaml` update the contract address to the one you just deployed
3.  Install packages

    ```bash
    yarn install
    ```
4.  Generate code

    ```bash
    graph codegen
    ```
5.  Get the access token from the Graph dashboard & authenticate

    ```bash
    graph auth --product hosted-service <ACCESS_TOKEN>
    ```
6.  Deploy the subgraph

    ```bash
    graph deploy --product hosted-service <GITHUB_USER>/<SUBGRAPH NAME>
    ```
7. See the subgraph playground

#### Integrate Push to the subgraph

1.  In `schema.graphql`, include the following Push Schema

    ```jsx
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
2.  In `mapping.ts`, export the subgraph ID

    ```typescript
    //Note: EPNS supports only The Graph Hosted Service at present

    export const subgraphID = "aiswaryawalter/push-token"
    ```
3. In `src/` create a file - `EPNSNotification.ts`. Copy & paste the Helper File code from[https://github.com/aiswaryawalter/epns-graph-token-alerter/blob/main/src/EPNSNotification.ts](https://github.com/aiswaryawalter/epns-graph-token-alerter/blob/main/src/EPNSNotification.ts)&#x20;
4.  In `mapping.ts` import the helper file

    ```jsx
    import { sendEPNSNotification } from "./EPNSNotification"
    ```

Follow steps 5, 6 and 7 within the respective handler functions from which the notification needs to be sent👇🏼

5\. Define Notification Payload Items

```jsx
let recipient = event.params.to.toHexString(),
  type = "3",
  title = "PUSH Received",
  body = `Received ${event.params.tokens.div(power)} PUSH from ${event.params.from.toHexString()}`,
  subject = "PUSH Received",
  message = `Received ${event.params.tokens.div(power)} PUSH from ${event.params.from.toHexString()}`,
  image = "https://play-lh.googleusercontent.com/i911_wMmFilaAAOTLvlQJZMXoxBF34BMSzRmascHezvurtslYUgOHamxgEnMXTklsF-S",
  secret = "null",
  cta = "https://epns.io/"
```

6\. Define Notification

The `notification` variable is defined in the given below format 👇🏼

**Format : `{"field" : "value"}`**

```jsx
let notification = `{\"type\": \"${type}\", \"title\": \"${title}\", \"body\": \"${body}\", \"subject\": \"${subject}\", \"message\": \"${message}\", \"image\": \"${image}\", \"secret\": \"${secret}\", \"cta\": \"${cta}\"}`
```

7\. Call the Push Helper Function

```jsx
sendEPNSNotification (recipient, notification)
```

#### Redeploy Subgraph

1.  Generate code

    ```jsx
    graph codegen
    ```
2.  Get the access token from the Graph dashboard & authenticate

    ```jsx
    graph auth --product hosted-service <ACCESS_TOKEN>
    ```
3.  Deploy the subgraph

    ```jsx
    graph deploy --product hosted-service <GITHUB_USER>/<SUBGRAPH NAME>
    ```
4.  In the subgraph playground, paste the below **query** & show the notification payloads

    ```jsx
    {
    epnsPushNotifications(first: 20) {
    id
    notificationNumber
    recipient
    notification
    }
    }
    ```



Here is the final subgraph with PUSH integration - [https://github.com/aiswaryawalter/epns-graph-token-alerter](https://github.com/aiswaryawalter/epns-graph-token-alerter)

### **Linking Subgraph to** Push **Notification Channel**

* Go to PUSH dApp ([https://staging.push.org/](https://staging.push.org/)) → Channel Dashboard → Settings → Add Subgraph Details
*   Enter your `Subgraph ID`_(githubID/subgraph-slug)_ and `Poll Interval` _(in seconds, must be at least 60 sec or more)_

    **Poll Interval** (in seconds) defines the time period at which Push Nodes shall ping the subgraph for fetching the latest notifications.

    **Note:** This is an on-chain transaction that stores the above data to **Push Core Contract.** So it requires $ETH for gas fees.

### **Testing Notification**

1. &#x20;`Opt-in` to the newly created channel
2. Initiate a `Transfer` transaction&#x20;
3. Wait for the notification to appear on the recipient's wallet via Push dApp, Mobile App, Browser Extension
