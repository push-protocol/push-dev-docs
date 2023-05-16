---
description: Before starting a video call, you need to initialize the Video object.
---

# Initializing Video Object

Pre-requisite: Deriving the signer

Some functions require passing the signer object with the function call. Fetching signer for web3 wallets is quite easy.

{% tabs %}
{% tab title="When using Frontend" %}
```typescript
// any other web3 ui lib is also acceptable
import { useWeb3React } from "@web3-react/core";
.
.
.
const { account, library, chainId } = useWeb3React();
const signer = library.getSigner(account);
```
{% endtab %}

{% tab title="When using Backend" %}
```typescript
const ethers = require('ethers');
const PK = 'your_channel_address_secret_key';
const Pkey = `0x${PK}`;
const signer = new ethers.Wallet(Pkey);
```
{% endtab %}
{% endtabs %}

## Create Video Object

To start video calling wallet addresses, first you need to declare data, setData which are essentially a state/variable to hold video call related data and a function to modify it respectively. Next you need to create a Video object.

<pre class="language-typescript"><code class="lang-typescript"><strong>import { initVideoCallData } from '@pushprotocol/restapi/src/lib/video';
</strong><strong>
</strong><strong>// data, setData for a vanilla JS project
</strong><strong>let data: PushAPI.VideoCallData = initVideoCallData;
</strong><strong>const setData: : (fn: (data: VideoCallData) => VideoCallData) => void = (fn) => {
</strong><strong>  data = fn(data);
</strong><strong>};
</strong><strong>
</strong><strong>// data, setData for a React project
</strong><strong>import {useState} from "react";
</strong><strong>const [data, setData] = useState&#x3C;PushAPI.VideoCallData>(initVideoCallData);
</strong><strong>
</strong><strong>const videoObject = new PushAPI.video.Video({
</strong><strong>  signer: SignerType;
</strong>  chainId: number;
  pgpPrivateKey: string;
  env?: ENV;
  setData: (fn: (data: VideoCallData) => VideoCallData) => void;
});
</code></pre>

| Property      | Description                                                                                                                                                                 |
| ------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| signer        | ethers.js signer                                                                                                                                                            |
| chainId       | chainId for the video call - Ethereum Mainnet: 1, Polygon Mainnet: 137                                                                                                      |
| pgpPrivateKey | decrypted pgp private key                                                                                                                                                   |
| env           | Environment variable. Default `prod`. For testing, use `staging`                                                                                                            |
| setData       | Function to update the video call data, takes a function as an argument which receives the latest state of data as a param and should return the modified/new state of data |

{% embed url="https://www.npmjs.com/package/@pushprotocol/restapi#constructor" %}
constructor
{% endembed %}
