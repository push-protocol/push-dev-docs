# Introduction

Ethereum Push Notification Service (EPNS) is the world’s first decentralized communication & notification protocol for Web3.&#x20;

Using the protocol, any smart contract, dApp, or backend can send on-chain or off-chain notifications tied to the wallet addresses of a user in a gasless, multichain, open and platform-agnostic way.

Being an open communication middleware, notifs can be integrated and shown on any crypto wallet, mobile apps, extension, or dApps enabling native communication layer for Web3.

{% embed url="https://epns.io" %}
https://epns.io
{% endembed %}

## How to EPNS (in 10 steps or less)

1. Any user who activates themselves on the protocol to send notification is called a **Channel**.
2. In other words, a [**channel**](https://whitepaper.epns.io/protocol-specs-section/epns-protocol/channels) is any service (protocol, dApp or even web2 service) that wants to send notification out to web3 usernames (wallet addresses).
3. A wallet address can only create one [**channel**](https://whitepaper.epns.io/protocol-specs-section/epns-protocol/channels) **** on the protocol.&#x20;
4. A channel is free to delegate (or revoke delegates) sending notifications functionality to any number of other wallet addresses on their behalf as well.
5. Creating a channel requires 50 DAI and Channel info \[Channel name, Image, description, CTA] and yeah some **ETH** too.&#x20;
6. Channels can send notifications to their users(wallet addresses) in a number of ways including:&#x20;
   * [**Backend SDK**](broken-reference) (**Gasless**, Best for automated logic from dApp / Backend)
   * ****[**Showrunners Framework**](broken-reference) (**Scaffold / Gasless**, Best for automated logic via scaffold backend)
   * Smart contract to Smart contract (**requires gas**, **** Best for instant on-chain events, piggybacks on a on-chain transaction via Interface ABI call)
   * Manually from EPNS dApp (**Gasless**, Best for manual logic)
7. Users can gaslessly opt-in to receive notifications from these channels. See [**entire walkthrough here**](https://app.epns.io/#/live\_walkthrough).
8. Opted-in users are called subscribers of the channels. Subscribers of the channel receives notifications from those channels in their Inbox.
9. Non-opted users or non-subscribers of the channel aren't alerted when they receive a notif from a non-subscribed channel, instead, it lands in their spam folder.
10. Currently we have [**Staging** ](https://staging-app.epns.io/)and [**Prod** ](https://app.epns.io/)dApp that interfaces with EPNS Protocol to enable communication & notifications.

##
