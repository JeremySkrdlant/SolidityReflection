# Baby Steps
I am practicing solidity.  It reminds me a lot of the C language. A lot of it is familiar but it does feel like a brand new language.  Swift has gotten me in the habit of not having semicolons but the RemixIDE is a wonderful tool for practicing smart contract coding.

## Variables
### Structure
#### type visibility variableName = value;

### Types
I went through a few of the types and noticed that there is a lot that can be taught while I teach students variables in swift.  The biggest shock was that decimal math is really rough from my understanding.  The documentation seams to note that decimal numbers are not formatted and using items like fixed and ufixed do not seem to work in the RemixIDE.

* int / uint 8-256
* bool
* exponents 2**2 not ^
* string (not technically a type, an array of characters)
* address - wallet address
* bytes

[Solidity Docs on Types](https://docs.soliditylang.org/en/v0.8.11/types.html)

## Functions
### Structure
#### function name(type variable1, type variable2) visibility returns (type)

### Info
Functions cost gas fees while variables are read with no charge.

## Sample Code of Super Simple Project
```solidity
// SPDX-License-Identifier: MIT

pragma solidity >=0.6.0 <0.9.0;

contract VariableTrial{

    uint256 public readableInt = 200;

    function addToFunds(uint256 amount) public returns (uint256){
        readableInt += amount;
        return readableInt;
    }

}
```
