# AL365TokenEnhanced
AL365 Enhanced Token, BEP-20 tabanlı bir token olup durdurulabilir, yakılabilir ve basılabilir özelliklere sahiptir. Bu repository, tokenin Solidity ile yazılmış akıllı kontratını içermektedir.
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IERC20 {
    // ... (önceki IERC20 interface detayları burada)
}

contract AL365TokenEnhanced is IERC20 {
    string public constant name = "AL365 Enhanced";
    string public constant symbol = "AL365E";
    uint8 public constant decimals = 18;
    uint256 private _totalSupply = 240000000 * (10 ** uint256(decimals));

    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;

    address public owner;
    bool public paused = false;

    modifier onlyOwner() {
        require(msg.sender == owner, "Not the contract owner");
        _;
    }

    modifier whenNotPaused() {
        require(!paused, "Token transfers are paused");
        _;
    }

    constructor() {
        _balances[msg.sender] = _totalSupply;
        emit Transfer(address(0), msg.sender, _totalSupply);
        owner = msg.sender;
    }

    function pause() external onlyOwner {
        paused = true;
    }

    function unpause() external onlyOwner {
        paused = false;
    }

    function mint(address to, uint256 amount) external onlyOwner {
        _totalSupply += amount;
        _balances[to] += amount;
        emit Transfer(address(0), to, amount);
    }

    function burn(uint256 amount) external {
        require(_balances[msg.sender] >= amount, "Insufficient balance");
        _balances[msg.sender] -= amount;
        _totalSupply -= amount;
        emit Transfer(msg.sender, address(0), amount);
    }

    function transfer(address recipient, uint256 amount) external override whenNotPaused returns (bool) {
        // ... (önceki transfer fonksiyonu kodları burada)
    }

    // ... (diğer IERC20 fonksiyonları ve yardımcı fonksiyonlar burada)
}
