---
description: >-
  A simple walkthrough guide to setup a Channel on EPNS for sending
  Notifications to your Web3 subscribers
---

# Creating A Channel

Creating a channel is the very first step for sending notifications via EPNS. Having a Channel on EPNS dApp (_and Smart Contracts_) allows you to establish a communication pathway with your users in Web3.0

While there are quite a few [protocol level details about channels](../../developer-tooling/epns-smart-contracts/epns-core-contract/channel-creation-process-on-smart-contract.md), let us first begin with understanding the overall Channel creation process.

Using the EPNS dApp or smart contracts, anyone with a wallet address on the Ethereum network can create their own channel. It can be deployed on;

1. The Ethereum Mainnet ([via Prod dApp](https://app.epns.io)), or
2. Kovan Test Network ([via Staging dApp](https://staging.epns.io))

The Prod dApp is mainly useful for fully functional dApps & smart contracts that are live on Blockchain networks. Creating your channel on Prod requires 50 DAI (yes, the real ones), and it's recommended if you have a good user base or active community who wants notifications.

The Staging dApp is very useful for Builders/Developers to test out channels, send dummy notifications, and understand the functionalities of the EPNS Protocol. In the upcoming sections, we will set up a channel on Staging App.

## Requirements for Setting Up a Channel

Ideally, there are six crucial requirements for creating a Channel. Make sure you are ready with the below list (required for both Prod and Staging)👇🏼

1. **A Channel Name**
2. **Channel Logo (**_an image of size 128px \* 128px_**)**
3. **Alias Network (**required for multi-chain, for example, if on Polygon, provide Polygon address of your contract, else can be left blank, currently supports only Polygon**)**. \
   \
   **Important:** This field needs to be provided at the very start in case you want to enable your channel on other blockchain networks, see [enabling-channel-on-other-chains](enabling-channel-on-other-chains/ "mention") for guides and to understand the process.\

4. **A brief Channel Description (**250 Characters**)**
5. **Channel CTA** (Call To Action link)
6. **An amount of 50 DAI in your Wallet (**and some ETH :innocent:**)**

{% hint style="info" %}
If you are setting up a Channel on Staging dApp, you can get Free DAI and Kovan ETH from the dApp & online faucets. No need for real DAI/ETH on the Staging app 😁
{% endhint %}

{% hint style="info" %}
If you are setting up a Channel on Prod dApp (Ethereum Mainnet), you can request for **channel creation gas fee reimbursement** within 24 hrs by [filling out this form](https://docs.google.com/forms/d/e/1FAIpQLScNQ2\_mACRQgyIPsr47woE69\_FOds8aLIGupT20QIEUMfgnQw/viewform). See [this medium article for more information](https://medium.com/ethereum-push-notification-service/calling-all-hobbyist-devs-channel-creation-gas-fee-is-now-refundable-6631ccd01baf).
{% endhint %}

## How to setup your channel

1. Ensure that you have the above requirements ready.
2. Head to [EPNS Prod dApp](https://app.epns.io) or [EPNS Staging dApp](https://staging.epns.io) based on your channel creation requirement. **Note:** Channel creation is a protocol based event which means you can also create the channel by interacting with [EPNS Core Smart Contract](../../developer-tooling/epns-smart-contracts/epns-core-contract/channel-creation-process-on-smart-contract.md).
3. Visit Create Channel and follow the instructions to create your channel. _Optionally you can visit_  [deploying-your-first-channel.md](../examples/deploying-your-first-channel.md "mention") _guide for step by step tutorial._
4. If you wish to create a channel using a Gnosis safe, please visit[creating-a-channel-using-gnosis-safe.md](channel-creation-guides/creating-a-channel-using-gnosis-safe.md "mention") and follow the guide to create one.

{% hint style="warning" %}
If you want to send notifications from other network, please make sure to check and understand [enabling-channel-on-other-chains](enabling-channel-on-other-chains/ "mention") section as you need to provide that info during your channel creation process.
{% endhint %}
