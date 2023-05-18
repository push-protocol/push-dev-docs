---
description: Wanna hang up? Here you go.
---

# Disconnecting a video call

## Disconnect a video call

To disconnect a call we use the `disconnect()` method on the videoObject.

<pre class="language-typescript"><code class="lang-typescript"><strong>videoObject.disconnect();
</strong></code></pre>

And add this to the event handler of the`USER_FEEDS` event from `@pushprotocol/socket`:

```typescript
pushSDKSocket?.on(EVENTS.USER_FEEDS, (feedItem: any) => {
    const { payload } = feedItem || {};
    // we check for the additionalMeta property in payload.data
    if (payload.hasOwnProperty('data') && payload['data'].hasOwnProperty('additionalMeta')) {
        const additionalMeta = JSON.parse(payload['data']['additionalMeta']);
        
        if (additionalMeta.status === VideoCallStatus.DISCONNECTED) {
            // here you can do a window reload or just clear out the video state
        }
    }
});
```

{% embed url="https://www.npmjs.com/package/@pushprotocol/restapi#disconnect" %}
disconnect
{% endembed %}
