# InvertedBTC Smart Contract

An ERC-20 token smart contract built on the Ethereum blockchain, featuring a dynamic transaction tax system and owner-controlled parameters.

## Overview

InvertedBTC (ticker: `BITCH`) is a Solidity-based memecoin contract that demonstrates core ERC-20 token mechanics alongside custom transfer logic, including a configurable transaction tax routed to a designated wallet.

**Total Supply:** 420,000,000,000,000 tokens  
**Standard:** ERC-20 (OpenZeppelin)  
**Network:** Ethereum-compatible (EVM)

---

## Features

- ✅ ERC-20 compliant token using OpenZeppelin's audited library
- ✅ Configurable transaction tax (default 3%, max 10%)
- ✅ Owner-controlled tax wallet address
- ✅ Ownable access control pattern
- ✅ Custom `_transfer` override for tax deduction logic

---

## Smart Contract Logic

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/access/Ownable.sol";

contract Bitchcoin is ERC20, Ownable {
    uint256 public constant TOTAL_SUPPLY = 420_000_000_000_000 * 10**18;
    uint256 public transactionTax = 3;
    address public taxWallet;

    constructor() ERC20("Bitchcoin", "BITCH") {
        _mint(msg.sender, TOTAL_SUPPLY);
        taxWallet = msg.sender;
    }

    function setTaxWallet(address _wallet) external onlyOwner {
        taxWallet = _wallet;
    }

    function setTransactionTax(uint256 _tax) external onlyOwner {
        require(_tax <= 10, "Tax too high! Max is 10%");
        transactionTax = _tax;
    }

    function _transfer(address sender, address recipient, uint256 amount) internal override {
        uint256 taxAmount = (amount * transactionTax) / 100;
        uint256 amountAfterTax = amount - taxAmount;
        super._transfer(sender, taxWallet, taxAmount);
        super._transfer(sender, recipient, amountAfterTax);
    }
}
```

---

## Tech Stack

- **Language:** Solidity ^0.8.0
- **Framework:** OpenZeppelin Contracts
- **Standard:** ERC-20

---

## What I Learned

- Writing and structuring ERC-20 token contracts from scratch
- Inheriting from OpenZeppelin's audited base contracts
- Implementing access control with `Ownable`
- Overriding internal transfer logic to add custom tax mechanics

---

## License

MIT
