#  Developing smart contracts
Welcome to the exciting world of smart contract development! This guide will let you get started writing Solidity contracts by going over the following:

[Setting up a Solidity Project](#setting-up-a-project)

[Compiling Solidity Source Code](#compiling-solidity)

[Adding More Contracts](#adding-more-contracts)

[Using OpenZeppelin Contracts](#importing-openzeppelin-contracts)

## About Solidity
We won’t cover language concepts such as syntax or keywords in this guide. For that, you’ll want to check out the following curated content, which feature great learning resources for both newcomers and experienced developers:

For a general overview of how Ethereum and smart contracts work, the official website hosts a Learn about Ethereum section with lots of beginner-friendly content.

If you’re new to the language, the official Solidity documentation is a good resource to have handy. Take a look at their security recommendations, which nicely go over the differences between blockchains and traditional software platforms.

Consensys' best practices are quite extensive, and include both proven patterns to learn from and known pitfalls to avoid.

The Ethernaut web-based game will have you look for subtle vulnerabilities in smart contracts as you advance through levels of increasing difficulty.

With that out of the way, let’s get started!

## Setting up a Project
The first step after creating a project is to install a development tool.

The most popular development framework for Ethereum is Hardhat, and we cover its most common use with ethers.js. The next most popular is Truffle which uses web3.js. Each has their strengths and it is useful to be comfortable using all of them.

In these guides we will show how to develop, test and deploy smart contracts using Truffle and Hardhat.

To get started with Truffle we will install it in our project directory.

```bash
$ npm install --save-dev truffle
```
Once installed, we can initialize Truffle. This will create an empty Truffle project in our project directory.

```bash
$ npx truffle init

Starting init...
================

> Copying project files to /home/openzeppelin/learn

Init successful, sweet!
```
First contract
We store our Solidity source files (.sol) in a contracts directory. This is equivalent to the src directory you may be familiar with from other languages.

We can now write our first simple smart contract, called Box: it will let people store a value that can be later retrieved.

We will save this file as contracts/Box.sol. Each .sol file should have the code for a single contract, and be named after it.

```solidity
// contracts/Box.sol
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract Box {
    uint256 private _value;

    // Emitted when the stored value changes
    event ValueChanged(uint256 value);

    // Stores a new value in the contract
    function store(uint256 value) public {
        _value = value;
        emit ValueChanged(value);
    }

    // Reads the last stored value
    function retrieve() public view returns (uint256) {
        return _value;
    }
}
```
## Compiling Solidity
The Ethereum Virtual Machine (EVM) cannot execute Solidity code directly: we first need to compile it into EVM bytecode.

Our Box.sol contract uses Solidity 0.8 so we need to first configure Truffle to use an appropriate solc version.

We specify a Solidity 0.8 solc version in our truffle-config.js.

```javascript
// truffle-config.js

  // Configure your compilers
  compilers: {
    solc: {
      version: "0.8.4",    // Fetch exact version from solc-bin (default: truffle's version)
      // docker: true,        // Use "0.5.1" you've installed locally with docker (default: false)
      // settings: {          // See the solidity docs for advice about optimization and evmVersion
      //  optimizer: {
      //    enabled: false,
      //    runs: 200
      //  },
      //  evmVersion: "byzantium"
      // }
    }
  },
```
Compiling can then be achieved by running a single compile command:

If you’re unfamiliar with the npx command, check out our Node project setup guide.
```bash
$ npx truffle compile

Compiling your contracts...
===========================
✔ Fetching solc version list from solc-bin. Attempt #1
✔ Downloading compiler. Attempt #1.
> Compiling ./contracts/Box.sol
> Compiling ./contracts/Migrations.sol
> Artifacts written to /home/openzeppelin/learn/build/contracts
> Compiled successfully using:
   - solc: 0.8.4+commit.c7e474f2.Emscripten.clang
```
The compile command will automatically look for all contracts in the contracts directory, and compile them using the Solidity compiler using the configuration in truffle-config.js.

You will notice a build/contracts directory was created: it holds the compiled artifacts (bytecode and metadata), which are .json files. It’s a good idea to add this directory to your .gitignore.

## Adding more contracts
As your project grows, you will begin to create more contracts that interact with each other: each one should be stored in its own .sol file.

To see how this looks, let’s add a simple access control system to our Box contract: we will store an administrator address in a contract called Auth, and only let Box be used by those accounts that Auth allows.

Because the compiler will pick up all files in the contracts directory and subdirectories, you are free to organize your code as you see fit. Here, we’ll store the Auth contract in an access-control subdirectory:

```javascript
// contracts/access-control/Auth.sol
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract Auth {
    address private _administrator;

    constructor(address deployer) {
        // Make the deployer of the contract the administrator
        _administrator = deployer;
    }

    function isAdministrator(address user) public view returns (bool) {
        return user == _administrator;
    }
}
```

To use this contract from Box we use an import statement, referring to Auth by its relative path:

```javascript
// contracts/Box.sol
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

// Import Auth from the access-control subdirectory
import "./access-control/Auth.sol";

contract Box {
    uint256 private _value;
    Auth private _auth;

    event ValueChanged(uint256 value);

    constructor() {
        _auth = new Auth(msg.sender);
    }

    function store(uint256 value) public {
        // Require that the caller is registered as an administrator in Auth
        require(_auth.isAdministrator(msg.sender), "Unauthorized");

        _value = value;
        emit ValueChanged(value);
    }

    function retrieve() public view returns (uint256) {
        return _value;
    }
}
```
Separating concerns across multiple contracts is a great way to keep each one simple, and is generally a good practice.

However, this is not the only way to split your code into modules. You can also use inheritance for encapsulation and code reuse in Solidity, as we’ll see next.

Using OpenZeppelin Contracts
Reusable modules and libraries are the cornerstone of great software. OpenZeppelin Contracts contains lots of useful building blocks for smart contracts to build on. And you can rest easy when building on them: they’ve been the subject of multiple audits, with their security and correctness battle-tested.

About inheritance
Many of the contracts in the library are not standalone, that is, you’re not expected to deploy them as-is. Instead, you will use them as a starting point to build your own contracts by adding features to them. Solidity provides multiple inheritance as a mechanism to achieve this: take a look at the Solidity documentation for more details.

For example, the Ownable contract marks the deployer account as the contract’s owner, and provides a modifier called onlyOwner. When applied to a function, onlyOwner will cause all function calls that do not originate from the owner account to revert. Functions to transfer and renounce ownership are also available.

When used this way, inheritance becomes a powerful mechanism that allows for modularization, without forcing you to deploy and manage multiple contracts.

## Importing OpenZeppelin Contracts
The latest published release of the OpenZeppelin Contracts library can be downloaded by running:

```bash
$ npm install @openzeppelin/contracts
```
You should always use the library from these published releases: copy-pasting library source code into your project is a dangerous practice that makes it very easy to introduce security vulnerabilities in your contracts.
To use one of the OpenZeppelin Contracts, import it by prefixing its path with @openzeppelin/contracts. For example, in order to replace our own Auth contract, we will import @openzeppelin/contracts/access/Ownable.sol to add access control to Box:

```javascript
// contracts/Box.sol
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

// Import Ownable from the OpenZeppelin Contracts library
import "@openzeppelin/contracts/access/Ownable.sol";

// Make Box inherit from the Ownable contract
contract Box is Ownable {
    uint256 private _value;

    event ValueChanged(uint256 value);

    constuctor() Ownable(msg.sender) {}

    // The onlyOwner modifier restricts who can call the store function
    function store(uint256 value) public onlyOwner {
        _value = value;
        emit ValueChanged(value);
    }

    function retrieve() public view returns (uint256) {
        return _value;
    }
}
```
The OpenZeppelin Contracts documentation is a great place to learn about developing secure smart contract systems. It features both guides and a detailed API reference: see for example the Access Control guide to know more about the Ownable contract used in the code sample above.

Next steps
Writing and compiling Solidity contracts are but the first steps in the journey to having your decentralized application running on the Ethereum network. Once you are comfortable with this setup, you’ll want to move on to more advanced tasks:

Deploying and Interacting

Writing Automated Tests

Connecting to Public Test Networks