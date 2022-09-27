# Core & Communicator Smart Contracts

In order for the protocol to be chain-agnostic as well as truly decentralized, PUSH smart contracts play a major role.

The PUSH smart contracts have now been divided into two different smart contracts, i.e., **PUSH **_**Core &**_** PUSH **_**Communicator.**_\


### **1.** PUSH **Core Protocol: (EPNSCore)**

The PUSH core protocol, as the name might indicate, is the main smart contract as it handles some of the most crucial features like _**Channel creation, governance, channel state changes as well as funds and incentive mechanisms**_**, etc.**&#x20;

\
It’s very important to note, however, that the PUSH **Core smart contract will only be deployed on the Ethereum blockchain and not on any other chain.**

### **2.** PUSH **Communicator Protocol: (EPNSCommunicator)**

Unlike the PUSH Core smart contract, the communicator protocol is designed to be deployed on multiple chains. This is also one of the imperative reasons behind the communicator contract being lightweight and less reliant on PUSH Core.

The PUSH Communicator protocol is comparatively quite simple. The communicator protocol includes features that allow users to _subscribe to a channel, unsubscribe from a channel as well as the imperative one, i.e., **sending notifications.**_

&#x20;_****_ As the communicator protocol can be deployed on various chains, it allows us to trigger notifications on multiple chains and not just the Ethereum blockchain.

### Technical Resources for PUSH Smart Contracts

{% content-ref url="epns-core-contract/" %}
[epns-core-contract](epns-core-contract/)
{% endcontent-ref %}

{% content-ref url="epns-communicator-protocol/" %}
[epns-communicator-protocol](epns-communicator-protocol/)
{% endcontent-ref %}

{% content-ref url="epns-contract-addresses.md" %}
[epns-contract-addresses.md](epns-contract-addresses.md)
{% endcontent-ref %}
