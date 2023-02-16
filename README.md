---
description: A quick introduction to Push Protocol
---

# ⭐ Getting Started

Push Protocol (EPNS) is the world’s first decentralized communication & notification protocol for Web3.

Using the protocol, any smart contract, dApp, or backend service can send on-chain or off-chain notifications tied to the wallet addresses of users in a gasless, multichain, open, and platform-agnostic way.

Being an open communication middleware, notifications can be integrated and shown on any crypto wallet, mobile app, browser extension, or dApps enabling a native communication layer for Web3.0

<details>

<summary>Quick Guide to Getting Started with Push Protocol 🛣</summary>

* Any user who activates themselves on the protocol to send a notification is called a [**Channel**](https://whitepaper.epns.io/protocol-specs-section/epns-protocol/channels).

<!---->

* In other words, a [**Channel**](https://whitepaper.epns.io/protocol-specs-section/epns-protocol/channels) is any service (protocol, dApp, or even web2 service) that wants to send notifications out to web3 usernames (wallet addresses).

<!---->

* A wallet address can create only one [**Channel**](https://whitepaper.epns.io/protocol-specs-section/epns-protocol/channels) \*\*\*\* on the protocol.

<!---->

* A channel is free to delegate (or revoke delegates) sending notifications functionality to any other wallet addresses on their behalf.

<!---->

* Creating a channel requires 50 DAI and Channel info (Channel name, Image, description, CTA) and some **ETH** too.

<!---->

* Channels can send notifications to their users(wallet addresses) in a number of ways including:
  * [**Backend SDK**](developer-tooling/epns-sdk/legacy-sdk/backend-sdk/) (**Gasless**, Best for automated logic from dApp / Backend)
  * \*\*\*\*[**Showrunners Framework**](developer-tooling/showrunners-framework/) (**Scaffold / Gasless**, Best for automated logic via scaffold backend)
  * Smart contract to Smart contract (**requires gas**, \*\*\*\* Best for instant on-chain events, piggybacks on an on-chain transaction via Interface ABI call)
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

### Developer Guides & Concepts

{% tabs %}
{% tab title="Channels" %}
**🖥** _Learn everything about channels, their working mechanisms, and how to create one._

{% content-ref url="concepts/create-your-notif-channel/" %}
[create-your-notif-channel](concepts/create-your-notif-channel/)
{% endcontent-ref %}

{% content-ref url="developer-guides/create-your-notif-channel/" %}
[create-your-notif-channel](developer-guides/create-your-notif-channel/)
{% endcontent-ref %}
{% endtab %}

{% tab title="Notifications" %}
🔔 _Explore all about notifications, their types, the different ways of sending and receiving notifications as well as other imperative details._

{% content-ref url="concepts/web3-notifications/" %}
[web3-notifications](concepts/web3-notifications/)
{% endcontent-ref %}

{% content-ref url="developer-guides/sending-notifications/" %}
[sending-notifications](developer-guides/sending-notifications/)
{% endcontent-ref %}

{% content-ref url="developer-guides/receiving-notifications/" %}
[receiving-notifications](developer-guides/receiving-notifications/)
{% endcontent-ref %}

{% content-ref url="developer-guides/sending-notifications/notification-payload-types/" %}
[notification-payload-types](developer-guides/sending-notifications/notification-payload-types/)
{% endcontent-ref %}
{% endtab %}

{% tab title="SDK" %}
⚙ _Understand the key features of EPNS SDK and how to use it in your own project easily._&#x20;

{% content-ref url="developer-tooling/push-sdk/" %}
[push-sdk](developer-tooling/push-sdk/)
{% endcontent-ref %}

{% content-ref url="developer-tooling/push-sdk/epns-sdk-starter-kit.md" %}
[epns-sdk-starter-kit.md](developer-tooling/push-sdk/epns-sdk-starter-kit.md)
{% endcontent-ref %}
{% endtab %}

{% tab title="Push Chat" %}
📝 _Learn about the details of Push Chat and how to easily integrate it._

{% content-ref url="concepts/push-chat-for-web3.md" %}
[push-chat-for-web3.md](concepts/push-chat-for-web3.md)
{% endcontent-ref %}

{% content-ref url="developer-guides/integrating-push-chat.md" %}
[integrating-push-chat.md](developer-guides/integrating-push-chat.md)
{% endcontent-ref %}

{% content-ref url="developer-tooling/push-sdk/sdk-packages-details/epnsproject-sdk-uiweb/push-support-chat.md" %}
[push-support-chat.md](developer-tooling/push-sdk/sdk-packages-details/epnsproject-sdk-uiweb/push-support-chat.md)
{% endcontent-ref %}
{% endtab %}

{% tab title="Showrunners" %}
_🛠 Learn about the showrunners framework and how to use it to build out notifications for your specific use cases._

{% content-ref url="developer-tooling/showrunners-framework/" %}
[showrunners-framework](developer-tooling/showrunners-framework/)
{% endcontent-ref %}

{% content-ref url="developer-guides/sending-notifications/using-showrunners-scaffold-gasless.md" %}
[using-showrunners-scaffold-gasless.md](developer-guides/sending-notifications/using-showrunners-scaffold-gasless.md)
{% endcontent-ref %}
{% endtab %}
{% endtabs %}

