## Setting up a Node project
New software industries often start out with every project sharing the same technology stack. The Ethereum ecosystem is no exception, and the language of choice is JavaScript. All of the most used Ethereum libraries, including OpenZeppelin software, are written in JavaScript or one of its variants.

JavaScript code is traditionally run on a web browser as part of a website, but it can be also executed as a standalone process using Node.

This guide will help you get your Node development environment set up, which you’ll need to use the different OpenZeppelin tools and other third party products.

If you are already familiar with Node, npm and Git, feel free to skip this guide!
Installing Node
There are multiple ways to get Node on your machine: you can get it either via a package manager or by downloading the installer directly.

If you are running Windows consider using Windows Subsystem for Linux as much of the ecosystem is written for Linux.
Once you’re done, run node --version on a terminal to check your installation: any version of the 14.x or 16.x line should be compatible with most Ethereum software.

```bash
$ node --version
v16.17.1
```

Creating a project
JavaScript software is often bundled in packages, which are distributed via the npm registry. A package is simply a directory that contains a file called package.json, describing the package’s name, version, content, and others. When you build your own project, you will be creating a package, even if you don’t plan to distribute it.

All Node installations include a command-line client for the npm registry, which you’ll use while developing your own projects. To start a new project, create a directory for it:

```bash
$ mkdir learn && cd learn
Then we can initialize it:
```

```bash
$ npm init -y
```

Simple as that! Your newly created package.json file will evolve as your project grows, such as when installing dependencies with npm install.

JavaScript and npm are some of the most used software tools in the world: if you’re ever in doubt, you’ll find plenty of information about them online.
Using npx
There are two broads type of packages stored in the npm registry: libraries and executables. Installed libraries are used like any other piece of JavaScript code, but executables are special.

A third binary was included when installing node: npx. This is used to run executables installed locally in your project.

Whilst Truffle and Hardhat can be installed globally we recommend installing locally in each project so that you can control the version on a project by project basis.

For clarity we’ll display the full command in our guides including npx so we don’t get errors due to the binary not being in the system path:

```bash
$ truffle init
truffle: command not found
$ npx truffle init
- Fetching solc version list from solc-bin. Attempt #1

Starting init...
================

> Copying project files to ...

Init successful, sweet!
```

Make sure you are inside your project’s directory when running npx! Otherwise, it will download the full executable again just to run that command, which most of the time is not what you want.
Tracking with Version Control
Before you get coding, you should add version control software to your project to track changes.

By far, the most used tool is Git, often in conjunction with GitHub for hosting purposes. Indeed, you will find the full source code and history of all OpenZeppelin software in our GitHub repository.

If you’ve never used Git before, a good starting place is the Git Handbook.
Don’t commit secrets such as mnemonics, private keys and API keys to version control! Make sure you .gitignore files with secrets.
