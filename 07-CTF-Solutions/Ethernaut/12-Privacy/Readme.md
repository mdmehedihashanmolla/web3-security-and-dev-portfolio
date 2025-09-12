<div align="center">

# Privacy

</div>



**Privacy (Vulnerable Code) :**


<details>
<summary>Code</summary>

```solidity

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract Privacy {
    bool public locked = true;
    uint256 public ID = block.timestamp;
    uint8 private flattening = 10;
    uint8 private denomination = 255;
    uint16 private awkwardness = uint16(block.timestamp);
    bytes32[3] private data;

    constructor(bytes32[3] memory _data) {
        data = _data;
    }

    function unlock(bytes16 _key) public {
        require(_key == bytes16(data[2]));
        locked = false;
    }

    /*
    A bunch of super advanced solidity algorithms...

      ,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`
      .,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,
      *.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^         ,---/V\
      `*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.    ~|__(o.o)
      ^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'  UU  UU
    */
}

```

</details>

---------

**Approach and Exploit Method Including Code :** 


1. **Understand the contract**

- `Privacy` stores several variables packed across storage slots.  
- `data` is a `bytes32[3]` array stored at slots `3`, `4`, and `5` (`data[2]` lives in slot 5).  
- `unlock(bytes16 _key)` checks `require(_key == bytes16(data[2]))` — i.e., the level compares the _leftmost_ 16 bytes of `data[2]` with your supplied key.  
- **Key idea:** `private` does not hide on-chain storage — you can read slot 5 and recover the secret.

2. **Exploit Method**

    i. Read the raw storage at slot 5 (single terminal / console):  
    - `await web3.eth.getStorageAt(contract.address, 5)` ← copy the returned 32-byte hex.

    ii. Convert that returned 32-byte hex to a `bytes16` by taking the leftmost 16 bytes (do this in the same console or in Remix).

    iii. Attach the deployed `Privacy` instance in Remix (**At Address**) or use the console, and call `unlock(<bytes16 key>)` with the converted value.

    iv. Verify success: `await contract.locked()` → should return `false`.

    **One-line summary:** read slot `5`, extract the leftmost 16 bytes, call `unlock(...)`, done.



---


