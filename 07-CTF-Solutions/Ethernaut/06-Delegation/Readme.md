<div align="center">

# Delegatecall

</div>



**Delegatecall (Vulnerable Code) :**


<details>
<summary>Code</summary>

```solidity

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract Delegate {
    address public owner;

    constructor(address _owner) {
        owner = _owner;
    }

    function pwn() public {
        owner = msg.sender;
    }
}

contract Delegation {
    address public owner;
    Delegate delegate;

    constructor(address _delegateAddress) {
        delegate = Delegate(_delegateAddress);
        owner = msg.sender;
    }

    fallback() external {
        (bool result,) = address(delegate).delegatecall(msg.data);
        if (result) {
            this;
        }
    }
}

```

</details>

---------

**Approach and Exploit Method Including Code :** 

1. **Understand the contract**

- `Delegate` has an `owner` variable and a `pwn()` function that sets `owner = msg.sender`.  
- `Delegation` also has an `owner` variable and stores a `Delegate` instance. Its `fallback()` forwards **any calldata** to the `Delegate` using `delegatecall`.  
- Important behaviour of `delegatecall`:
  - `delegatecall` runs the **code** of `Delegate` but **in the storage context of `Delegation`**. That means any storage writes inside `Delegate` affect `Delegation`'s storage.  
  - Both contracts put `owner` in the same storage slot (slot 0). So when `Delegate.pwn()` runs via `delegatecall`, it writes the caller address into `Delegation`'s `owner` slot.  
- Analogy: `Delegate` is a worker with a button `pwn()` that writes a name into a notebook. `Delegation` is the house whose notebook lines up with the worker’s. Using `delegatecall` is like asking the worker to press the button but the worker writes into the house’s notebook — it ends up recording your name as the house owner.


2. **Exploit Method**

    i. Open the Ethernaut console and get the Delegate contract address from the instance:  
    - `contract.address` ← copy this address from the console log.

    ii. In Remix IDE use **At Address** and paste the copied address to load the **Delegate** contract instance.

    iii. In Remix call `pwn()` on that loaded Delegate contract (no ETH required).

    iv. Verify ownership:  
    - Check `delegation.owner()` (in Ethernaut console or via Remix) — it should now be your EOA address.



---


