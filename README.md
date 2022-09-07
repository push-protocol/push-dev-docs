
# ⭐ Getting Started

Ethereum Push Notification Service (EPNS) is the world’s first decentralized communication & notification protocol for Web3.&#x20;

Using the protocol, any smart contract, dApp, or backend service can send on-chain or off-chain notifications tied to the wallet addresses of users in a gasless, multichain, open, and platform-agnostic way.

Being an open communication middleware, notifications can be integrated and shown on any crypto wallet, mobile app, browser extension, or dApps enabling a native communication layer for Web3.0

<details>

<summary>Quick Guide to Getting Started with EPNS 🛣</summary>

* Any user who activates themselves on the protocol to send a notification is called a [**Channel**](https://whitepaper.epns.io/protocol-specs-section/epns-protocol/channels).

<!---->

* In other words, a [**Channel**](https://whitepaper.epns.io/protocol-specs-section/epns-protocol/channels) is any service (protocol, dApp, or even web2 service) that wants to send notifications out to web3 usernames (wallet addresses).

<!---->

* A wallet address can create only one [**Channel**](https://whitepaper.epns.io/protocol-specs-section/epns-protocol/channels) **** on the protocol.&#x20;

<!---->

* A channel is free to delegate (or revoke delegates) sending notifications functionality to any other wallet addresses on their behalf.

<!---->

* Creating a channel requires 50 DAI and Channel info (Channel name, Image, description, CTA) and some **ETH** too.&#x20;

<!---->

* Channels can send notifications to their users(wallet addresses) in a number of ways including:&#x20;
  * [**Backend SDK**](developer-tooling/epns-sdk/legacy-sdk/backend-sdk/) (**Gasless**, Best for automated logic from dApp / Backend)
  * ****[**Showrunners Framework**](developer-tooling/showrunners-framework/) (**Scaffold / Gasless**, Best for automated logic via scaffold backend)
  * Smart contract to Smart contract (**requires gas**, **** Best for instant on-chain events, piggybacks on an on-chain transaction via Interface ABI call)
  * Manually from EPNS dApp (**Gasless**, Best for manual logic)

<!---->

* Users can gaslessly opt-in to receive notifications from these Channels. See the [**entire walkthrough here**](https://app.epns.io/#/live\_walkthrough).

<!---->

* Opted-in users are called subscribers of the Channels. Subscribers of the Channel receive notifications from those Channels in their Inboxes.

<!---->

* Non-opted users or non-subscribers of the Channel aren't alerted when they receive a notif from a non-subscribed channel, instead, it lands in their spam folder.

<!---->

* Currently, we have [**Staging** ](https://staging.epns.io/)and [**Prod** ](https://app.epns.io/)dApp that interfaces with EPNS Protocol to enable communication & notifications.

</details>

## Developer Guides & Concepts 

### 1. Channels
*🖥 Lear️n everything about channels, their working mechanisms, and how to create one.*

* [What are Channels?](https://docs.epns.io/developers/concepts/create-your-notif-channel)
* [How to create a Channel?](https://docs.epns.io/developers/developer-guides/create-your-notif-channel)


### 2. Notificatins
*🔔 Explore all about notifications, their types, the different ways of sending and receiving notifications as well as other imperative details.*

* [Web3 Notificatins](https://docs.epns.io/developers/concepts/web3-notifications)
* [Different ways of Sending Notifications](https://docs.epns.io/developers/developer-guides/sending-notifications)
* [Receiving Notifications](https://docs.epns.io/developers/developer-guides/receiving-notifications)
* [Notification Standards](https://docs.epns.io/developers/developer-guides/sending-notifications/notification-payload-types)


### 3. EPNS SDK
*⚙ Understand the key features of EPNS SDK and how to use it in your own project easily.*
* [EPNS SDK](https://docs.epns.io/developers/developer-tooling/epns-sdk)
* [EPNS SDK Starter Kit](https://docs.epns.io/developers/developer-tooling/epns-sdk/epns-sdk-starter-kit) 

