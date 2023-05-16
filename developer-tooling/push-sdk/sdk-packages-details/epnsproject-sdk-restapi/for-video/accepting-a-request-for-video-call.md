---
description: We fired a request for video call, time to accept it.
---

# Accepting a request for video call

## Receiving a request for a video call

In order to receive a video call request need to listen for the `USER_FEEDS` event from `@pushprotocol/socket` and use the following code inside of the event listener:

```typescript
import { VideoCallStatus } from '@pushprotocol/restapi';

epnsSDKSocket?.on(EVENTS.USER_FEEDS, (feedItem: any) => {
    const { payload } = feedItem || {};
    // we check for the additionalMeta property in payload.data
    if (payload.hasOwnProperty('data') && payload['data'].hasOwnProperty('additionalMeta')) {
        const additionalMeta = JSON.parse(payload['data']['additionalMeta']);
        if (additionalMeta.status === VideoCallStatus.INITIALIZED) {
            const {
                recipientAddress,
                senderAddress,
                chatId,
                signalingData,
                status,
            } = additionalMeta;
            // Note - signalingData is the signalData we need in the acceptRequest call
            // You can save these properties in a state for furture use
            
            /* 
                If you want you can also save these properties on the data state
                we created while initializing the video object.
                Later you can use it while calling acceptRequest()
                signalData will be available via data.meta.initiator.signal
            */
            videoObject.setData((oldData) => {
              return produce(oldData, (draft) => {
                draft.local.address = recipientAddress;
                draft.incoming[0].address = senderAddress;
                draft.incoming[0].status = PushAPI.VideoCallStatus.RECEIVED;
                draft.meta.chatId = chatId;
                draft.meta.initiator.address = senderAddress;
                draft.meta.initiator.signal = signalingData;
              });
            });
            // PS: Dont forget to import videoObject. :)
        }
    }
});
```

## Accept Request for a Video Call

After receiving a request for video call, its time to accept the request on the receiver's end. For this we'll need the `signalData` we received from the event handler of the `USER_FEEDS` event above.

<pre class="language-typescript"><code class="lang-typescript"><strong>await videoObject.acceptRequest({
</strong><strong>  signalData: any;
</strong><strong>  senderAddress: string;
</strong>  recipientAddress: string;
  chatId: string;
  onReceiveMessage?: (message: string) => void;
  retry?: boolean;
<strong>});
</strong></code></pre>

| Property         | Description                                                                                             |
| ---------------- | ------------------------------------------------------------------------------------------------------- |
| signalData       | Signal data received from the initiator peer via push notification upon request() call                  |
| senderAddress    | Wallet address of the local peer/user                                                                   |
| recipientAddress | Wallet address of remote peer/user ie the address which you want to call                                |
| chatId           | Unique identifier for every push chat, here, the one between the senderAddress and the recipientAddress |
| onReceiveMessage | Function which will be called when the sender receives a message via webRTC data channel                |
| retry            | If we are retrying the call, only for internal use                                                      |

{% embed url="https://www.npmjs.com/package/@pushprotocol/restapi#acceptrequest" %}
accept request
{% endembed %}
