<div align="center">

# Fallback

</div>



**Fallback (Vulnerable Code) :**


<details>
<summary>Code</summary>

```solidity

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract Fallback {
    mapping(address => uint256) public contributions;
    address public owner;

    constructor() {
        owner = msg.sender;
        contributions[msg.sender] = 1000 * (1 ether);
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "caller is not the owner");
        _;
    }

    function contribute() public payable {
        require(msg.value < 0.001 ether);
        contributions[msg.sender] += msg.value;
        if (contributions[msg.sender] > contributions[owner]) {
            owner = msg.sender;
        }
    }

    function getContribution() public view returns (uint256) {
        return contributions[msg.sender];
    }

    function withdraw() public onlyOwner {
        payable(owner).transfer(address(this).balance);
    }

    receive() external payable {
        require(msg.value > 0 && contributions[msg.sender] > 0);
        owner = msg.sender;
    }
}

```

</details>

---------

**Approach and Exploit Method :** 

1. **Understand the contract**
   - `owner` can withdraw all funds.  
   - `contribute()` lets us increase our balance in the mapping.  
   - `receive()` has a bug:  
     ```solidity
     require(msg.value > 0 && contributions[msg.sender] > 0);
     owner = msg.sender;
     ```
     → If we send ETH with empty calldata *and* have a non-zero contribution, we become `owner`.

2. **Exploit Method**
   - Call `contribute()` with a very small amount (e.g., `1 wei`) → this gives `contributions[msg.sender] > 0`.  
   - Send a transaction with **empty calldata** but with some ETH (`1 wei`) → triggers `receive()` and sets `owner = msg.sender`.  
   - Now that we are `owner`, call `withdraw()` to drain the contract.

---