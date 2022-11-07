# Integrating Push Chat

Integrating Push Chat for any functionality is extremely easy. The Push Chat SDK is divided into the following functionalities:

{% hint style="success" %}
This guide provides high-level knowledge about function calls and what they do, to dive into code :point\_right: [epnsproject-sdk-restapi](../developer-tooling/push-sdk/sdk-packages-details/epnsproject-sdk-restapi/ "mention")
{% endhint %}

{% hint style="info" %}
Push Chat API is in alpha, please bookmark this page for the new APIs introduction. The chat socket API will also be released shortly.
{% endhint %}

{% hint style="success" %}
To enable dApps to adopt Push Chat while it's closed, we have enabled an optional API key that bypasses the whitelisted access for the dApp and their users. The API key will not be required when Push Chat access becomes public.\
\
To get the API key, please join our discord and ask a CM or a Dev for one. :point\_right: [https://discord.com/invite/pushprotocol](https://discord.com/invite/pushprotocol)
{% endhint %}

## User Meta

Each User of Push Chat has a PGP key which is either created locally or stored encrypted on Push nodes.&#x20;

You are required to get the PGP key and decrypt it locally for which you can use the following SDK functions:

#### **a.** To get the User

`sdk.user.get`

* This function will return all the user information, like the PGP keys. It takes as arguments the address of the wallet and the environment variable.

**b. To create the User**

`sdk.user.create`

* This function will create a new user and **** return the created user’s information, like the PGP keys. It takes as arguments the address of the wallet and the environment variable.

## Fetching Chats for a User

All chats for a user or all chats request for a user can be fetched in a paginated fashion using the following SDK functions:

**a. To Fetch  a list of all chats of a User**

`sdk.chat.chats`&#x20;

* This function returns all the latest chats from each address the caller is talking to. It’s used to build the inbox on a chat application for an address

**b. To Fetch a list of all chats request of a User**

`sdk.chat.requests`&#x20;

* This function returns all the requests that wallet addresses sent to a particular address. In Push Chat, the receiver of the messages must always approve the request to start the chat with the other address.

### Fetching individual messages in a specific Chat

Each conversation between the users or group of users have a conversation hash which is a linked list of thread that contains the encrypted chat messages stored on IPFS, the SDK does the work of decryption, etc provided you give the SDK a conversation hash.

**a. Getting conversation hash of a single chat**

`sdk.chat.conversationHash`

* This function returns the conversation hash of the latest message exchanged between the user and the conversation.

**b. Getting the History of a single chat**

`sdk.chat.history`

* This function takes as `an the` argument the conversation hash from a message and the pagination and then returns the message content decrypted.

**c. Getting just the latest message from a single chat**

`sdk.chat.latest`

* This function takes as an argument the conversation hash from a message and then returns the message content decrypted.

## Replying to Chats

theReplying chats require the user to approve the request if it's the first time and then interact normally via the send rest API call.

a. To approve a chat request (_only required for first time_)

`sdk.chat.send`

* When receiving a Request, call this function to approve the request so you can start talking back to the address.

**b. To chat with a user or conversation id**

`sdk.chat.requests`

* Use this function to send messages to other addresses.

{% hint style="success" %}
To learn more about the API params and how to call the Restful API, please check :point\_right: [epnsproject-sdk-restapi](../developer-tooling/push-sdk/sdk-packages-details/epnsproject-sdk-restapi/ "mention")
{% endhint %}
