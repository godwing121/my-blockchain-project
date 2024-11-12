# DeFi Empire: A DeFi Kingdom Clone on Avalanche

This project is a simplified DeFi Kingdom clone on Avalanche, allowing players to collect, build, and earn rewards through game activities. By deploying on a custom EVM subnet, we gain control over network fees, native currency, and other subnet parameters, making it ideal for a gaming environment.

## Table of Contents

- [Introduction](#introduction)
- [Features](#features)
- [Prerequisites](#prerequisites)
- [Contracts](#contracts)
  - [ERC20 Token](#erc20-token)
  - [Vault](#vault)
- [Deployment Steps](#deployment-steps)
- [Testing](#testing)
- [Submission Requirements](#submission-requirements)

## Introduction

In this project, we set up a custom EVM subnet on the Avalanche network and deploy two foundational smart contracts: an ERC20 token and a Vault. These contracts serve as the foundation for game activities in a decentralized finance (DeFi) gaming application, where players use native tokens to interact within the game.

## Features

- Custom EVM subnet on Avalanche for low-cost transactions and custom game logic.
- ERC20 token as the game's native currency.
- Vault contract enabling users to deposit and withdraw tokens.
- Easy connection to MetaMask, enabling in-game asset storage and trading.

## Prerequisites

- **Node.js** and **npm** or **yarn** for managing dependencies.
- **Avalanche CLI** to create a custom subnet.
- **MetaMask** for wallet integration.
- **Remix** for contract deployment.

## Contracts

### ERC20 Token

The ERC20 contract creates an in-game token that can be used for transactions, rewards, and other game functions.

#### ERC20 Solidity Code

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract ERC20 {
    uint public totalSupply;
    mapping(address => uint) public balanceOf;
    mapping(address => mapping(address => uint)) public allowance;
    string public name = "GameToken";
    string public symbol = "GT";
    uint8 public decimals = 18;

    event Transfer(address indexed from, address indexed to, uint value);
    event Approval(address indexed owner, address indexed spender, uint value);

    function transfer(address recipient, uint amount) external returns (bool) {
        balanceOf[msg.sender] -= amount;
        balanceOf[recipient] += amount;
        emit Transfer(msg.sender, recipient, amount);
        return true;
    }

    function approve(address spender, uint amount) external returns (bool) {
        allowance[msg.sender][spender] = amount;
        emit Approval(msg.sender, spender, amount);
        return true;
    }

    function transferFrom(
        address sender,
        address recipient,
        uint amount
    ) external returns (bool) {
        allowance[sender][msg.sender] -= amount;
        balanceOf[sender] -= amount;
        balanceOf[recipient] += amount;
        emit Transfer(sender, recipient, amount);
        return true;
    }

    function mint(uint amount) external {
        balanceOf[msg.sender] += amount;
        totalSupply += amount;
        emit Transfer(address(0), msg.sender, amount);
    }

    function burn(uint amount) external {
        balanceOf[msg.sender] -= amount;
        totalSupply -= amount;
        emit Transfer(msg.sender, address(0), amount);
    }
}
```

### Vault

The Vault contract enables users to deposit and withdraw tokens, earning "shares" that represent their balance in the contract.

#### Vault Solidity Code

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

interface IERC20 {
    function balanceOf(address account) external view returns (uint);
    function transferFrom(address sender, address recipient, uint amount) external returns (bool);
    function transfer(address recipient, uint amount) external returns (bool);
}

contract Vault {
    IERC20 public immutable token;
    uint public totalSupply;
    mapping(address => uint) public balanceOf;

    constructor(address _token) {
        token = IERC20(_token);
    }

    function deposit(uint _amount) external {
        uint shares = totalSupply == 0 ? _amount : (_amount * totalSupply) / token.balanceOf(address(this));
        totalSupply += shares;
        balanceOf[msg.sender] += shares;
        token.transferFrom(msg.sender, address(this), _amount);
    }

    function withdraw(uint _shares) external {
        uint amount = (_shares * token.balanceOf(address(this))) / totalSupply;
        totalSupply -= _shares;
        balanceOf[msg.sender] -= _shares;
        token.transfer(msg.sender, amount);
    }
}
```

## Deployment Steps

1. **Deploy the Subnet**: Use the Avalanche CLI to launch your subnet and connect MetaMask to this subnet.
2. **Deploy Contracts with Remix**: Connect Remix to MetaMask and deploy the ERC20 token and Vault contracts.
3. **Verify Transactions**: Ensure all deployment and interaction transactions are successful.

## Testing

Use Remix to interact with deployed contracts by:
- Minting tokens in the ERC20 contract.
- Depositing and withdrawing tokens in the Vault contract to test shares and balances.

 ## Contact 
Godwin 

sagayamgodwin19@gmail.com

This README provides a comprehensive guide for deploying and testing ERC20 and Vault contracts on an Avalanche subnet, essential for a DeFi Kingdom-style project on Avalanche.
