---
description: Finally we connect the video call.
---

# Connecting a video call

## Connect a video call

Now, to finally connect a video call on the initiator's end, we need to listen for the `USER_FEEDS` event from `@pushprotocol/socket` and use the following code inside of the event listener:

<pre class="language-typescript"><code class="lang-typescript"><strong>pushSDKSocket?.on(EVENTS.USER_FEEDS, (feedItem: any) => {
</strong>    const { payload } = feedItem || {};
    // we check for the additionalMeta property in payload.data
    if (payload.hasOwnProperty('data') &#x26;&#x26; payload['data'].hasOwnProperty('additionalMeta')) {
        const additionalMeta = JSON.parse(payload['data']['additionalMeta']);
        if (additionalMeta.status === VideoCallStatus.RECEIVED) {
            const {
                signalingData,
            } = additionalMeta;
            
            // connecting the call using received signalData
            videoObject.connect({
                signalData: signalingData
            })
            // PS: Dont forget to import videoObject. :)
        }
    }
});
</code></pre>

<pre class="language-typescript"><code class="lang-typescript"><strong>videoObject.acceptRequest({
</strong><strong>  signalData: any;
</strong><strong>});
</strong></code></pre>

| Property   | Description                                                                                 |
| ---------- | ------------------------------------------------------------------------------------------- |
| signalData | Signal data received from the receiver peer via push notification upon acceptRequest() call |

{% embed url="https://www.npmjs.com/package/@pushprotocol/restapi#connect" %}
connect
{% endembed %}
