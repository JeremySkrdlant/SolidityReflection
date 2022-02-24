# Day 1 - Baby Steps (learning variables again)
I am getting spun up in the Solidity coding language.  It reminds me a lot of the C language so it feels like relearning a familiar language. There are small habits like not having semicolons but overall it seems like a good language. RemixIDE is a wonderful tool for practicing smart contract coding.

I think the biggest thing I am trying to wrap my mind around is how users are going to be paying gas fees for functions.  It makes sense since building blocks in a blockchain require money for the mining but it is an interesting thing to think about.  It would be strange to think that every function call would cost money if it changes the structure of the data when you are used to object oriented languages that change the structure all the time.   

## Variables
### Structure
#### type visibility variableName = value;

### Types
I went through a few of the types and noticed that there is a lot that can be taught while I teach students variables in Swift.  The biggest shock was that decimal math is really rough from my understanding.  The documentation seams to note that decimal numbers are not formatted and using items like fixed and ufixed do not seem to work in the RemixIDE.

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
Functions cost gas fees unless they are marked view while variables are read with no charge.

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

## RemixIDE
[Overview of using the RemixIDE on Day 1](https://www.youtube.com/watch?v=B0BDi7J5DTI)

## Notes
The structure is usually the licensing, the pragma to let you know which version of Solidity you are using and the contract is similar to a class.

[Back to Index](https://github.com/JeremySkrdlant/SolidityReflection)
