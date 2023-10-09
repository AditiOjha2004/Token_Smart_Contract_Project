## Project Objective 

Create a smart contract that implements a custom token with the ability to mint new tokens and burn existing tokens. The contract should maintain public variables for token details and a mapping of addresses to balances. Mint and burn functions will be implemented with proper conditionals to ensure data consistency.

## Project Description 

The "Token" smart contract provides the framework for a custom token with the capability to mint and burn tokens. It features public variables storing token details like name and symbol, and a mapping to manage balances for various addresses. The "mint" function facilitates the creation of new tokens, increasing both total supply and recipient balances, while the "burn" function enables token destruction, reducing total supply and sender balances. The contract includes safeguards to ensure that token burning only occurs if the sender's balance is sufficient. It meets the specified requirements and offers a versatile foundation for managing token transactions and balances on a blockchain platform.

## Code Explanation

```solidity
// SPDX-License-Identifier: MIT
pragma solidity 0.8.18;

contract MyToken {
    string public tokenName = "Aditi";
    string public tokenSymbol = "abc";
    uint256 public totalSupply;
    mapping(address => uint256) public balances;

    event Transfer(address indexed from, address indexed to, uint256 value);

    constructor() {
        totalSupply = 0;
    }

    function mint(address _to, uint256 _value) external {
        require(_to != address(0), "Invalid address");
        totalSupply += _value;
        balances[_to] += _value;
        emit Transfer(address(0), _to, _value);
    }

    function burn(uint256 _value) external {
        require(balances[msg.sender] >= _value, "Insufficient balance");
        totalSupply -= _value;
        balances[msg.sender] -= _value;
        emit Transfer(msg.sender, address(0), _value);
    }

    function transfer(address _to, uint256 _value) external returns (bool) {
        require(_to != address(0), "Invalid address");
        require(balances[msg.sender] >= _value, "Insufficient balance");

        balances[msg.sender] -= _value;
        balances[_to] += _value;

        emit Transfer(msg.sender, _to, _value);
        return true;
    }

    function getInfo() external view returns (string memory, string memory) {
        return (tokenName, tokenSymbol);
    }
}
```

The provided Solidity smart contract is a basic implementation of a token contract called "MyToken." It defines a simple ERC-20-like token with functionalities to mint new tokens, burn existing tokens, transfer tokens between addresses, and retrieve information about the token.

In the contract's state variables section, there are several important variables. tokenName and tokenSymbol are publicly accessible strings that store the name and symbol of the token, respectively. These variables are used to provide human-readable information about the token. The totalSupply is an unsigned integer that represents the total supply of the token, which starts at 0 when the contract is deployed. Lastly, there's a balances mapping that associates Ethereum addresses with their respective token balances. This mapping is crucial for tracking the ownership of tokens by different addresses.

The contract also includes an event declaration called Transfer. This event is used to log token transfers between addresses. Events are a way to provide transparency and allow external applications to monitor and react to specific contract events. In this case, the Transfer event logs the sender's address (from), the recipient's address (to), and the amount of tokens transferred (value).

The constructor function of the contract is executed only once during contract deployment. In this contract, the constructor initializes the totalSupply to 0. This means that initially, there are no tokens in circulation, and new tokens must be minted.

The mint function allows the contract owner (or anyone with permission) to create new tokens and assign them to a specified address. It checks for a valid recipient address and then increases the total supply and updates the balance of the recipient accordingly. It also emits a Transfer event to log the creation of new tokens.

The burn function enables token holders to destroy their own tokens, reducing the total supply. It verifies that the caller has a sufficient token balance before allowing the burn operation. The function then deducts the burned tokens from the caller's balance, updates the total supply accordingly, and emits a Transfer event to log the destruction of tokens.

The transfer function facilitates the transfer of tokens between addresses. It ensures that the recipient address is valid and that the sender has enough tokens to transfer. If these conditions are met, it updates the balances of both the sender and the recipient and logs the transfer using the Transfer event.

Lastly, the getInfo function is a read-only function that allows anyone to retrieve the name and symbol of the token without making any state changes. It returns the tokenName and tokenSymbol as strings.


