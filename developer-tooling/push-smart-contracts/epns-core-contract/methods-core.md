---
description: >-
  Brief overview of all the imperative functionalities of the Push Core smart
  contract
---

# Methods - Core



### Only Admin Setter Functions

1. **setEpnsCommunicatorAddress(address)**

```
function setEpnsCommunicatorAddress(address _commAddress) external onlyPushChannelAdmin() {};
```

|      Arguments      |    Type   |              Description             |
| :-----------------: | :-------: | :----------------------------------: |
| _**\_commAddress**_ | _address_ | Address of the communicator protocol |

_**Description:**_

* Allows only the Push Channel Admin to set the Push Communicator smart contract's address

**2.setGovernanceAddress(address)**

```
function setGovernanceAddress(address _governanceAddress) external onlyPushChannelAdmin() {};
```

|          Argument         |    Type   |             Description            |
| :-----------------------: | :-------: | :--------------------------------: |
| _**\_governanceAddress**_ | _address_ | Address of the Governance protocol |

**Description:**

* Allows only the Push Channel Admin to set the Governance address

**3. setChannelDeactivationFees(uint256)**

```
function setChannelDeactivationFees(uint256 _newFees) external onlyGovernance() {};
```

|     Argument    |    Type   |                  Description                  |
| :-------------: | :-------: | :-------------------------------------------: |
| _**\_newFees**_ | _uint256_ | new Channel deactivation fees in the protocol |

**CheckPoints:**

* Can only be called via Governance contract
* The **\_newFees** argument being passed must be greater than Zero.

**Description:**

* Sets the Channel deactivation fees to a new fee amount

**4. setMinChannelCreationFees(uint256)**

```
function setMinChannelCreationFees(uint256 _newFees) external onlyGovernance() {};
```

| Argument        |    Type   |                Description                |
| --------------- | :-------: | :---------------------------------------: |
| _**\_newFees**_ | _uint256_ | new Channel Creation fees in the protocol |

**CheckPoints:**

* Can only be called via Governance contract
* The **\_newFees** argument being passed must be greater than or equal to the already existing Channel creation fee amount.

**Description:**

* Sets the Channel Creation fees to a new fee amount.

**5. transferPushChannelAdminControl(address)**

```
function transferPushChannelAdminControl(address _newAdmin) public onlyPushChannelAdmin() {};
```

|     Argument     |    Type   |        Description       |
| :--------------: | :-------: | :----------------------: |
| _**\_newAdmin**_ | _address_ | address of the new Admin |

**CheckPoints:**

* Can only be called by the current Push Channel Admin.
* _**\_newAdmin**_ address being passed as an argument must be a valid address.
* _**\_newAdmin**_ address being passed as an argument must not be the already existing Push Channel Admin's address

**Description:**

* Changes the Push Channel admin's address to a new Address.

### Core Functionalities&#x20;

**5. createChannelWithFees(ChannelType, bytes, uint256)**

```solidity
  function createChannelWithFees(
        ChannelType _channelType,
        bytes calldata _identity,
        uint256 _amount
    )
        external
        onlyInactiveChannels(msg.sender)
        onlyUserAllowedChannelType(_channelType)
    {};
```

|      Arguments      |    Type   |                  Description                 |
| :-----------------: | :-------: | :------------------------------------------: |
| _**\_channelType**_ |    Enum   | Represents the type of Channel being created |
|   _**\_identity**_  |  _bytes_  |        Identity bytes of the Channel.        |
|    _**\_amount**_   | _uint256_ |       Total amount of Dai being deposit      |

**CheckPoints:**

* Channel must already be in an **INACTIVE STATE**, i.e., _Channel is not already created on the protocol_
* Channel type being passed as an argument must be of a valid type.
* Total amount of DAI being deposited for Channel Creation must be greater than or equal to **50 DAI**

\*_Description:_

* Channel's state is changed from **Inactive to Active state**
* All the imperative information of the channel such as the  _channel's creation block number, total amount of dai deposited, channel's type, etc_ are stored.
* Total **Channel count** of the protocol is incremented by 1.
* Emits out an _**AddChannel()**_ event with the _Channel's address, Channel's Type and its Identity_

**6. deactivateChannel()**

```
   function deactivateChannel(uint256 _amountsOutValue) external onlyActivatedChannels(msg.sender) {};
```

|         Argument        |    Type   |                        Description                       |
| :---------------------: | :-------: | :------------------------------------------------------: |
| _**\_amountsOutValue**_ | _uint256_ | Value of the expected output amount of tokens after Swap |

**CheckPoints:**

* Channel must already be in an **ACTIVE STATE**.

**Description:**

* Channel's state is changed from **Active to DeActivated state**
* **Channel Deactivation Fees** of 10 DAI are deducted.
* The remaining amount of DAI after fee deduction is Swapped to Push Tokens and refunded back to the Channel Owner
* Imperative on-chain details about the channel like _new Channel pool contribution, new Channel weight ,etc_ are updated in the contract
* Emits out a _**DeactivateChannel()**_ event with the _Channel's address, Total Refund amount value_

**7. reactivateChannel()**

```
  function reactivateChannel(uint256 _amount) external onlyDeactivatedChannels(msg.sender) {}
```

|    Argument    |    Type   |                           Description                          |
| :------------: | :-------: | :------------------------------------------------------------: |
| _**\_amount**_ | _uint256_ | DAI amount to be deposited for the reactivation of the channel |

**CheckPoints:**

* Channel must already be in a **DEACTIVATED STATE**.
* Total amount of DAI being deposited for Channel Reactivation must be greater than or equal to **50 DAI**

**Description:**

* Channel's state is changed from **DeActivated to ACTIVE state**
* Amount of DAI deposited for Channel reactivation is stored.
* Remaining amount of DAI after fee deduction is Swapped to PUSH Tokens and refunded back to the Chanenl Owner
* Channel's new pool contribution and weight is updated
* Emits out a _**ReactivateChannel()**_ event with the _Channel's address, Total Deposited amount value_

**8. blockChannel(address)**

```
  function blockChannel(address _channelAddress) external onlyPushChannelAdmin() onlyUnblockedChannels(_channelAddress){};
```

|        Argument        |    Type   |                 Description                 |
| :--------------------: | :-------: | :-----------------------------------------: |
| _**\_channelAddress**_ | _address_ | Address of the target channel to be blocked |

**CheckPoints:**

* Caller of the function should only be the Push Channel Admin
* Channel must not already be in a **BLOCKED** state

**Description:**

* Channel's state is changed to **BLOCKED** state.
* Once blocked, the channel address cannot be reactivated.
* Channel's pool contribution & weight are updated to new values and no refund shall be given to the Channel owner when blocked.
* Emits out a _**ChannelBlocked()**_ event with the _Channel's address_.

***

**9. verifyChannel(address)**

```
  function verifyChannel(address _channel) public onlyActivatedChannels(_channel) {};
```

|     Argument    |    Type   |                Description                |
| :-------------: | :-------: | :---------------------------------------: |
| _**\_channel**_ | _address_ | The address of the channel to be verified |

**CheckPoints:**

* The channel to be verified must be in an **ACTIVE** state, i.e., the channel must not be _blocked, inactive or deactivated_.
* The caller of this function should already be a verified channel or the Push Channel Admin.
* The target channel must not already be verified.

**Description:**

* Target channel is marked as a verified channel.
* The Verifier's address of the target channel is stored in the channel's struct. This determines the type of verification tag the target channel has. For instance:
  * If the Channel was verified directly by Push Channel Admin, it will have a **Primary Verification Tag.**
  * If the Channel was verified by any other verified channel, it will have a **Secondary Verification Tag.**
* Emits out an _**ChannelVerified()**_ event with the _Channel's address and the Verifier's Address_

**10. unverifyChannel(address)**

```
  function unverifyChannel(address _channel) public {};
```

|     Argument    |    Type   |                           Description                          |
| :-------------: | :-------: | :------------------------------------------------------------: |
| _**\_channel**_ | _address_ | The address of the channel whose verification shall be revoked |

**CheckPoints:**

* The caller of this function must be the **Verifier** of the target channel or the **Push Channel Admin** itself

**Description:**

* Marks the target channel as **Unverified**.
* Emits out an _**ChannelVerificationRevoked()**_ event with the _Channel's address and address that revoked the verification tag of the channel_

**11. swapAndTransferPUSH(address, uint256, uint256)**

```solidity
    function swapAndTransferPUSH(address _user, uint256 _userAmount, uint256 _amountsOutValue)
    internal
    returns (bool) {};
```

|       Description       |    Type   |                     Description                    |
| :---------------------: | :-------: | :------------------------------------------------: |
|       _**\_user**_      | _address_ |                 address of the user                |
|    _**\_userAmount**_   | _uint256_ |      User's amount to be swapped to Push token     |
| _**\_amountsOutValue**_ | _uint256_ | amountsOut value for Uniswap while swapping tokens |

_**Description:**_

* An internal function that helps in swapping aDai tokens back to PUSH token.
* First, the aDai tokens accumulated in the contract is redeemed to get back the DAI tokens
* The DAI token is then swapped to **PUSH** tokens using the _**UNISWAP\_V2\_ROUTER.**_

**12. updateChannelMeta()**

```
  function updateChannelMeta(address _channel, bytes calldata _newIdentity)
        external
        onlyChannelOwner(_channel) {};
```

|      Arguements     |    Type   |             Description            |
| :-----------------: | :-------: | :--------------------------------: |
|   _**\_channel**_   | _address_ |       address of the channel       |
| _**\_newIdentity**_ |  _bytes_  | New Identity bytes of the Channel. |

**CheckPoints:**

* This function must be called only by the Owner of the channel that is being updated

_**Description:**_

* Allows Channel Owner to update their Channel Description or any imperative detail
* Emits out an _**UpdateChannel()**_ event with the _Channel's address and The New Identity bytes of the Channel_

### Getter Functions

**13. getChannelState(address)**

```
function getChannelState(address _channel) external view returns(uint256 state) {};
```

|     Argument    |    Type   |                   Description                  |
| :-------------: | :-------: | :--------------------------------------------: |
| _**\_channel**_ | _address_ | address of the channel whose state is required |

_**Description:**_

* Returns the current state of the Channel.

**14. getChannelVerification(address)**

```
function getChannelVerfication(address _channel) public view returns (uint8 verificationStatus) {};
```

|     Argument    |    Type   |                       Description                       |
| :-------------: | :-------: | :-----------------------------------------------------: |
| _**\_channel**_ | _address_ | address of the channel whose verification tag is needed |

_**Description:**_

* Returns the verification tag of the channel's address passed in the argument.
* If the target channel is currently not verified by anyone, the function returns **0**.
* If the target channel was verified by the Push Channel admin itself, the function returns **1**. It means the channel has a **Primary verification tag**.
* If the target channel was verified by any other verified channel, the function returns **2**. It means the channel has a **Secondary verification tag**. _Note: It's quite important to keep the following Channel Verification procedure of Push Core in mind:_
* _If a Channel (C-A) with **Secondary verification tag** verifies other channels in the protocol but **later gets unverified by the Push Channel Admin**, all the other channels that were verified by that specific channel **'C-A**' get unverified as well._ For instance,

> 1. Push Channel Admin verifies Channel A - Primary Verification
> 2. Channel A verifies Channel B, C & D - Secondary Verification
> 3. Push channel admin revokes the verification of Channel A
> 4. Channel B, C, & D are unverfied as well

**15. \_readjustFairShareOfChannels(ChannelAction, uint256, uint256, uint256, uint256, uint256, uint256)**

```solidity
    function _readjustFairShareOfChannels(
        ChannelAction _action,
        uint256 _channelWeight,
        uint256 _oldChannelWeight,
        uint256 _groupFairShareCount,
        uint256 _groupNormalizedWeight,
        uint256 _groupHistoricalZ,
        uint256 _groupLastUpdate
    )
        private
        view
        returns (
            uint256 groupNewCount,
            uint256 groupNewNormalizedWeight,
            uint256 groupNewHistoricalZ,
            uint256 groupNewLastUpdate
        )
    {};
```

|           Arguments           |    Type   |              Description             |
| :---------------------------: | :-------: | :----------------------------------: |
|         _**\_action**_        |   _Enum_  |          Channel Action type         |
|     _**\_channelWeight**_     | _uint256_ |     Current weight of the channel    |
|    _**\_oldChannelWeight**_   | _uint256_ |       Old weight of the channel      |
|  _**\_groupFairShareCount**_  | _uint256_ |        Alias to channel Count        |
| _**\_groupNormalizedWeight**_ | _uint256_ |    Normalized weight of the groups   |
|    _**\_groupHistoricalZ**_   | _uint256_ |        The Historical Constant       |
|    _**\_groupLastUpdate**_    | _uint256_ | The last Channel Update block number |

_**Description:**_

* Readjusts fair share runs on channel addition, removal or update.
