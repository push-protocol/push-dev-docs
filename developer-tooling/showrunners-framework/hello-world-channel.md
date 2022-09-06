---
description: >-
  This guide explains how to create your very own hello world channel on top of
  the showrunners scaffold. Do read Setup Showrunners incase you haven't as the
  guide is dependent on it.
---

# Hello World Channel

Please ensure that you have correctly installed showrunners using the [**Setup Showrunners**](how-to-setup-showrunners.md) guide before proceeding forward.

## Setting up Hello World Channel

Hello World channel exists to simply demonstrate how easy it is to send notifications in Web3. To setup the channel, you will need to do the following things.

1. Ensure that you have [**created your channel**](../../developer-guides/create-your-notif-channel/) and make a note of the private key used for the same.&#x20;
2. Head to `src/sample_showrunners` from the git clone of **epns-showrunners-framework-staging** which you had done earlier.
3. Copy the folder `helloWorld` and drop it in `src/showrunners` folder.
4. You might notice that the showrunners has already moved to complain about `helloWorldKeys.json` not containing the correct private key.\
   \
   `{`\
   &#x20;   `"PRIVATE_KEY_1": "YOUR_CHANNEL_PRIVATE_KEY_HERE"` \
   `}`\
   ``&#x20;
5. Simply copy and paste the private key of your channel instead of `YOUR_CHANNEL_PRIVATE_KEY_HERE` and you should be presented with the following screen.

![](<../../.gitbook/assets/Screen Shot 2022-05-09 at 1.10.56 PM.png>)

Congrats, your channel is setup and good to go. Let's look at what components of your channel folders enable what functionality next!
