---
description: A React component for integrating Support Chat in DApps.
---

# Push Support Chat

## Installation

{% tabs %}
{% tab title="npm" %}
```bash
npm install @pushprotocol/uiweb
```
{% endtab %}

{% tab title="yarn" %}
```bash
yarn add @pushprotocol/uiweb
```
{% endtab %}
{% endtabs %}

> _**Note:**_ `styled-components` is a `peerDependency`. Please install it in your dApp if you don't have it already!

We'll need the `@pushprotocol/restapi` package as well.

{% tabs %}
{% tab title="npm" %}
```bash
npm install styled-components

npm install @pushprotocol/restapi
```
{% endtab %}

{% tab title="yarn" %}
```bash
yarn add styled-components

yarn add @pushprotocol/restapi
```
{% endtab %}
{% endtabs %}

## Pre-requisite: Deriving the signer

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
{% endtabs %}

## Support Chat component Usage

Import the SDK package in the component file where you want to render the support chat component.

```typescript
import { Chat } from "@pushprotocol/uiweb";
import { ITheme } from '@pushprotocol/uiweb';
```

Render the Chat Component as follows

```typescript
<Chat
   account="0x6430C47973FA053fc8F055e7935EC6C2271D5174" //user address
   supportAddress="0xd9c1CCAcD4B8a745e191b62BA3fcaD87229CB26d" //support address
   signer={signer}
   env="staging"
 />
```

<table><thead><tr><th width="181">Prop</th><th width="104">Type</th><th width="186">Default</th><th>Remarks</th></tr></thead><tbody><tr><td>account*</td><td>string</td><td>-</td><td>user address(sender)</td></tr><tr><td>supportAddress*</td><td>string</td><td>-</td><td>support user's address(receiver)</td></tr><tr><td>signer*</td><td>ethers.js signer</td><td>-</td><td>signer ( used for decrypting chats )</td></tr><tr><td>greetingMsg</td><td>string</td><td><code>'Hi there!'</code></td><td>first message in chat screen</td></tr><tr><td>theme</td><td>ITheme</td><td><code>lightTheme</code></td><td>theme for chat modal (only lightTheme available now)</td></tr><tr><td>modalTitle</td><td>string</td><td><code>'Chat with us!'</code></td><td>Modal header title</td></tr><tr><td>apiKey</td><td>string</td><td><code>''</code></td><td>api key</td></tr><tr><td>env</td><td>string</td><td><code>'prod'</code></td><td>API env: <code>'prod'</code>, <code>'staging'</code>, <code>'dev'</code></td></tr></tbody></table>

Example code for using custom theme

```typescript
import React from 'react';
import { Chat, ITheme } from '@pushprotocol/uiweb';

export const ChatSupportTest = () => {
  const theme: ITheme = {
    bgColorPrimary: 'gray',
    bgColorSecondary: 'purple',
    textColorPrimary: 'white',
    textColorSecondary: 'green',
    btnColorPrimary: 'red',
    btnColorSecondary: 'purple',
    border: '1px solid black',
    borderRadius: '40px',
    moduleColor: 'pink',
  };
  return (
    <Chat
      account='0xFe6C8E9e25f7bcF374412c5C81B2578aC473C0F7'
      supportAddress="0xFe6C8E9e25f7bcF374412c5C81B2578aC473C0F7"
      env='staging'
      signer={signer}
      theme={theme}
    />
  );
};
```

For customizing the Chat theme, here below are the list of variables:

<figure><img src="../../../../.gitbook/assets/Push Sdk Diagram (1).png" alt=""><figcaption><p>List of all themes variable</p></figcaption></figure>

<figure><img src="../../../../.gitbook/assets/pushLogo.png" alt=""><figcaption><p>How Chat Component will look in your dapp</p></figcaption></figure>

<figure><img src="../../../../.gitbook/assets/chat1.png" alt=""><figcaption><p>How Chat Component will look in your dapp</p></figcaption></figure>

<figure><img src="../../../../.gitbook/assets/chat2.png" alt=""><figcaption><p>How Chat Component will look in your dapp</p></figcaption></figure>

## Troubleshoot

During the procedure, you might encounter an error, as can be seen in the image below.

<figure><img src="../../../../.gitbook/assets/err-1.png" alt=""><figcaption></figcaption></figure>

If you run into such an error, try to include the code below in config-overrides.js in the root folder.

```typescript
const webpack = require('webpack');

module.exports = function override(config, env) {
  // do stuff with the webpack config...
  config.resolve.fallback = {
    assert: require.resolve('assert'),
    buffer: require.resolve('buffer'),
    child_process: false,
    constants: require.resolve('constants-browserify'),
    crypto: require.resolve('crypto-browserify'),
    fs: false,
    http: require.resolve('stream-http'),
    https: require.resolve('https-browserify'),
    os: require.resolve('os-browserify/browser'),
    path: require.resolve('path-browserify'),
    url: require.resolve('url'),
    util: require.resolve('util/'),
    stream: require.resolve('stream-browserify')
  }
  config.resolve.extensions = [...config.resolve.extensions, '.ts', '.js']
  config.plugins = [
    ...config.plugins,
    new webpack.ProvidePlugin({
      process: 'process/browser',
      Buffer: ['buffer', 'Buffer']
    })
  ]
  config.module.rules = [...config.module.rules,
    {
      test: /\.m?js/,
      resolve: {
        fullySpecified: false
      }
    }
  ]

  return config
}
```
