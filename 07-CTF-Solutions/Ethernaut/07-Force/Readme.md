<div align="center">

# Force

</div>



**Force (Vulnerable Code) :**


<details>
<summary>Code</summary>

```solidity

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract Force { /*
                   MEOW ?
         /\_/\   /
    ____/ o o \
    /~____  =ø= /
    (______)__m_m)
                   */ }

```

</details>

---------

**Approach and Exploit Method Including Code :** 

1. **Understand the contract**
   - The `Force` contract has **no payable functions** and **cannot receive ETH via normal transfers**.
   - Its balance is initially 0.  
   - Since there’s no way to deposit ETH normally, sending ETH directly to this contract using `send` or `transfer` **will fail**.  
   - **Key idea:** even contracts that don’t have payable functions can receive ETH via `selfdestruct()`.

2. **Exploit Method**
   - Deploy an `Attack` contract with a constructor that takes the `Force` contract address as `_target`.  
   - Send some ETH while deploying the `Attack` contract (`payable constructor`).  
   - The constructor calls `selfdestruct(_target)`, which **forces the ETH** to be sent to the `Force` contract regardless of its code.  
   - After deployment, check `force.balance` — it should now be greater than 0.


  
## Exploit Code

```Solidity

contract Hack{
    constructor (address payable _target) payable {
        selfdestruct(_target);
    }
}


```



---


