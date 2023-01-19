# Receiving via Delivery Node

Push delivery nodes are a decentralized solution to enable web3 to web2 bridging. They allow any platform (whether centralized or decentralized) to receive communication from **Push storage nodes**, i.e., the nodes validating and indexing all communications and tying them to your wallet address (and multi-chain identity). The Whitelabel solution of Push delivery nodes can be found in the GitHub below. :point\_down:

{% embed url="https://github.com/ethereum-push-notification-service/push-delivery-node" %}

{% hint style="warning" %}
Push Delivery Nodes is a Whitelabel solution to enable any app, whether mobile, desktop or extension, to allow Web3 push notifications to their app.&#x20;
{% endhint %}

## Steps for Getting Started With the Delivery Node Module <a href="#4da3" id="4da3"></a>

### Prerequisites



* MYSQL (Version >= 5.7)
* Redis (Version >= 6.0)
* Docker (For local setup)
* Google FCM Account Setup

### Environment Configuration

\
Refer env sample file; the MYSQL DB credentials and Redis URL need to be updated. The remaining conf need not be edited as of now.

```
# DELIVERY NODE DATABASE
DELIVERY_NODE_DB_HOST=localhost
DELIVERY_NODE_DB_NAME=dbname
DELIVERY_NODE_DB_USER=user
DELIVERY_NODE_DB_PASS=pass
DELIVERY_NODE_DB_PORT=3306

# FIREBASE
FIREBASE_DATABASE_URL=<url>

```

### Local Setup

To start the Delivery node with MYSQL and Redis :

```
docker compose up

```

You should then be able to build the server using the following command :

```
npm install
```

You should then be able to start the server using the following command :

```
npm start
```

### Production Setup

* Host MYSQL and Redis Separately
* Delivery node installation
* Add MYSQL and Redis credentials in the .env file

_You should then be able to build the server using the following:_

```
npm install
```

You should then be able to start the server using the following command:

```
npm start

```

### FCM Setup

* Refer [https://firebase.google.com/docs/admin/setup](https://firebase.google.com/docs/admin/setup)
* Create firebase-adminsdk.json file in the root folder and add the FCM JSON in the file
* Add FIREBASE\_DATABASE\_URL in the .env file

### Device Registration

Below is the API to create a mapping between the wallet address and the device token for which the messages need to be delivered.

```
curl --location --request POST 'https://<delivery_node_url>/apis/v1/pushtokens/register' \
--header 'Content-Type: application/json' \
--data-raw '{
    
    "wallet": "eip155:0x35B84d6848D16415177c64D64504663b998A6ab4",
    "device_token": "device_token",
    "platform": "android"
}'

```
