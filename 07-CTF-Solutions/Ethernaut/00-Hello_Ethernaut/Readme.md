<div align="center">

# Hello Ethernaut

</div>



**Hello Ethernaut (Vulnerable Code) :**


<details>
<summary>Code</summary>

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract Instance {
    string public password;
    uint8 public infoNum = 42;
    string public theMethodName = "The method name is method7123949.";
    bool private cleared = false;

    // constructor
    constructor(string memory _password) {
        password = _password;
    }

    function info() public pure returns (string memory) {
        return "You will find what you need in info1().";
    }

    function info1() public pure returns (string memory) {
        return 'Try info2(), but with "hello" as a parameter.';
    }

    function info2(string memory param) public pure returns (string memory) {
        if (keccak256(abi.encodePacked(param)) == keccak256(abi.encodePacked("hello"))) {
            return "The property infoNum holds the number of the next info method to call.";
        }
        return "Wrong parameter.";
    }

    function info42() public pure returns (string memory) {
        return "theMethodName is the name of the next method.";
    }

    function method7123949() public pure returns (string memory) {
        return "If you know the password, submit it to authenticate().";
    }

    function authenticate(string memory passkey) public {
        if (keccak256(abi.encodePacked(passkey)) == keccak256(abi.encodePacked(password))) {
            cleared = true;
        }
    }

    function getCleared() public view returns (bool) {
        return cleared;
    }
}
}
```

</details>

---------

**Approach and Exploit Method :** 

1. **Explore the contract ABI**  
   - Used the `contract` object in browser console to inspect available methods.  

2. **Follow the hints in `info()` functions**  
   - `info()` → said check `info1()`  
   - `info1()` → said call `info2("hello")`  
   - `info2("hello")` → hinted to check `info42()`  
   - `info42()` → revealed the method name `method7123949()`  
   - `method7123949()` → told me to use the password with `authenticate()`  

3. **Get the password**  
   - Read the public storage variable:  
     ```javascript
     await contract.password()
     ```

4. **Authenticate with password**  
   - Called:
     ```javascript
     await contract.authenticate("<password>")
     ```

✅ This set `cleared = true`, completing the level.

---