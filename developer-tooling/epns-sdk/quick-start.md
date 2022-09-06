---
description: A super quick guide to get you started with EPNS SDK
---

# Quick Start

Now that you have a basic understanding of what EPNS SDK is, let's go through an absolutely easy procedure of how to set it up and use the package in your code.

### Installation and Set-Up

1. Let's start by creating new project set-up

```
mkdir sdk-quickstart

cd sdk-quickstart

# at sdk-quickstart, hit enter for all if no change from the default intended
yarn init 
```

> _**Note**:_ If you wish to use ES6 Modules syntax, then inside package.json set **“type” to “module”.**

2\. **Installation**

<pre><code># install the sdk "restapi" package &#x26; its peer dependencies in your app
<strong>
</strong><strong>yarn add @epnsproject/sdk-restapi ethers
</strong>
# or, 

npm install @epnsproject/sdk-restapi ethers</code></pre>

### Usage

In order to demonstrate a simple example of how to use the SDK, we shall try to use a feature from our **@epnsproject/sdk-restapi** package.

> _Note: This is just one of the many features of our SDK for the sake of this example._ \
> _You can refer to the_ _subsequent sections for details on all the_ [.](./ "mention")__

#### Fetching Notifications

For this example, we shall try to use the **getFeeds()** feature from the **sdk-restapi** package.&#x20;

This simply allows us to get all the notifications for a particular user address on a specific chain.

```typescript
import * as EpnsAPI from "@epnsproject/sdk-restapi";

const fetchNotifs = async() => {
    const notifications = await EpnsAPI.user.getFeeds({
        user: 'eip155:42:0xD8634C39BBFd4033c0d3289C4515275102423681', // user address in CAIP
        env: 'staging'
    });

    console.log('Notifications: \n\n', notifications);
}

fetchNotifs();
```

#### Note on CAIP

We use [CAIP](https://github.com/ChainAgnostic/CAIPs/blob/master/CAIPs/caip-10.md) format for any on-chain addresses to pass to the SDK methods.

#### Fetching Spam Notifications

The above-mentioned example gives you all the notifications that a given user received in his/her inbox.&#x20;

However, now let's fetch the spam notification received by a given user address.

```typescript
import * as EpnsAPI from "@epnsproject/sdk-restapi";

const fetchSpam = async() => {
    const spams = await EpnsAPI.user.getFeeds({
        user: 'eip155:42:0xD8634C39BBFd4033c0d3289C4515275102423681', // user address in CAIP
        spam: true,
        env: 'staging'
    });

    console.log('Spams: \n\n', spams);
}

fetchSpam();
```

Notice, how we simply need to add an additional argument, i.e., _**`spam: true`**_to get the spam notifications for the given address.

Perfect! 👏

Now that you have a good understanding of how to use the SDK, you are all set to explore its other interesting features of the SDK and build incredibly amazing tools using the same. 🦾
