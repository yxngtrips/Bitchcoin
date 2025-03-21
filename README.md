# Bitchcoin
Memecoin smart contract 
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/access/Ownable.sol";

contract Bitchcoin is ERC20, Ownable {
    uint256 public constant TOTAL_SUPPLY = 420_000_000_000_000 * 10**18; // 420T BITCH
    
    uint256 public transactionTax = 3; // 3% transaction tax (adjustable)
    address public taxWallet;

    constructor() ERC20("Bitchcoin", "BITCH") {
        _mint(msg.sender, TOTAL_SUPPLY);
        taxWallet = msg.sender; // Owner receives tax initially
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
