<div align="center">

# Vault

</div>



**Vault (Vulnerable Code) :**


<details>
<summary>Code</summary>

```solidity

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract Vault {
    bool public locked;
    bytes32 private password;

    constructor(bytes32 _password) {
        locked = true;
        password = _password;
    }

    function unlock(bytes32 _password) public {
        if (password == _password) {
            locked = false;
        }
    }
}

```

</details>

---------

**Approach and Exploit Method Including Code :** 


1. **Understand the contract**
   - `Vault` has two key variables:
     - `locked` → a boolean indicating whether the vault is locked.
     - `password` → a `bytes32` value marked `private`.
   - The `unlock(bytes32 _password)` function sets `locked = false` if the input matches the stored password.
   - **Important:** In Solidity, `private` only restricts access **within the contract code**. The data is still stored on-chain and can be read by anyone with access to the blockchain.

2. **Exploit Method**
   - Access the deployed Vault instance (via console or web3).  
   - Read the password directly from storage:
     ```solidity
     await web3.eth.getStorageAt(contract.address, 1)
     ```
   - Use the retrieved password to call `unlock(password)` on the Vault contract.  
   - Verify that `locked` is now `false` — the vault is successfully unlocked.

---


