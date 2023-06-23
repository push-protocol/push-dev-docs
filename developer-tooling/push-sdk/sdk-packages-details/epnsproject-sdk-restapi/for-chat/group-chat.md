# Group Chat

## **About Group Chat**

Group chat is the latest exciting addition to Push suite, for the first time ever, web3 users are finally able to talk to each other in groups and have fine-grained controls over them. Group chat is a part of Push Chat protocol with the vision of enabling protocols to finally build:

* Their own Discord
* DAO communication
* NFT negotiation
* Private or Public groups
* Token-gated, NFT-gated or Transaction-gated group
* Message gating (coming soon)
* Incentivized Chat (coming soon)
* Payments, Polls and DAO Voting

{% hint style="success" %}
Each group has a chat id associated with them. The chat id is used to do modifications to a group or send messages or approve a group chat request.
{% endhint %}

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

## **To create a group**

Enables creation of the group, group (and even DMs) have chat ids. For group controls, chat id of the group is required. There are a couple of ways by which they can be obtained (via group name or chat id).

```javascript
// pre-requisite API calls that should be made before
// need to get user and through that encryptedPvtKey of the user
const user = await PushAPI.user.get({
    account: 'eip155:0xFe6C8E9e25f7bcF374412c5C81B2578aC473C0F7'
  });
  
// need to decrypt the encryptedPvtKey to pass in the api using helper function
const pgpDecryptedPvtKey = await PushAPI.chat.decryptPGPKey({
      encryptedPGPPrivateKey: user.encryptedPrivateKey, signer: _signer
    });

// actual api
const response = await PushAPI.chat.createGroup({
  groupName:'Push Group Chat 3',
  groupDescription: 'This is the oficial group for Push Protocol',
  members: ['0x9e60c47edF21fa5e5Af33347680B3971F2FfD464','0x3829E53A15856d1846e1b52d3Bdf5839705c29e5'],
  groupImage: &lt;group image link&gt; ,
  admins: ['0x3829E53A15856d1846e1b52d3Bdf5839705c29e5'],
  isPublic: true,
  account: '0xD993eb61B8843439A23741C0A3b5138763aE11a4',
  pgpPrivateKey: pgpDecryptedPvtKey, //decrypted private key
});
```

## **To create a token gated group**

Enables creation of the group, group (and even DMs) have chat ids. For group controls, chat id of the group is required. We can specify ERC20 Contract and the minimum number of tokens join the group. It as supports NFT token gated groups.

```javascript
// pre-requisite API calls that should be made before
// need to get user and through that encryptedPvtKey of the user
const user = await PushAPI.user.get({
    account: 'eip155:0xFe6C8E9e25f7bcF374412c5C81B2578aC473C0F7'
  });
  
// need to decrypt the encryptedPvtKey to pass in the api using helper function
const pgpDecryptedPvtKey = await PushAPI.chat.decryptPGPKey({
      encryptedPGPPrivateKey: user.encryptedPrivateKey, signer: _signer
    });

// actual api
const response = await PushAPI.chat.createGroup({
  groupName:'Push Group Chat 3',
  groupDescription: 'This is the oficial group for Push Protocol',
  members: ['0x9e60c47edF21fa5e5Af33347680B3971F2FfD464','0x3829E53A15856d1846e1b52d3Bdf5839705c29e5'],
  groupImage: &lt;group image link&gt; ,
  admins: ['0x3829E53A15856d1846e1b52d3Bdf5839705c29e5'],
  isPublic: true,
  contractAddressERC20: "0x8Afa8FDf9fB545C8412499E8532C958086608b30",
  numberOfERC20: 20,
  contractAddressNFT: "0x42af3147f17239341477113484752D5D3dda997B",
  numberOfNFTTokens: 2,
  account: '0xD993eb61B8843439A23741C0A3b5138763aE11a4',
  pgpPrivateKey: pgpDecryptedPvtKey, //decrypted private key
});
```

####

#### Allowed Options (params with \* are mandatory)

<table><thead><tr><th width="217">Param</th><th width="111">Type</th><th width="93">Default</th><th>Remarks</th></tr></thead><tbody><tr><td>account*</td><td>string</td><td>-</td><td>user address</td></tr><tr><td>groupName*</td><td>string</td><td>-</td><td>group name</td></tr><tr><td>groupDescription*</td><td>string</td><td>-</td><td>group description</td></tr><tr><td>groupImage*</td><td>string</td><td>-</td><td>group image link</td></tr><tr><td>members*</td><td>Array</td><td>-</td><td>wallet addresses of all members except admins and groupCreator</td></tr><tr><td>admins*</td><td>Array</td><td>-</td><td>wallet addresses of all admins except members and groupCreator</td></tr><tr><td>isPublic*</td><td>boolean</td><td>-</td><td>true for public group, false for private group</td></tr><tr><td>contractAddressERC20</td><td>string</td><td>null</td><td>ERC20 Contract Address</td></tr><tr><td>numberOfERC20</td><td>int</td><td>0</td><td>Minimum number of tokens required to join the group</td></tr><tr><td>contractAddressNFT</td><td>string</td><td>null</td><td>NFT Contract Address</td></tr><tr><td>numberOfNFTTokens</td><td>int</td><td>0</td><td>Minimum number of NFTs required to join the group</td></tr><tr><td>pgpPrivateKey</td><td>string</td><td>null</td><td>mandatory for users having pgp keys</td></tr><tr><td>env</td><td>string</td><td>'prod'</td><td>API env - 'prod' or 'staging'</td></tr></tbody></table>

{% embed url="https://www.npmjs.com/package/@pushprotocol/restapi#to-create-a-group" %}
To create a group
{% endembed %}

## **To update group details**

Enables updating group details (is an idempotent operation). Idempotent means that most of the details even if unmodified are required to be passed as a way to protect the group's latest settings and have verification proof available for the modification of the group.

```javascript
// pre-requisite API calls that should be made before
// need to get user and through that encryptedPvtKey of the user
const user = await PushAPI.user.get({
    account: 'eip155:0xFe6C8E9e25f7bcF374412c5C81B2578aC473C0F7'
});
  
// need to decrypt the encryptedPvtKey to pass in the api using helper function
const pgpDecryptedPvtKey = await PushAPI.chat.decryptPGPKey({
        encryptedPGPPrivateKey: user.encryptedPrivateKey, 
        signer: _signer
});

// actual api
const response = await PushAPI.chat.updateGroup({
    chatId: '870cbb20f0b116d5e461a154dc723dc1485976e97f61a673259698aa7f48371c',
    groupName: 'Push Group Chat 3',
    groupDescription: 'This is the oficial group for Push Protocol',
    members: ['0x2e60c47edF21fa5e5A333347680B3971F1FfD456','0x3829E53A15856d1846e1b52d3Bdf5839705c29e5'],
    groupImage: &lt;group image link&gt; ,
    admins: ['0x3829E53A15856d1846e1b52d3Bdf5839705c29e5'],
    account: '0xD993eb61B8843439A23741C0A3b5138763aE11a4',
    pgpPrivateKey: pgpDecryptedPvtKey, //decrypted private key
});
```

## **To update a token group details**

Same as above call

```javascript
// pre-requisite API calls that should be made before
// need to get user and through that encryptedPvtKey of the user
const user = await PushAPI.user.get({
    account: 'eip155:0xFe6C8E9e25f7bcF374412c5C81B2578aC473C0F7'
});
  
// need to decrypt the encryptedPvtKey to pass in the api using helper function
const pgpDecryptedPvtKey = await PushAPI.chat.decryptPGPKey({
        encryptedPGPPrivateKey: user.encryptedPrivateKey, 
        signer: _signer
});

// actual api
const response = await PushAPI.chat.updateGroup({
    chatId: '870cbb20f0b116d5e461a154dc723dc1485976e97f61a673259698aa7f48371c',
    groupName: 'Push Group Chat 3',
    groupDescription: 'This is the oficial group for Push Protocol',
    members: ['0x2e60c47edF21fa5e5A333347680B3971F1FfD456','0x3829E53A15856d1846e1b52d3Bdf5839705c29e5'],
    groupImage: &lt;group image link&gt; ,
    admins: ['0x3829E53A15856d1846e1b52d3Bdf5839705c29e5'],
    contractAddressERC20: "0x8Afa8FDf9fB545C8412499E8532C958086608b30",
    numberOfERC20: 20,
    contractAddressNFT: "0x42af3147f17239341477113484752D5D3dda997B",
    numberOfNFTTokens: 2,
    account: '0xD993eb61B8843439A23741C0A3b5138763aE11a4',
    pgpPrivateKey: pgpDecryptedPvtKey, //decrypted private key
});
```

####

#### Allowed Options (params with \* are mandatory)

<table><thead><tr><th width="233">Param</th><th width="106">Type</th><th width="103">Default</th><th>Remarks</th></tr></thead><tbody><tr><td>chatId*</td><td>string</td><td>-</td><td>chatId of the group</td></tr><tr><td>account*</td><td>string</td><td>-</td><td>user address</td></tr><tr><td>groupName*</td><td>string</td><td>-</td><td>group name</td></tr><tr><td>groupDescription*</td><td>string</td><td>-</td><td>group description</td></tr><tr><td>groupImage*</td><td>string</td><td>-</td><td>group image link</td></tr><tr><td>members*</td><td>Array</td><td>-</td><td>wallet addresses of all members except admins and groupCreator</td></tr><tr><td>admins*</td><td>Array</td><td>-</td><td>wallet addresses of all admins except members and groupCreator</td></tr><tr><td>contractAddressERC20</td><td>string</td><td>null</td><td>ERC20 Contract Address</td></tr><tr><td>numberOfERC20</td><td>int</td><td>0</td><td>Minimum number of tokens required to join the group</td></tr><tr><td>contractAddressNFT</td><td>string</td><td>null</td><td>NFT Contract Address</td></tr><tr><td>numberOfNFTTokens</td><td>int</td><td>0</td><td>Minimum number of nfts required to join the group</td></tr><tr><td>pgpPrivateKey</td><td>string</td><td>null</td><td>mandatory for users having pgp keys</td></tr><tr><td>env</td><td>string</td><td>'prod'</td><td>API env - 'prod' or 'staging'</td></tr></tbody></table>

{% embed url="https://www.npmjs.com/package/@pushprotocol/restapi#to-update-group-details" %}
To update the group
{% endembed %}

## **To get group details by group name**

Enables fetching group details by group name.

```javascript
const response = await PushAPI.chat.getGroupByName({
  groupName: "Push Group Chat 3"
});
```

#### Allowed Options (params with \* are mandatory)

<table><thead><tr><th width="162">Param</th><th width="104">Type</th><th width="121">Default</th><th>Remarks</th></tr></thead><tbody><tr><td>groupName*</td><td>string</td><td>-</td><td>name of the group</td></tr><tr><td>env</td><td>string</td><td>'prod'</td><td>API env - 'prod' or 'staging'</td></tr></tbody></table>

{% embed url="https://www.npmjs.com/package/@pushprotocol/restapi#to-get-group-details-by-group-name" %}
Get group details by group name
{% endembed %}

## **To get group details by chatId**

Enables fetching group details by chat id.

```javascript
const response = await PushAPI.chat.getGroup({
  chatId: '190591e84108cdf12e62eecabf02ddb123ea92f1c06fb98ee9b5cf3871f46fa9'
});
```

#### Allowed Options (params with \* are mandatory)

<table><thead><tr><th width="149">Param</th><th width="113">Type</th><th width="110">Default</th><th>Remarks</th></tr></thead><tbody><tr><td>chatId*</td><td>string</td><td>-</td><td>group chat id</td></tr><tr><td>env</td><td>string</td><td>'prod'</td><td>API env - 'prod' or 'staging'</td></tr></tbody></table>

{% embed url="https://www.npmjs.com/package/@pushprotocol/restapi#to-get-group-details-by-chatid" %}
Get group details by chat id
{% endembed %}
