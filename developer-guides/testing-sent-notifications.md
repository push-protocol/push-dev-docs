# Testing Sent Notifications

EPNS is an open network and supports all platforms. This means that notifications work just like how Ethereum functions. The communications are stored in an open, distributed (and soon to be decentralised) network (much like how the backend of Ethereum stores the data).&#x20;

This can then fetched by any crypto frontend / wallet / software to display it out to their users (very similar to how frontend wallets like Metamask, Trust, Phantom, etc works).

This means you can see notifications on any platform or frontend that integrates EPNS. You can test out how notifications come by using any of the crypto frontends listed below or any other third party ones that have integrated the Push protocol.

{% hint style="info" %}
The tutorial assumes you are a developer creating and sending notifications from a **staging environment**. If you are a user or have created notifications from the prod environment, then check out [https://docs.epns.io/users](https://docs.epns.io/users)
{% endhint %}

## Push Protocol Integrated Mobile Apps / Wallets / Extensions and dApps

{% hint style="success" %}
Push protocol is made for users and all its functions including opting in to receive notifications are completely gassless :tada:
{% endhint %}

### Extensions

**Step 1:** Install EPNS Staging Chrome Extension.

{% embed url="https://chrome.google.com/webstore/detail/epns-staging-protocol-alp/bjiennpmhdcandkpigcploafccldlakj" %}
**Staging Environment**
{% endembed %}

{% embed url="https://chrome.google.com/webstore/detail/epns-protocol-alpha/lbdcbpaldalgiieffakjhiccoeebchmg" %}
**Production Environment**
{% endembed %}

{% hint style="success" %}
**Next steps:** just sign in from the public wallet address with which you have opted in to receive notifications from your channel.
{% endhint %}

{% hint style="warning" %}
If you are installing the Chrome extension on **Brave** then you need to enable the following for the extension to work properly:

**Go on settings-> Privacy and Security ->Use Google services for push messaging (enable this and relaunch)**&#x20;
{% endhint %}

### dApp

{% embed url="https://staging-app.epns.io/#/channels" %}
&#x20;**Staging Environment**
{% endembed %}

{% embed url="https://app.epns.io" %}
**Production Environment**
{% endembed %}

{% hint style="success" %}
**Next steps:** just sign in from the public wallet address with which you have opted in to receive notifications from your channel.
{% endhint %}

### Mobile Apps

#### Android

{% embed url="https://play.google.com/store/apps/details?gl=US&hl=en&id=io.epns.epnsstaging" %}
**Staging Environment**
{% endembed %}

{% embed url="https://play.google.com/store/apps/details?gl=US&hl=en&id=io.epns.epns" %}
**Production Environment**
{% endembed %}

{% hint style="success" %}
**Next steps:** just sign in from the public wallet address with which you have opted in to receive notifications from your channel.
{% endhint %}

#### iOS

{% hint style="warning" %}
iOS doesn't have a public staging app (only production app is available on the App Store). \
\
Best way to test staging on iOS would be to join :point\_right: [EPNS Discord Channel](https://discord.com/invite/YVPB99F9W5) :point\_left: and ask a team member to provide you with a testflight beta link.&#x20;
{% endhint %}

{% embed url="https://discord.com/invite/YVPB99F9W5" %}
**For Requesting Staging Environment App**
{% endembed %}

{% embed url="https://apps.apple.com/us/app/ethereum-push-service-epns/id1528614910" %}
**Production Environment**
{% endembed %}
