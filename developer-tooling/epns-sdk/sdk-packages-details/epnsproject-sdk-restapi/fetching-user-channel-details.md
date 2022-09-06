# Fetching User / Channel Details

### a. **fetching user notifications**&#x20;

This method allows us to fetch all the notifications that landed in the inbox of a user.

```typescript
const notifications = await EpnsAPI.user.getFeeds({
  user: 'eip155:42:0xD8634C39BBFd4033c0d3289C4515275102423681', // user address in CAIP
  env: 'staging'
});
```

### **b. fetching user spam notifications**

Allows us to fetch all the spam notifications for a given user's wallet address.

```typescript
const spams = await EpnsAPI.user.getFeeds({
  user: 'eip155:42:0xD8634C39BBFd4033c0d3289C4515275102423681', // user address in CAIP
  spam: true,
  env: 'staging'
});
```

Allowed Options (params with \* are mandatory)

| Param  | Type    | Default | Remarks                                                       |
| ------ | ------- | ------- | ------------------------------------------------------------- |
| user\* | string  | -       | user account address (CAIP)                                   |
| page   | number  | 1       | page index of the results                                     |
| limit  | number  | 10      | number of items in 1 page                                     |
| spam   | boolean | false   | if “true” it will fetch spam feeds                            |
| env    | string  | ‘prod’  | API env - ‘prod’, ‘staging’, ‘dev’                            |
| raw    | boolean | false   | if “true” the method will return unformatted raw API response |

### **c. fetching user subscriptions**

&#x20;This method shall provide us with the list of addresses of channels subscribed by a user address

```typescript
const subscriptions = await EpnsAPI.user.getSubscriptions({
  user: 'eip155:42:0xD8634C39BBFd4033c0d3289C4515275102423681', // user address in CAIP
  env: 'staging'
});
```

where `subscriptions` is a list of channels `[{ channel: '0xaddress', ... }]` subscribed by the user.

_Note: We can find out if a user is subscribed to a channel by checking if the channel address is present in the subscriptions list_

Allowed Options (params with \* are mandatory)

| Param  | Type   | Default | Remarks                            |
| ------ | ------ | ------- | ---------------------------------- |
| user\* | string | -       | user address (CAIP)                |
| env    | string | ‘prod’  | API env - ‘prod’, ‘staging’, ‘dev’ |

### **d. fetching channel details**

this method will get channel data for any valid channel address

```typescript
const channelData = await EpnsAPI.channels.getChannel({
  channel: 'eip155:42:0xD8634C39BBFd4033c0d3289C4515275102423681', // channel address in CAIP
  env: 'staging'
});
```

Allowed Options (params with \* are mandatory)

| Param     | Type   | Default | Remarks                            |
| --------- | ------ | ------- | ---------------------------------- |
| channel\* | string | -       | channel address (CAIP)             |
| env       | string | ‘prod’  | API env - ‘prod’, ‘staging’, ‘dev’ |

### **e. searching for channel(s)**

&#x20;This method fetches the list of channels’ data which match the query in the search

```typescript
const channelsData = await EpnsAPI.channels.search({
  query: 'epns', // a search query
  page: 1, // page index
  limit: 20, // no of items per page
  env: 'staging'
});
```

Allowed Options (params with \* are mandatory)

| Param   | Type   | Default | Remarks                            |
| ------- | ------ | ------- | ---------------------------------- |
| query\* | string | -       | search query                       |
| page    | number | 1       | page index of the results          |
| limit   | number | 10      | number of items in 1 page          |
| env     | string | ‘prod’  | API env - ‘prod’, ‘staging’, ‘dev’ |
