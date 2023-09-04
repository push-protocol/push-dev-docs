---
description: Turn-on/off your audio and video
---

# Enable/Disable Local Audio/Video

## Enable/Disable Local Video

To enable/disable local video we'll call the `enableVideo()` method on the `videoObject`.

```typescript
videoObject.enableVideo({
    state:boolean;
});
```

<table><thead><tr><th width="203">Property</th><th>Description</th></tr></thead><tbody><tr><td>state</td><td><code>true</code> for enable and <code>false</code> for disable</td></tr></tbody></table>

## Enable/Disable Local Video Audio

To enable/disable local audio we'll call the `enableAudio()` method on the `videoObject`.

```typescript
videoObject.enableAudio({
    state:boolean;
});
```

<table><thead><tr><th width="203">Property</th><th>Description</th></tr></thead><tbody><tr><td>state</td><td><code>true</code> for enable and <code>false</code> for disable</td></tr></tbody></table>

{% hint style="info" %}
## How to know the current status of local audio/video?

\
The current local video status can be accessed by `videoObject.data.local.video` similarly for audio -> `videoObject.data.local.audio`.&#x20;

If `videoObject.data.local.video` is true, then that means your local video is enabled, and if it's false then that means the local video is disabled. Likewise for `videoObject.data.local.audio`.
{% endhint %}

{% embed url="https://www.npmjs.com/package/@pushprotocol/restapi#enablevideo" %}
enable video
{% endembed %}

{% embed url="https://www.npmjs.com/package/@pushprotocol/restapi#enableaudio" %}
enable audio
{% endembed %}
