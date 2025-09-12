<div align="center">

# Telephone

</div>



**Telephone (Vulnerable Code) :**


<details>
<summary>Code</summary>

```solidity

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract Telephone {
    address public owner;

    constructor() {
        owner = msg.sender;
    }

    function changeOwner(address _owner) public {
        if (tx.origin != msg.sender) {
            owner = _owner;
        }
    }
}

```

</details>

---------

**Approach and Exploit Method Including Code :** 

1. **Understand the contract**
   - `owner` is set in the constructor to the deployer.
   - `changeOwner(address _owner)` sets `owner = _owner` only when `tx.origin != msg.sender`.
   - `tx.origin` is the original external account that started the transaction; `msg.sender` is the immediate caller (could be a contract).
   - If a contract calls `changeOwner`, then inside `changeOwner`:
     - `tx.origin` = your EOA (attacker)
     - `msg.sender` = the attack contract address
     - so `tx.origin != msg.sender` → the check passes and ownership can be changed.

2. **Exploit Method**
   - Deploy a contract that calls `telephone.changeOwner(msg.sender)` from within the contract.
   - When you (EOA) call the attack contract’s `attack()` function, `tx.origin` is your EOA and `msg.sender` inside `changeOwner` is the attack contract — condition satisfied.
   - The target contract will set `owner` to the address you pass (`msg.sender` from the EOA perspective — the attacker EOA).

  
## Exploit Code

```Solidity

contract Attack {
    Telephone public phone;

    constructor(address _target){
        phone = Telephone(_target);
    }
    function attack()public {
        phone.changeOwner(msg.sender);
    }

}

```



---


