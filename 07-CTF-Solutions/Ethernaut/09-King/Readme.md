<div align="center">

# King

</div>



**King (Vulnerable Code) :**


<details>
<summary>Code</summary>

```solidity

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract King {
    address king;
    uint256 public prize;
    address public owner;

    constructor() payable {
        owner = msg.sender;
        king = msg.sender;
        prize = msg.value;
    }

    receive() external payable {
        require(msg.value >= prize || msg.sender == owner);
        payable(king).transfer(msg.value);
        king = msg.sender;
        prize = msg.value;
    }

    function _king() public view returns (address) {
        return king;
    }
}

```

</details>

---------

**Approach and Exploit Method Including Code :** 

1. **Understand the contract**

   - `King` keeps track of the current `king` and the current `prize` (amount required to become king).  
   - On receiving ETH (`receive()`), the contract requires `msg.value >= prize` (unless sender is `owner`), then **pays the previous king** with `payable(king).transfer(msg.value)`, and finally updates `king = msg.sender` and `prize = msg.value`.  
   - **Key weakness:** `transfer` will revert if the recipient (the previous king) cannot accept ETH. If the current `king` is an address that refuses payment, any future attempts to become king will revert at the `transfer` step — locking the throne.

---

1. **Exploit Method**

- **Goal:** Become king with an address that **cannot receive refunds**, so future dethrone attempts fail.

    Steps:
    i. Read the current `prize` from the target:  
    - `prize = await contract.prize()` (or via the console).
    ii. Deploy or use an **Attack** contract that either has no payable `receive`/`fallback` or explicitly reverts when it receives ETH (so it cannot be refunded).
    iii. During deployment (or in an immediate constructor call), send `value = prize` (or more) in a transaction that calls the `King` contract — this makes the `Attack` contract the new `king`.
    iv. Because `Attack` refuses refunds, any subsequent calls to `King.receive()` will revert at `payable(king).transfer(msg.value)`, preventing the level from reclaiming kingship.  
    v. Verify by checking `king._king()` (or the `King`'s getter) — it should return the `Attack` contract address.



  
## Exploit Code

```Solidity

contract Attack{
    constructor (address payable  target) payable {
        uint prize = King(target).prize();
        (bool ok,) = target.call{value : prize}("");

        require(ok, "call failed");
    }
}


```



---


