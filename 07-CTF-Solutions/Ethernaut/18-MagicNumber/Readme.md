<div align="center">

# MagicNum

</div>



**MagicNum (Vulnerable Code) :**


<details>
<summary>Code</summary>

```solidity

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract MagicNum {
    address public solver;

    constructor() {}

    function setSolver(address _solver) public {
        solver = _solver;
    }

    /*
    ____________/\\\_______/\\\\\\\\\_____        
     __________/\\\\\_____/\\\///////\\\___       
      ________/\\\/\\\____\///______\//\\\__      
       ______/\\\/\/\\\______________/\\\/___     
        ____/\\\/__\/\\\___________/\\\//_____    
         __/\\\\\\\\\\\\\\\\_____/\\\//________   
          _\///////////\\\//____/\\\/___________  
           ___________\/\\\_____/\\\\\\\\\\\\\\\_ 
            ___________\///_____\///////////////__
    */
}


```

</details>

---------

**Approach and Exploit Method Including Code :** 

1. **Understand the contract**

   - `MagicNum` requires a `solver` contract that responds to `whatIsTheMeaningOfLife()` with the correct 32-byte number.  
   - The challenge imposes a **size constraint**: the solver contract must be tiny (≤10 bytes of runtime bytecode).  
   - **Key idea:** Solidity compiler may produce larger bytecode, so the solution requires writing **raw EVM bytecode** manually to meet the size limit.

2. **Vulnerability Highlight (key part)**


```solidity
address public solver;

function setSolver(address _solver) public {
    solver = _solver;
}
```


## Exploit Code

```Solidity

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract Attack{

    constructor(MagicNum _target){
        address addr;

        bytes memory bytecode = hex"69602a60005260206000f3600052600a6016f3";

        assembly {
            addr := create(0, add(bytecode, 0x20),0x13)
        }
        require(addr != address(0));

        _target.setSolver(addr);
    }

}


```

---


