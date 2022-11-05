---
description: >-
  Explains the different types of Data types and access controls used in the
  Push Core smart contract
---

# Types & Modifiers - Core

### A. Storage Variables

```
    string public constant name = "EPNS CORE V1";
    address public pushChannelAdmin;
    address public governance;
    address public daiAddress;
    address public aDaiAddress;
    address public WETH_ADDRESS;
    address public epnsCommunicator;
    address public UNISWAP_V2_ROUTER;
    address public PUSH_TOKEN_ADDRESS;
    address public lendingPoolProviderAddress;

    uint256 public REFERRAL_CODE;
    uint256 ADJUST_FOR_FLOAT;
    uint256 public channelsCount;

    //  @notice Helper Variables for FSRatio Calculation | GROUPS = CHANNELS
    uint256 public groupNormalizedWeight;
    uint256 public groupHistoricalZ;
    uint256 public groupLastUpdate;
    uint256 public groupFairShareCount;

    // @notice Necessary variables for Keeping track of Funds and Fees
    uint256 public POOL_FUNDS;
    uint256 public PROTOCOL_POOL_FEES;
    uint256 public ADD_CHANNEL_MIN_FEES;
    uint256 public CHANNEL_DEACTIVATION_FEES;
    uint256 public ADD_CHANNEL_MIN_POOL_CONTRIBUTION;
```

**A.1 To keep track of FUNDS and FEES in Push Core**

* **POOL\_FUNDS:**
  * Keeps track of the total amount of DAI in the protocol.
  * Incremented whenever a new Channel is created by depositing some DAI or a channel is reactivated by paying Channel Reactivation fees in DAI.
  * Decremented whenever a specific amount of DAI is swapped to PUSH and given back to users.
* **PROTOCOL\_POOL\_FEES:**
  * Keeps track of the non-refundable amount of DAI whenever a channel is blocked.
* **ADD\_CHANNEL\_MIN\_FEES:**
  * The minimum amount of DAI that is required for creating or reactivating a channel.
  * Current value of this state variable is **50 DAI.**
  * Can be updated only via on-chain governance using the _**setMinChannelCreationFees()**_ function.
  * Can never be below **50 DAI**
* **CHANNEL\_DEACTIVATION\_FEES:**
  * Represents deactivation fee charged to a channel owner when the channel is Deactivated.
  * Current value of this state variable is **10 DAI.**
  * Can be updated only via on-chain governance using the _**setChannelDeactivationFees()**_ function.
* **ADD\_CHANNEL\_MIN\_POOL\_CONTRIBUTION:**
  * Represents the constant value of 50 DAI used for the calculation of a channel's weight in the protocol.

**A.2 Storage variables for FSRatio Calculation**

* **groupNormalizedWeight:**
  * Represents the normalized weight of all channels available in the smart contract
* **groupHistoricalZ:**
  * A historical constant used for fair share ratio calculation
* **groupLastUpdate:**
  * Represents the last channel Update block number
* **groupFairShareCount:**
  * Indicates the FS count in the protocol

### B. STRUCTS

```
struct Channel {
        ChannelType channelType;

        uint8 channelState;

        address verifiedBy;

        uint256 poolContribution;

        uint256 channelHistoricalZ;

        uint256 channelFairShareCount;

        uint256 channelLastUpdate;

        uint256 channelStartBlock;

        uint256 channelUpdateBlock;

        uint256 channelWeight;
    }
```

The **Channel** struct in the Push Core smart contract stores every crucial data about the channels that are created on the core contract.

* **ChannelType**
  * Denotes the type of channel being created.
  * A Channel can be any of the 4 available types:
    1. ProtocolNonInterest
    2. ProtocolPromotion
    3. InterestBearingOpen
    4. InterestBearingMutual
* **channelState**
  * Symbolizes the current state of a particular channel
  * A channel can have any of the following states:
    1. INACTIVE
    2. ACTIVE
    3. DEACTIVATED
    4. BLOCKED
* **verifiedBy**
  * Denotes the address of the verifier of the Channel
* **poolContribution**
  * Denotes the total amount of DAI deposited by the channel owner during Channel Creation\*\*
* **channelHistoricalZ**
  * Represents the Historical Constant
* **channelFairShareCount**
  * Represents the FS Count of the channel
* **channelLastUpdate**
  * The last update block number. It is used to calculate the fair share ratio
* **channelStartBlock**
  * Represents the block number when a specific channel was created
* **channelUpdateBlock**
  * Represents the block number that depicts when a channel was updated
* **channelWeight**
  * Represents the individual weight to be applied as per pool contribution.

### C. MODIFIERS

*   **onlyPushChannelAdmin()**

    Only allows Push Channel Admin to access the function
* **onlyGovernance()**
  * Only allows Governance contract to access the function
* **onlyInactiveChannels()**
  * Only for channels that are currently in an **INACTIVE** state
  * Used in the following function:
    * **createChannelWithFees()**
* **onlyActivatedChannels()**
  * Only for channels that are currently in an **ACTIVE** state
  * Used in the following functions:
    * **createChannelSettings()**
    * **deactivateChannel()**
    * **verifyChannel()**
* **onlyDeactivatedChannels()**
  * Only for channels that are neither in **BLOCKED** state nor **INACTIVE**
  * Used in the following function:
    * **reactivateChannel()**
* **onlyUnblockedChannels()**
  * Only for channels that are currently in a **BLOCKED** state
  * Used in the following function:
    * **blockChannel()**
* **onlyChannelOwner()**
  * Only for the owner of a particular channel
  * Used in the following function:
    * **updateChannelMeta()**
    *
* **onlyUserAllowedChannelType()**
  * Ensures that the channel type passed as an argument while creating a channel is a valid channel type
  * Used in the following function:
    * **createChannelWithFees()**

### D. MAPPINGS

* **channels**
  * `mapping(address => Channel) public channels;`
  * Maps a channel's address to its Struct
* **channelById**
  * `mapping(uint256 => address) public channelById;`
  * Maps the uint256 ID of a particular channel to its address.
  * Updated in the **\_createChannel()** function.
* **channelNotifSettings**
  * `mapping(address => string) public channelNotifSettings;`
  * Keeps track of the notification settings selected by a channel
  * Updated in the **createChannelSettings()** function.

### E. ENUMS

The EPNS Core smart contract includes 2 main ENUMS.

*   **ChannelType**

    ```
    enum ChannelType {
        ProtocolNonInterest,
        ProtocolPromotion,
        InterestBearingOpen,
        InterestBearingMutual
    }
    ```

    * This represents the type of channel being created.
    * It can be any one of the 4 types:
      * ProtocolNonInterest,
      * ProtocolPromotion,
      * InterestBearingOpen,
      * InterestBearingMutual
*   **ChannelAction**

    ```
        enum ChannelAction {
        ChannelRemoved,
        ChannelAdded,
        ChannelUpdated
        }
    ```

    * This represents the different channel actions that occur in the protocol.
    * There can be 3 main channel actions:
      * ChannelAdded: _When a channel is created and added to the protocol_
      * ChannelRemoved: _When a channel is blocked and removed from the protocol_
      * ChannelUpdated: _When a channel is either deactivated or reactivated_
