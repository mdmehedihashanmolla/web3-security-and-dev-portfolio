<div align="center">

# Fallout

</div>



**Fallout (Vulnerable Code) :**


<details>
<summary>Code</summary>

```solidity

// SPDX-License-Identifier: MIT
pragma solidity ^0.6.0;

import "openzeppelin-contracts-06/math/SafeMath.sol";

contract Fallout {
    using SafeMath for uint256;

    mapping(address => uint256) allocations;
    address payable public owner;

    /* constructor */
    function Fal1out() public payable {
        owner = msg.sender;
        allocations[owner] = msg.value;
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "caller is not the owner");
        _;
    }

    function allocate() public payable {
        allocations[msg.sender] = allocations[msg.sender].add(msg.value);
    }

    function sendAllocation(address payable allocator) public {
        require(allocations[allocator] > 0);
        allocator.transfer(allocations[allocator]);
    }

    function collectAllocations() public onlyOwner {
        msg.sender.transfer(address(this).balance);
    }

    function allocatorBalance(address allocator) public view returns (uint256) {
        return allocations[allocator];
    }
}

```

</details>

---------

**Approach and Exploit Method Including Code :** 

1. **Understand the contract**
   - The function `Fal1out()` was **intended to be a constructor**, but due to the typo (`Fal1out` instead of `Fallout`) it became a **public function**.  
   - This means **anyone can call it** after deployment and set themselves as `owner`.  
   - The `owner` can then call `collectAllocations()` to drain the entire contract balance.  

2. **Exploit steps**
   - Call `Fal1out()` → ownership is transferred to us.  
   - Verify with `owner()` → attacker’s address is now the owner.  
---

## 💻 Exploit Interface

```solidity
// SPDX-License-Identifier: MIT
pragma solidity 0.8;

interface Fallout {
    function owner() external view returns (address);
    function Fal1out() external payable;
}
```

---