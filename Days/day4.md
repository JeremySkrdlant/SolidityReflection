# Deploying Contracts with javascript
I took a while off but finally came back to practicing some solidity.  Up until this point I have been using the remix ide in the browser.  In this session I started using Ganache along with Node.js to deploy and look at a contract.

## Tools Used
* Ganache
* Node.js
* Visual Studio Code (Need to see if Atom compares with the autocomplete)
* dotenv - environment variable file reader
* ethers.js - wrapper that makes working with contracts easier
* solc - solidity compiler

## Making a local blockchain
Ganache is a useful tool in creating a test network with multiple wallets that each have 100 eth in each of them.  You can quickly see each wallet along with their private keys and any transactions that have occured.  It also shows at the top the network id that you can connect to.  

## compiling your solidity file
You will want to compile your solidity file into a binary and an abi (application binary interface).  The the keyword in abi is interface.  It lets you know which functions you can call within the binary without having access to the code that made it.  Once you have the contract, you can access any of these functions using dot notation.

```
  solcjs --bin --abi --include-path node_modules --base-path . -o . SimpleStorage.sol
```

The command above can be put in a script in your package.json.  It is similar to making a c file.
** --bin ** this parameter specifies to make a binary.
** --abi ** this parameter specifies to make the abi file.
** --include-path node_modules ** will compile any required contracts in the node modules directory.
** --base-path . ** specifies our solidity file is in this directory
** -o . ** specifies to output the files to this directory.
** SimpleStorage.sol ** is the solidity file to compile.

Once you run this file, you will see the two files appear that you will be using later on.

## Required Imports
```js
// wrapper for reading solidity contracts.
const ethers = require("ethers");
// Standard File System tool for reading the abi and the bin
const fs = require("fs-extra");
// Tool to read in the .env file into the process.env variable.
require("dotenv").config();
```

## Async Function
Our function that calls the contract needs to be asyncronous.  We need to await for the blockchain to write the data before we can have access to the contract.

```js
async function main() { ... }

main()
  .then(() => process.exit(0))
  .catch((error) => {
    console.log(error);
    process.exit(1);
  });
```

When we call our asyncronous function, we use promises to catch any errors.  Our .then simply exits with the 0 which means completed with no issues.

Our .catch will print our error and then exit with 1 which will indicate that an error occured.

## .env file
We will need to set up a couple of variables that will not be pushed to the git repository.  Make sure your .env is in your gitignore file.

** Private Key ** this is of the wallet that will be  paying the gas fees for deploying the contract to the network.

** RPC URL ** this is the location of our local test blockchain.

## Deploying a contract

```js
//Creates a variable to access our blockchain provider using the RPC URL
const provider = new ethers.providers.JsonRpcProvider(process.env.RPC_URL);
//Creates a wallet to use to pay for the gas fees.
const wallet = new ethers.Wallet(process.env.PRIVATE_KEY, provider);
//Reads in our two files we compiled with solc
const abi = fs.readFileSync("./SimpleStorage_sol_SimpleStorage.abi", "utf8");
const binary = fs.readFileSync("./SimpleStorage_sol_SimpleStorage.bin", "utf8");
//Creates a factory that can make contracts and deploy them to the chain.
const contractFactory = new ethers.ContractFactory(abi, binary, wallet);
//Uses the factory to deploy the contract to the network.
//We can use this contract to call any functions in the ABI.
const contract = await contractFactory.deploy();
//This gets a reciept that the contract was deployed.
//This gives us more information about the deployment.
//The code will work without this reciept, need to look more into if
//it truely is required.  
const transactionReciept = await contract.deployTransaction.wait(1);
```
At this point, the contract is deployed to the blockchain network ready for frontend applications to access it.  You can also test out some of the functions using contract.functionName().  This will be in the virtual machine so metamask will not popup at this moment.  All gas fees will be paid using the wallet that is linked to the contract in the line that has the contract factory.  

# Notes
Make sure you include "utf8" in the file reading.  This makes for an odd out of gas error that will have you hunting for a while trying to figure out what is going on.  

The ABI makes a lot more sense to me now that I have compiled one and see that is is similar to JSON and is basically like an API or an Protocol in Swift or an Interface in many other languages.  
