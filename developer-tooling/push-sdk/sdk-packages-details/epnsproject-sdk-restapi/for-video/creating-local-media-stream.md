---
description: Next step is to create a local media stream
---

# Creating Local Media Stream

## Create Local Media Stream

Before initiating the call, we need to create a local media stream i.e. local audio and video which will be used during the call initiation.

<pre class="language-typescript"><code class="lang-typescript"><strong>await videoObject.create({
</strong><strong>    video?: boolean;
</strong><strong>    audio?: boolean;
</strong><strong>});
</strong></code></pre>

| Property | Description                                                |
| -------- | ---------------------------------------------------------- |
| video    | Whether the video should be enabled or not during the call |
| audio    | Whether the audio should be enabled or not during the call |

## ⚠️ Warning

If audio, video aren't enabled in `create()` then they wont be available during the video call.

{% embed url="https://www.npmjs.com/package/@pushprotocol/restapi#create" %}
