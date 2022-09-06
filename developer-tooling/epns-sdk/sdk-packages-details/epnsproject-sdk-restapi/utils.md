# Utils

_**NOTE on generating the "signer" object for different platforms:**_

When using in SERVER-SIDE code:

```typescript
const ethers = require('ethers');
const PK = 'your_channel_address_secret_key';
const Pkey = `0x${PK}`;
const signer = new ethers.Wallet(Pkey);
```

When using in FRONT-END code:

```typescript
// any other web3 ui lib is also acceptable
import { useWeb3React } from "@web3-react/core";
.
.
.
const { account, library, chainId } = useWeb3React();
const signer = library.getSigner(account);
```

### **opt in to a channel Comment**

```typescript
await EpnsAPI.channels.subscribe({
  signer: _signer,
  channelAddress: 'eip155:42:0xD8634C39BBFd4033c0d3289C4515275102423681', // channel address in CAIP
  userAddress: 'eip155:42:0x52f856A160733A860ae7DC98DC71061bE33A28b3', // user address in CAIP
  onSuccess: () => {
   console.log('opt in success');
  },
  onError: () => {
    console.error('opt in error');
  },
  env: 'staging'
})
```

Allowed Options (params with \* are mandatory)

| Param                    | Type     | Default | Remarks                            |
| ------------------------ | -------- | ------- | ---------------------------------- |
| signer\*                 | -        | -       | Signer object                      |
| channelAddress\*         | string   | -       | channel address (CAIP)             |
| userAddress\*            | string   | -       | user address (CAIP)                |
| verifyingContractAddress | string   | -       | EPNS communicator contract address |
| onSuccess                | function | -       | on success callback                |
| onError                  | function | -       | on error callback                  |
| env                      | string   | ‘prod’  | API env - ‘prod’, ‘staging’, ‘dev’ |

### **opt out to a channel**

```typescript
await EpnsAPI.channels.unsubscribe({
  signer: _signer,
  channelAddress: 'eip155:42:0xD8634C39BBFd4033c0d3289C4515275102423681', // channel address in CAIP
  userAddress: 'eip155:42:0x52f856A160733A860ae7DC98DC71061bE33A28b3', // user address in CAIP
  onSuccess: () => {
   console.log('opt out success');
  },
  onError: () => {
    console.error('opt out error');
  },
  env: 'staging'
})
```

Allowed Options (params with \* are mandatory)

| Param                    | Type     | Default | Remarks                            |
| ------------------------ | -------- | ------- | ---------------------------------- |
| signer\*                 | -        | -       | Signer object                      |
| channelAddress\*         | string   | -       | channel address (CAIP)             |
| userAddress\*            | string   | -       | user address (CAIP)                |
| verifyingContractAddress | string   | -       | EPNS communicator contract address |
| onSuccess                | function | -       | on success callback                |
| onError                  | function | -       | on error callback                  |
| env                      | string   | ‘prod’  | API env - ‘prod’, ‘staging’, ‘dev  |

**EPNS communicator contract address**

```
ETH Mainnet - 0xb3971BCef2D791bc4027BbfedFb47319A4AAaaAa
ETH Kovan - 0x87da9Af1899ad477C67FeA31ce89c1d2435c77DC
```
