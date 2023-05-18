---
description: Turn-on/off your audio and video.
---

# Toggling Local Audio/Video

## Toggle Local Video

To toggle your video we'll call the `toggleVideo()` method on the videoObject.

```typescript
videoObject.toggleVideo();
```

## Toggle Local Audio

To toggle your video we'll call the `toggleAudio()` method on the videoObject.

```typescript
videoObject.toggleAudio();
```

## ℹ️ How to know the current status of local audio/video?

The current local video status can be accessed by `videoObject.data.local.video` similarly for audio -> `videoObject.data.local.audio`.

{% embed url="https://www.npmjs.com/package/@pushprotocol/restapi#togglevideo" %}
toggle video
{% endembed %}

{% embed url="https://www.npmjs.com/package/@pushprotocol/restapi#toggleaudio" %}
toggle audio
{% endembed %}
