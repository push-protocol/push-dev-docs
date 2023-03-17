# Initializing User

## Pre-requisite: Deriving the signer

Some functions require passing the signer object with the API call. fetching signer for web3 wallets is quite easy.

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

## Create User

To start messaging wallet addresses, you need to get encryption keys attached to the user. In case, the user is not created, the first step is to create the user.

```typescript
const user = await PushAPI.user.create({
   account: '0xFe6C8E9e25f7bcF374412c5C81B2578aC473C0F7'
});
```

{% embed url="https://www.npmjs.com/package/@pushprotocol/restapi#create-user-for-chat" %}
Create User Example
{% endembed %}

## Get User

Once the user is created, you can fetch details about the user including their encrypted PGP keys. PGP keys are used to encrypt and decrypt messages and you are required to decrypt them after fetching the encrypted version of it.

```javascript
const user = await PushAPI.user.get({
   account: 'eip155:0xFe6C8E9e25f7bcF374412c5C81B2578aC473C0F7'
});
```

#### Allowed Options (params with \* are mandatory)

| Param     | Type   | Default | Remarks                       |
| --------- | ------ | ------- | ----------------------------- |
| account\* | string | -       | user address (Partial CAIP)   |
| env       | string | 'prod'  | API env - 'prod' or 'staging' |

{% embed url="https://www.npmjs.com/package/@pushprotocol/restapi#get-user-data-for-chat" %}
Get User Example
{% endembed %}

## Decrypt PGP Keys

Push chat is encrypted and only the users are able to decrypt chats, send messages, approve chat, etc. This means that you need to decrypt PGP keys after you have fetched user info for most of the features. Decrypting is easy and requires you to pass the user object and the signer.

```javascript
// pre-requisite API calls that should be made before
const user = await PushAPI.user.get(account: 'eip155:0xFe6C8E9e25f7bcF374412c5C81B2578aC473C0F7', env: 'staging');
  
// actual api
const decryptedPvtKey = await PushAPI.chat.decryptPGPKey(
    encryptedPGPPrivateKey: user.encryptedPrivateKey,
    signer: _signer
);
```

{% embed url="https://www.npmjs.com/package/@pushprotocol/restapi#decrypting-encrypted-pgp-private-key-from-user-data" %}
Decrypt PGP Keys
{% endembed %}

