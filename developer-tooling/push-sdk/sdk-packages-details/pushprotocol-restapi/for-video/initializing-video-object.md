---
description: Before starting a video call, you need to initialize the Video object
---

# Initializing Video Object

Some functions require passing the signer object with the function call. Fetching a signer for web3 wallets is quite easy.

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

To start video calling wallet addresses, first, you need to declare `data`, `setData` which are essentially a state/variable to hold video call-related data and a function to modify it respectively.&#x20;

Here below is how to create the video object:

```typescript
const videoObject = new PushAPI.video.Video({
  signer: SignerType;
  chainId: number;
  pgpPrivateKey: string;
  env?: ENV;
  setData: (fn: (data: VideoCallData) => VideoCallData) => void;
});
```

<table><thead><tr><th width="179">Property</th><th>Description</th></tr></thead><tbody><tr><td>signer</td><td>Ethers.js signer</td></tr><tr><td>chainId</td><td>ChainId for the video call - Ethereum Mainnet: 1, Polygon Mainnet: 137</td></tr><tr><td>pgpPrivateKey</td><td>User's decrypted pgp private key</td></tr><tr><td>env</td><td>Environment variable. Default <code>prod</code>. For testing, use <code>staging</code></td></tr><tr><td>setData</td><td>Function to update the video call data, takes a function as an argument which receives the latest state of data as a param and should return the modified/new state of data</td></tr></tbody></table>

Now let's go over each property individually and understand how to set them.

### Data

* The `data` variable is an object that is used to store all of the video call-related data.
* It is of type `PushAPI.VideoCallData` and should be initialized with `initVideoCallData`.
* For React-based projects, this would basically be a React state, and for vanilla JS projects, it will be a simple JS variable.

The type `PushAPI.VideoCallData` is described below:

```typescript
// IMediaStream is a custom type that represents a media stream object or null
type IMediaStream = MediaStream | null;

// VideoCallStatus is an enum that represents different statuses of a video call
// These values are used to indicate the current status of a video call
enum VideoCallStatus {
  // call hasn't been started yet
  UNINITIALIZED,
  
  // call has been started by the initiator but not received by the receiver
  INITIALIZED,
  
  // call has been received by the receiver but not connected yet
  RECEIVED,
  
  // call has been connected, and both users can interact now
  // MediaStream (includes both audio and video) exchange is now possible 
  CONNECTED,
  
  // call has been ended by one of the users
  DISCONNECTED,
  
  // retrying to establish the call from the INITIALIZED state
  RETRY_INITIALIZED,
  
  // retrying to establish the call from the RECEIVED state
  RETRY_RECEIVED,
}

type VideoCallData = {
  meta: { // contains meta info of the video call
    chatId: string; // unique identifier for every push chat
    initiator: {
      address: string; // address of the initiator of the call
      signal: any; // signal data from the initiator
    };
    broadcast?: { // will be used in Push Spaces
      livepeerInfo: any;
      hostAddress: string;
      coHostAddress: string;
    };
  };
  // This property is used to store the info of the local peer/user
  local: {
    stream: IMediaStream; // the stream object
    audio: boolean | null; // whether local audio is on or not
    video: boolean | null; // whether local video is on or not
    address: string; // address of the local peer/user
  };
  incoming: [
  // This property is used to store the info of an incoming peer/user
  {
    stream: IMediaStream; // the stream object
    audio: boolean | null; // whether incoming audio is on or not
    video: boolean | null; // whether incoming video is on or not
    address: string; // address of the incoming peer/user
    status: VideoCallStatus; // video call status with a particular incoming peer
    retryCount: number; // no. of retries required in establishing the call 
  }];
};
```

The `initVideoCallData` looks like the following:

```typescript
// As mentioned earlier, this should be used to initialize the data variable.
const initVideoCallData: PushAPI.VideoCallData = {
  meta: {
    chatId: '', 
    initiator: {
      address: '',
      signal: null,
    },
  },
  local: {
    stream: null,
    audio: null,
    video: null,
    address: '',
  },
  incoming: [
    {
      stream: null,
      audio: null,
      video: null,
      address: '',
      status: VideoCallStatus.UNINITIALIZED, // call is at the UNINITIALIZED status
      retryCount: 0,
    },
  ],
};
```

Creating and initializing the `data` variable is shown below:

```typescript
// NOTE: You don't have to define initVideoCallData in your project.
// Just import it using the following and use it:
import { initVideoCallData } from '@pushprotocol/restapi/src/lib/video';

// 1. For a vanilla JS project
let data: PushAPI.VideoCallData = initVideoCallData;

// 2. For a React project
import { useState } from "react";
const [data, setData] = useState<PushAPI.VideoCallData>(initVideoCallData);
```

### setData

* `setData` is a function to update the video call data i.e., the `data` variable.
* It is of type `(fn: (data: VideoCallData) => VideoCallData) => void` and is described below:

<pre class="language-typescript"><code class="lang-typescript"><strong>/*
</strong><strong> - fn function is supplied by the caller of setData()
</strong><strong> - fn is a function that accepts current 'data' as input and returns updated 'data' 
</strong><strong>*/
</strong><strong>const setData: : (fn: (data: VideoCallData) => VideoCallData) => void = (fn) => {
</strong><strong>  /*
</strong><strong>    - Here, we are passing the current value of 'data' to fn
</strong><strong>    - The return value of fn() i.e., the updated value of 'data' is assigned back to 'data'
</strong><strong>  */
</strong><strong>  data = fn(data);
</strong><strong>};
</strong></code></pre>

### Initializing video object

* Now we are ready to instantiate a new object of the Video class (can be accessed via PushAPI.video.Video) using the `signer`, `chainId`, `pgpPrivatekey`, `env` and the `setData` function.

```typescript
const videoObject = new PushAPI.video.Video({
  signer: SignerType;
  chainId: number;
  pgpPrivateKey: string;
  env?: ENV;
  setData: (fn: (data: VideoCallData) => VideoCallData) => void;
});
```

{% embed url="https://www.npmjs.com/package/@pushprotocol/restapi#constructor" %}
constructor
{% endembed %}
