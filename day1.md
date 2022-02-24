#Baby Steps
I am practicing solidity.  It reminds me a lot of the C language.  I have been coding in Swift so I need to get used to the following structure of elements.

## Variables
### type visibility variableName = value;

## Functions
### function name(type variable1, type variable2) visibility returns (type)

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
