# I Want Cookies!!!
In class we were working with observable variables in Angular using the ngModel.  We also implemented the old cookie clicker game that students way back in the day used to play.  It still makes no sense to me but I do know the chocolate milk is important [More Info](https://www.wikihow.com/Quickly-Get-Chocolate-Milk-in-Cookie-Clicker)

## The Challenge  
My goal was to create a function that was passed in a string and if it was chocolate milk, it would increment an unsigned int by 500.  Anything else would increment only by 1.  This would be similar to a promotional code you see on many sites to get a discount.

### My Assumption
This is a simple if-then statement and I probably should plan more work for day 2.  It will be good because my day 2 is a week later and I do need to still get my feet wet in the basic syntax.

### The Solution
After a lot of exploring, I got it to work with the help of stack overflow.  Now I am going back in and deconstructing what each piece does.

```solidity
// SPDX-License-Identifier: MIT

pragma solidity ^0.6.0;

contract CookieCounter {

    uint256 public cookieCount = 0;

    function addCookieTap(string calldata mode) public {

        if (keccak256(abi.encode(mode)) == keccak256(abi.encode("Chocolate Milk"))){
            cookieCount += 500;
        } else {
            cookieCount += 1;
        }

    }

}
```

### Soooo...
This is way more complicated than other languages and when you read the comments, most people are saying (Why would you compare strings here?) This brings up a great question.  Solidity is about blockchain contracts and any special offer should not be here.  Anyone can look at the contract and see your discount code!!

#### Mindset
I need a change my mindset.  I have to really focus on realizing that the code is completely out in the open. I have warned students in the past about putting passwords in the frontend javascript file because anybody can look at the code.  When you are building an iOS app, the user starts at your interface and is only allowed to see what you want them to see. The same is usually true with backend code as well.  The user only sees what the API allows them to see.  When you are coding a smart contract, the first thing the user is going to do is look at all your code.  This is interesting because I think of Smart Contracts as backend development yet the code is completely visible.

#### How it Works in General
To start, let's look at the solution in swift.
```swift
var cookieCount = 0

func addCookieTap(mode:String){
    if mode == "Chocolate Milk"{
        cookieCount += 500
    }else
    {
        cookieCount += 1
    }
}
```

This solution is pretty straightforward and simple.  Pass in a string, check it's value, update based on if it is Chocolate Milk or not.

The Solidity solution is just one way to achieve the result. The ingeniously use compare hash values from the binary version of the string.  To explain, I will go through the phases of error after errror to finally get to this solution and what I learned along the way.

### Phase 1
I typed in this code thinking there might be some spelling errors or a semicolon missing but it should work.  
```solidity
contract CookieCounter {
    uint256 public cookieCount = 0;

    function addCookieTap(string  mode) public {

        if (mode == "Chocolate Milk"){
            cookieCount += 500;
        }else {
            cookieCount += 1;
        }
    }
}
```
This results in the error for the mode parameter.
```
TypeError: Data location must be "memory" or "calldata" for parameter in function, but none was given.
    function addCookieTap(string  mode) public {
                          ^----------^
```

I needed to learn more about memory.  
The first site I looked at compared memory to storage. (note that storage persists like a hard drive and memory disappears like ram.)
[memory vs. storage](https://medium.com/coinmonks/solidity-bits-storage-vs-memory-a54a650ea4ff)

I then looked up calldata and got some interesting information.  Note that memory is mutable while calldata is immutable.  
[memory vs. calldata](https://ethereum.stackexchange.com/questions/74442/when-should-i-use-calldata-and-when-should-i-use-memory)

### Phase 2
I chose to go with calldata, The one error is fixed so everything is good and I can enjoy a cup of coffee in my success.  Oh wait, another bug.  

```solidity
contract CookieCounter {
    uint256 public cookieCount = 0;

    function addCookieTap(string memory mode) public {

        if (mode == "Chocolate Milk"){
            cookieCount += 500;
        }else {
            cookieCount += 1;
        }
    }
}
```
This gives an error at the if statement.

```
TypeError: Operator == not compatible with types string calldata and literal_string "Chocolate Milk"
        if (mode == "Chocolate Milk"){
            ^----------------------^
```

I also tried changing Chocolate Milk to a calldata and got the following error.

```
contracts/take2sol.sol:10:13: TypeError: Operator == not compatible with types string calldata and string calldata
        if (mode == test){
            ^----------^
```

I also tried changing my Solidity version to 0.8 and up and that still didn't work.  So after some searching, the solution seemed to be convert the strings to binary and then convert them to a hash and compare the two hashes.

Here are the components.

### abi.encode
ABI is the Application Binary Interface.  We are using it to encode the string back into bytes of data.

[abi documentation](https://docs.soliditylang.org/en/v0.5.3/abi-spec.html)

```solidity
contract CookieCounter {
    uint256 public cookieCount = 0;

    function addCookieTap(string calldata mode) public {
        if (abi.encode(mode) == abi.encode("Chocolate Milk")){
            cookieCount += 500;
        }else {
            cookieCount += 1;
        }
    }

}
```

Now we get the error
```
TypeError: Operator == not compatible with types bytes memory and bytes memory
 --> contracts/take2sol.sol:9:13:
  |
9 |         if (abi.encode(mode) == abi.encode("Chocolate Milk")){
  |             ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
```

Notice the the bytes memory and bytes memory are the same but not comparable.  

### keccak256
This is solidities hashing function.  It can take bytes of data and create a hash from it.  Unfortunately it can not take a string so you need to use the abi.encode to convert it first.

[a few options to try](https://www.tutorialspoint.com/solidity/solidity_cryptographic_functions.htm)

```solidity
contract CookieCounter {
    uint256 public cookieCount = 0;

    function addCookieTap(string calldata mode) public {
        if (keccak256(abi.encode(mode)) == keccak256(abi.encode("Chocolate Milk"))){
            cookieCount += 500;
        }else {
            cookieCount += 1;
        }
    }

}
```

## YouTube Review
[YouTube Video Showing the Project](https://youtu.be/ghai0PpkY3k)
