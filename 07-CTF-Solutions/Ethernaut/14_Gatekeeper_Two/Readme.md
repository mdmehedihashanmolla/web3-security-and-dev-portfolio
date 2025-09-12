<div align="center">

# GatekeeperTwo

</div>



**GatekeeperTwo (Vulnerable Code) :**


<details>
<summary>Code</summary>

```solidity

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract GatekeeperTwo {
    address public entrant;

    modifier gateOne() {
        require(msg.sender != tx.origin);
        _;
    }

    modifier gateTwo() {
        uint256 x;
        assembly {
            x := extcodesize(caller())
        }
        require(x == 0);
        _;
    }

    modifier gateThree(bytes8 _gateKey) {
        require(uint64(bytes8(keccak256(abi.encodePacked(msg.sender)))) ^ uint64(_gateKey) == type(uint64).max);
        _;
    }

    function enter(bytes8 _gateKey) public gateOne gateTwo gateThree(_gateKey) returns (bool) {
        entrant = tx.origin;
        return true;
    }
}

```

</details>

---------

**Approach and Exploit Method Including Code :** 


1. **Understand the contract**

  - `GatekeeperTwo` protects `enter()` with three gates:

  - **Gate One:** `require(msg.sender != tx.origin)`  
    → the call must come from a **contract**, not directly from your EOA.

  - **Gate Two:** reads `extcodesize(caller())` and requires it to be `0`  
    → `extcodesize(addr)` returns the runtime code size of `addr`. For a contract **during its constructor** the runtime code is not yet stored, so `extcodesize` returns `0`. This forces you to call `enter()` **from inside a contract's constructor**.

  - **Gate Three:** `require(uint64(keccak256(abi.encodePacked(msg.sender))) ^ uint64(_gateKey) == type(uint64).max)`  
    → the 8-byte `_gateKey` must be the bitwise complement of the low-8-bytes of the hash of the **caller address** (the attacker contract). Concretely:  
      `key = bytes8(uint64(bytes8(keccak256(abi.encodePacked(attackerAddress)))) ^ 0xFFFFFFFFFFFFFFFF)`.

   - **Key idea:** pass the call from a contract (not your EOA), make the call **while that contract is being constructed** so `extcodesize` is 0, and supply a key derived from the attacker contract's address that satisfies the XOR-with-max condition.


2. **Exploit Method**

    i. Create an `Attack` contract whose **constructor** does the exploit (no external calls after deploy):

    - Inside the constructor compute:
    - `s = uint64(bytes8(keccak256(abi.encodePacked(address(this)))))`
    - `k = s ^ type(uint64).max`
    - `key = bytes8(k)`

    - From the constructor call `target.enter(key)` so the call originates from the contract under construction (satisfies Gate One and Gate Two).

    ii. Deploy the `Attack` contract passing the `GatekeeperTwo` address. The constructor will run the `enter` call immediately; because the contract is still constructing `extcodesize(caller)` is `0` and the crafted `key` meets Gate Three.

    iii. Verify success:
    - `gatekeeper.entrant()` should equal your EOA (`tx.origin`).

    **One-line summary:** call `enter` from inside an attack contract's constructor and pass `key = bytes8(uint64(keccak256(address(this))) ^ 0xFFFFFFFFFFFFFFFF)` so Gate One, Gate Two and Gate Three all pass.

## Exploit Code

```Solidity

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;


contract Attack{
    constructor(GatekeeperTwo target){


        uint64 s = uint64(bytes8(keccak256(abi.encodePacked(address(this)))));

         uint64 k =  s ^ type(uint64).max;

        bytes8 key = bytes8(k);
        require(target.enter(key), "failed");

    }
}


```

---


