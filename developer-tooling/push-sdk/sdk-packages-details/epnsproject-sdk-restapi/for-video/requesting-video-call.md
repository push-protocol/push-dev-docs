---
description: We are ready to request a video call to a wallet address
---

# Requesting video call

## Request Video Call

We are now ready to request/initiate a video call. As the name suggests this will be done on the initiator's end.

<pre class="language-typescript"><code class="lang-typescript"><strong>await videoObject.request({
</strong><strong>  senderAddress: string;
</strong>  recipientAddress: string;
  chatId: string;
  onReceiveMessage?: (message: string) => void;
  retry?: boolean;
<strong>});
</strong></code></pre>

| Property         | Description                                                                                             |
| ---------------- | ------------------------------------------------------------------------------------------------------- |
| senderAddress    | Wallet address of the local peer/user                                                                   |
| recipientAddress | Wallet address of remote peer/user ie the address which you want to call                                |
| chatId           | Unique identifier for every push chat, here, the one between the senderAddress and the recipientAddress |
| onReceiveMessage | Function which will be called when the sender receives a message via webRTC data channel                |
| retry            | If we are retrying the call, only for internal use                                                      |

{% embed url="https://www.npmjs.com/package/@pushprotocol/restapi#request" %}
request
{% endembed %}
