# Functions
I had a little time to code some solidity and I noticed that a book I was using used pure. Most of the tutorials had view before the function return type.

## pure vs view
**View** is when you look at the blockchain but you do not change the state. This doesn't require any mining so it is free because no transaction is necessary to take advantage of it.

**Pure** is when you are not even looking at the blockchain.  You are just simply doing a calculation.  

**Constant** is deprecated since the community didn't think that it described what it did well.  It is the same as view but seemed misleading.  View and pure let you know for sure that the function is looking at the block chain (view) or that the function is minding it's own business (pure).  

I did some research and I am still trying to figure out the benefit of pure. There is the concept of letting individuals reading the contract know that no data is being read.  From the code stance, reading the blockchain has no cost to my knowledge and it does not mutate the state at all.  

## return vs tuple
You can specify a return or you can use a tuple.  The tuple requires no return word and it takes the values of the variables that they last were before the closing bracket. This is neat but completely new to me.

```solidity
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

contract LetsTuple{

    function sample(uint8 original) public pure returns (uint8 doubled, uint8 tripled, uint8 quadrupled){
        doubled = original * 2 ;
        tripled = original * 3 ;
        quadrupled = original * 4;
    }
}
```
The code above is a simple function that takes an unsigned 8 bit integer and it returns a tuple with 3 values in it.

## Visibility Levels
* public - anyone can access it.
* external - only other contracts can call this.
* internal - only this contract can access it. (derived contracts have access)
* private - only this contract and no derived contracts.

## Inheritance (derived)
Solidity uses the is key to inherit from a parent.

```solidity
contract AdvancedTuple is LetsTuple{

}
```

AdvancedTuple gets all the functions from LetsTuple as long as they are not private.

## Best Practice
It was mentioned in a conference video that you should always focus on writing the least amount to the chain as possible.  Using a 8 bit vs a 32 bit can have a big effect on the cost.  Blockchains are definitely trying to save as much space like we used to do in the old days when ram was limited.

[Article on Saving Gas](https://medium.com/coinmonks/gas-optimization-in-solidity-part-i-variables-9d5775e43dde#:~:text=Solidity%20contracts%20have%20contiguous%2032,it%20is%20called%20variable%20packing.)


[Back to Index](https://github.com/JeremySkrdlant/SolidityReflection)
