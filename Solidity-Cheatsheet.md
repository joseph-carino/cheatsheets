[back to overview](/../..)  

# Solidity Cheatsheet

Based on a [Udemy course](https://klarna.udemy.com/course/blockchain-developer/) and [Cryptozombies](https://cryptozombies.io/en/course/) and [solidity docs](https://docs.soliditylang.org)

## Table of Contents

- [Basics](#basics)
- - [Developing](#developing)
- - [Fungible vs Non-Fungible Tokens](#fungible-vs-non-fungible-tokens)
- - [Crypto Hashing](#crypto-hashing)
- - - [Keccak256](#keccak256)
- - [Hello World](#hello-world)
- - [Typecasting](#typecasting)
- - [Addresses](#addresses)
- - [Operators](#operators)
- [Global Units](#global-units)
- - [Monetary](#monetary)
- - [Time Units](#time-units)
- - [Address](#address)
- - [block & msg](#block-&-msg)
- [Global Properties](#global-properties)
- - [Address](#address)
- - [Encoding](#encoding)
- - [Block Properties](#block-properties)
- [State Variables](#state-variables)
- [Math operators](#math-operators)
- [Structs](#structs)
- [Arrays](#arrays)
- [Functions](#functions)
- - [Reference Types](#reference-types)
- - [Function modifiers](#function-modifiers)
- - [Multiple Return Values](#multiple-return-values)
- [Events](#events)
- [Mappings](#mappings)
- [Contracts, Inheritance & Import](#contracts-inheritance-&-import)
- - [Interacting with other contracts](#interacting-with-other-contracts)
- [Ownership](#ownershio)

## Basics

### Developing

Workspaces:

- http://remix.ethereum.org/
Interactive dev env. in the browser. Under Deploy and Run you can chose JavaScript VMs with fake accounts.

- https://www.trufflesuite.com/ganache
Development environment that runs locally on your machine

### Fungible vs Non-Fungible Tokens

#### Non-Fungible Tokens

- Are unique
- Non Divisible
- Non Interchangeable
- ERC-721 Standard
- i.e. a House

#### Fungible Tokens

- Uniform
- Divisible
- Interchangeable
- ERC-20 & ERC-777 Standard
- i.e. Gold

### Crypto Hashing

- The same input gives always the same output
- It is impossible to go back from the output to the input
- Input and output are totally uncorrelated (a simple change will create a complete different output)

#### Keccak256

Ethereum has the hash function `keccak256` built in, which is a version of SHA3. A hash function basically maps an input into a random 256-bit hexadecimal number. A slight change in the input will cause a large change in the hash.

- keccak256 expects a single parameter of type bytes. This means that we have to "pack" any parameters before calling keccak256:

```Solidity
//6e91ec6b618bb462a4a6ee5aa2cb0e9cf30f7a052bb467b0ba58b8748c00d2e5
keccak256(abi.encodePacked("aaaab"));
//b1f078126895a1424524de5321b339ab00408010b7cf0e6ed451514981e58aa9
keccak256(abi.encodePacked("aaaac"));
```

### Hello world

```Solidity
pragma solidity >=0.5.0 <0.6.0;

contract MyContract {
  string public myString = "Hello World!";
}
```

- Any public variable automatically creates a getter function internally in solidity
- Version Pragma: declares the version to use `pragma solidity >=0.5.0 <0.6.0;` using any version between 0.5.0 and 0.6.0

### Typecasting

```Solidity
uint8 a = 5;
uint b = 6;
// throws an error because a * b returns a uint, not uint8:
uint8 c = a * b;
// we have to typecast b as a uint8 to make it work:
uint8 c = a * uint8(b);
```

### Addresses

- An address is owned by a specific user (or a smart contract).
- So we can use it as a unique ID for ownership
- Every interaction with the contract has a related address

In Solidity, there are certain global variables that are available to all functions. One of these is `msg.sender`, which refers to the address of the person (or smart contract) who called the current function:

```Solidity
mapping (address => uint) favoriteNumber;

function setMyNumber(uint _myNumber) public {
  // Update our `favoriteNumber` mapping to store `_myNumber` under `msg.sender`
  favoriteNumber[msg.sender] = _myNumber;
  // ^ The syntax for storing data in a mapping is just like with arrays
}

function whatIsMyNumber() public view returns (uint) {
  // Retrieve the value stored in the sender's address
  // Will be `0` if the sender hasn't called `setMyNumber` yet
  return favoriteNumber[msg.sender];
}
```

### Operators

- Comparisons: <=, <, ==, !=, >=, > (evaluate to bool)
- Bit operaBit operators: &, |, ^ (bitwise exclusive or), ~ (bitwise negation)
- Shift operators: << (left shift), >> (right shift)
- Arithmetic operators: +, -, unary -, *, /, % (modulo), ** 
(exponentiation)

## Global Units

### Monetary

- A literal number can take a suffix of wei, finney, szabo or ether to specify a subdenomination of ether.
- 1 wei == 1
- 1 szabo == 1e12
- 1 finney == 1e15
- 1 ether == 1e18

### Time Units

- seconds, minutes, hours, days, weeks
- 1 == 1 seconds
- 1 minutes == 60 seconds
- 1 hours == 60 minutes
- 1 days == 24 hours
- 1 week == 7 days

### block & msg

[See docs](https://docs.soliditylang.org/en/develop/units-and-global-variables.html?highlight=special%20variables%20and%20functions#block-and-transaction-properties)

## Global Properties

### Address
- address: Holds a 20 byte value (size of an ethereum wallet)
- address payable: Same but you can send money to it. i.e. address payable x = address(0x123);
- <address>.balance (uint256): balance of the Address in Wei
- <address payable>.transfer(uint256 amount): send given amount of Wei to Address, reverts on failure, forwards 2300 gas stipend, not adjustable
- <address payable>.send(uint256 amount) returns (bool): send given amount of Wei to Address, returns false on failure, forwards 2300 gas stipend, not adjustable
- <address>.call(<address>.call(bytes memory) returns (bool, bytes memory): issue low-level CALL with the given payload, returns success condition and return data, forwards all available gas, adjustable
- <address>.delegatecall(bytes memory) returns (bool, bytes memory): issue low-level DELEGATECALL with the given payload, returns success condition and return data, forwards all available gas, adjustable
- <address>.staticcall(bytes memory) returns (bool, bytes memory): issue low-level STATICCALL with the given payload, returns success condition and return data, forwards all available gas, adjustable

### Encoding

- abi.decode(bytes memory encodedData, (...)) returns (...): ABI-decodes the given data, while the types are given in parentheses as second argument. Example: (uint a, uint[2] memory b, bytes memory c) = abi.decode(data, (uint, uint[2], bytes))
- abi.encode(...) returns (bytes memory): ABI-encodes the given arguments
- abi.encodePacked(...) returns (bytes memory): Performs packed encoding of the given arguments. Note that packed encoding can be ambiguous!
- abi.encodeWithSelector(bytes4 selector, ...) returns (bytes memory): ABI-encodes the given arguments starting from the second and prepends the given four-byte selector
- abi.encodeWithSignature(string memory signature, ...) returns (bytes memory): Equivalent to abi.encodeWithSelector(bytes4(keccak256

### Block Properties

- blockhash(uint blockNumber) returns (bytes32): hash of the given block - only works for 256 most recent, excluding current, blocks
- block.coinbase (address payable): current block miner’s address
- block.difficulty (uint): current block difficulty
- block.gaslimit (uint): current block gaslimit
- block.number (uint): current block number
- block.timestamp (uint): current block timestamp as seconds since unix epoch
- gasleft() returns (uint256): remaining gas
- msg.data (bytes calldata): complete calldata
- msg.sender (address payable): sender of the message (current call)
- msg.sig (bytes4): first four bytes of the calldata (i.e. function identifier)
- msg.value (uint): number of wei sent with the message
- now (uint): current block timestamp (alias for block.timestamp)
- tx.gasprice (uint): gas price of the transaction
- tx.origin (address payable): sender of the tr

### selfdestruct

- `selfdestruct(msg.sender);` destroys the smart contract and withdraws all the funds to the sender

## State Variables

```Solidity
contract Example {
  // This will be stored permanently in the blockchain
  uint myUnsignedInteger = 100;
}
```

- Variables get stored permanently on the blockchain
- Any public variable automatically creates a getter function internally in solidity
- `uint`: unsigned integer (meaning: value must me non-negative) (uint is actually an alias for uint256, a 256-bit unsigned integer. You can declare uints with less bits — uint8, uint16, uint32, etc. Useful to save space and lower costs)
- `int`: integer

## Math operators

```Solidity
uint x = 5 ** 2; // equal to 5^2 = 25
```

- Addition: x + y
- Subtraction: x - y,
- Multiplication: x * y
- Division: x / y
- Modulus / remainder: x % y (for example, 13 % 5 is 3, because if you divide 5 into 13, 3 is the remainder)
- Exponential operator: x ** y (i.e. "x to the power of y", x^y)

## Structs

```Solidity
struct Person {
  uint age;
  string name;
}
```

- Structs allow you to create more complicated data types that have multiple properties.
- Kind of like a `Class` in other languages

Usage:
```Solidity
Person[] public people;

// create a New Person:
Person satoshi = Person(172, "Satoshi");

// Add that person to the Array:
people.push(satoshi);

// or like this:
people.push(Person(16, "Vitalik"));
```

## Arrays

```Solidity
// Array with a fixed length of 2 elements:
uint[2] fixedArray;
// another fixed Array, can contain 5 strings:
string[5] stringArray;
// a dynamic Array - has no fixed size, can keep growing:
uint[] dynamicArray;
```

## Functions

```Solidity
function eatHamburgers(string memory _name, uint _amount) private {

}
```

- `private`: the function can only be called from within the contract. Functions are **public by default**, meaning they can be called from anywhere in the network.
- `string memory _name`: store the _name variable in memory. This is required for all reference types such as arrays, structs, mappings, and strings. This indicates to store by reference not by value (see below)

### Reference Types

There are two ways in which you can pass an argument to a Solidity function:

- By value: creates a new copy of the parameter's value and passes it to your function. This allows your function to modify the value without worrying that the value of the initial parameter gets changed.
- By reference: function called with reference to the original variable. If your function changes the value of the variable it receives, the value of the original variable gets changed.

Storage and Memory:
- State variables (variables declared outside of functions) are by default `storage` and written permanently to the blockchain, while variables declared inside functions are `memory` and will disappear when the function call ends.

Example:
```Solidity
contract SandwichFactory {
  struct Sandwich {
    string name;
    string status;
  }

  Sandwich[] sandwiches;

  function eatSandwich(uint _index) public {
    // Sandwich mySandwich = sandwiches[_index];

    // ^ Seems pretty straightforward, but solidity will give you a warning
    // telling you that you should explicitly declare `storage` or `memory` here.

    // So instead, you should declare with the `storage` keyword, like:
    Sandwich storage mySandwich = sandwiches[_index];
    // ...in which case `mySandwich` is a pointer to `sandwiches[_index]`
    // in storage, and...
    mySandwich.status = "Eaten!";
    // ...this will permanently change `sandwiches[_index]` on the blockchain.

    // If you just want a copy, you can use `memory`:
    Sandwich memory anotherSandwich = sandwiches[_index + 1];
    // ...in which case `anotherSandwich` will simply be a copy of the 
    // data in memory, and...
    anotherSandwich.status = "Eaten!";
    // ...will just modify the temporary variable and have no effect 
    // on `sandwiches[_index + 1]`. But you can do this:
    sandwiches[_index + 1] = anotherSandwich;
    // ...if you want to copy the changes back into blockchain storage.
  }
}
```

### Function modifiers

```Solidity
function sayHello() public view returns (string memory) {}
```

- `view`: meaning it's only viewing the data but not modifying it.

```Solidity
function _multiply(uint a, uint b) private pure returns (uint) {
  return a * b;
}
```

- `pure`: you're not even accessing any data in the app.
- `returns`: marks what the function will return

```Solidity
function eat() internal {
  sandwichesEaten++;
}
```

- `internal`: same as private, except that it's also accessible to contracts that inherit from this contract.
- `external`: similar to public, except that these functions can ONLY be called outside the contract.

```Solidity
function eat() payable {
  
}
```

- `payable`: makes it possible to send money to a function

### Multiple Return Values

```Solidity
function multipleReturns() internal returns(uint a, uint b, uint c) {
  return (1, 2, 3);
}

function processMultipleReturns() external {
  uint a;
  uint b;
  uint c;
  // This is how you do multiple assignment:
  (a, b, c) = multipleReturns();
}

// Or if we only cared about one of the values:
function getLastReturnValue() external {
  uint c;
  // We can just leave the other fields blank:
  (,,c) = multipleReturns();
}
```

## Events

```Solidity
// declare the event
event IntegersAdded(uint x, uint y, uint result);

function add(uint _x, uint _y) public returns (uint) {
  uint result = _x + _y;
  // fire an event to let the app know the function was called:
  emit IntegersAdded(_x, _y, result);
  return result;
}
```

Your app front-end could then listen for the event. A JavaScript implementation would look something like:

```JavaScript
YourContract.IntegersAdded(function(error, result) {
  // do something with result
})
```

## Mappings

```Solidity
// For a financial app, storing a uint that holds the user's account balance:
mapping (address => uint) public accountBalance;
// Or could be used to store / lookup usernames based on userId
mapping (uint => string) userIdToName;
```
```Solidity
mapping (address => uint) favoriteNumber;

function setMyNumber(uint _myNumber) public {
  // Update our `favoriteNumber` mapping to store `_myNumber` under `msg.sender`
  favoriteNumber[msg.sender] = _myNumber;
  // ^ The syntax for storing data in a mapping is just like with arrays
}

function whatIsMyNumber() public view returns (uint) {
  // Retrieve the value stored in the sender's address
  // Will be `0` if the sender hasn't called `setMyNumber` yet
  return favoriteNumber[msg.sender];
}
```

## Contracts, Inheritance & Import

```Solidity
contract Doge {
  function catchphrase() public returns (string memory) {
    return "So Wow CryptoDoge";
  }
}

contract BabyDoge is Doge {
  function anotherCatchphrase() public returns (string memory) {
    return "Such Moon BabyDoge";
  }
}
```

- BabyDoge inherits from Doge

This can also be in another file:

```Solidity
import "./someothercontract.sol";

contract newContract is SomeOtherContract {}
```

### Constructor

```Solidity
contract Doge {
  constructor() {}
}
```

The constructor is called only once during contract deployment

### Interacting with other contracts

To interact with other contracts we need to define an `interface`.

Let’s say there is this external contract:

```Solidity
contract LuckyNumber {
  mapping(address => uint) numbers;

  function setNum(uint _num) public {
    numbers[msg.sender] = _num;
  }

  function getNum(address _myAddress) public view returns (uint) {
    return numbers[_myAddress];
  }
}
```

You’d create this kind of interface:
```Solidity
contract NumberInterface {
  function getNum(address _myAddress) public view returns (uint);
}
```

You can now use the interface like this:
```Solidity
contract NumberInterface {
  function getNum(address _myAddress) public view returns (uint);
}

contract MyContract {
  address NumberInterfaceAddress = 0xab38... 
  // ^ The address of the FavoriteNumber contract on Ethereum
  NumberInterface numberContract = NumberInterface(NumberInterfaceAddress);
  // Now `numberContract` is pointing to the other contract

  function someFunction() public {
    // Now we can call `getNum` from that contract:
    uint num = numberContract.getNum(msg.sender);
    // ...and do something with `num` here
  }
}
```

In this way, your contract can interact with any other contract on the Ethereum blockchain, as long they expose those functions as public or external.

## Ownership

All smart contracts are publicly available on the blockchain (note: everything is, also the information in the variables within the smart contract, even the private ones). You'd have to use your own network if you want privacy or external resources. There is no concept of ownership. However we can use a little trick:

```Solidity
contract OwnershipExample {
  address owner;

  constructor() public {
    owner = msg.sender;
  }

  function ownerDoSomething() {
    require(msg.sender === owner, "You are not the owner");
    // do something as the owner
    // i.e. destroying the smart contract :O
    selfdestruct(msg.sender);
  }
}
```

- `Constructor` is only called once when deploying. `msg.sender` is the address that is creating the action to deploy the smart contract.
- `require` will roll back whatever happened before (the transaction) if the first argument is false. The second message is the error message.
- `selfdestruct(msg.sender);` destroys the smart contract and withdraws all the funds to the sender. It will not delete the contract, just "stop" it. Because that is impossible (since you'd have to undo the transaction).
