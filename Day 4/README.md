# 30_Days_Of_Solidity

We have different ways of classifying functions in Solidity

## By function visibility:

![Screenshot 2023-01-07 at 13 17 16](https://user-images.githubusercontent.com/29931071/211153356-d6a33611-6efe-44a1-b7e1-9bfe0c7b93d2.png)

public : visible everywhere (within the contract itself and other contracts or addresses).

private : visible only by the contract it is defined in, not derived contracts.

internal : visible by the contract itself and contracts deriving from it.

external : visible only by external contracts / addresses.


Note: Like public functions, external functions are part of the contract interface (ABI).


```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract Function {
    // Functions can return multiple values.
    function returnMany() public pure returns (uint, bool, uint) {
        return (1, true, 2);
    }

    // Return values can be named.
    function named() public pure returns (uint x, bool b, uint y) {
        return (1, true, 2);
    }

    // Return values can be assigned to their name.
    // In this case the return statement can be omitted.
    function assigned() public pure returns (uint x, bool b, uint y) {
        x = 1;
        b = true;
        y = 2;
    }

    // Use destructuring assignment when calling another
    // function that returns multiple values.
    function destructuringAssignments()
        public
        pure
        returns (uint, bool, uint, uint, uint)
    {
        (uint i, bool b, uint j) = returnMany();

        // Values can be left out.
        (uint x, , uint y) = (4, 5, 6);

        return (i, b, j, x, y);
    }

    // Cannot use map for either input or output

    // Can use array for input
    function arrayInput(uint[] memory _arr) public {}

    // Can use array for output
    uint[] public arr;

    function arrayOutput() public view returns (uint[] memory) {
        return arr;
    }
}

// Call function with key-value inputs
contract XYZ {
    function someFuncWithManyInputs(
        uint x,
        uint y,
        uint z,
        address a,
        bool b,
        string memory c
    ) public pure returns (uint) {}

    function callFunc() external pure returns (uint) {
        return someFuncWithManyInputs(1, 2, 3, address(0), true, "c");
    }

    function callFuncWithKeyValue() external pure returns (uint) {
        return
            someFuncWithManyInputs({a: address(0), b: true, c: "c", x: 1, y: 2, z: 3});
    }
}
```



## View vs Pure 

- View functions can read contract’s storage, but can’t modify the contract storage. Therefore, they are used for getters.

- Pure functions can neither read, nor modify the contract’s storage. They are used for pure computation, like functions that perform mathematic or cryptographic operations.

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract ViewAndPure {
    uint public x = 1;

    // Promise not to modify the state.
    function addToX(uint y) public view returns (uint) {
        return x + y;
    }

    // Promise not to modify or read from the state.
    function add(uint i, uint j) public pure returns (uint) {
        return i + j;
    }
}
```


## Payable function

A payable function is a function that can receive ether while being called. It is mandatory to include the payable keyword (from Solidity 0.4.x) if you wish your function to receive ethers. If you try to send ethers to a function that is not mentioned as payable, the transaction will be rejected and fail.


```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract Payable {
    // Payable address can receive Ether
    address payable public owner;

    // Payable constructor can receive Ether
    constructor() payable {
        owner = payable(msg.sender);
    }

    // Function to deposit Ether into this contract.
    // Call this function along with some Ether.
    // The balance of this contract will be automatically updated.
    function deposit() public payable {}

    // Call this function along with some Ether.
    // The function will throw an error since this function is not payable.
    function notPayable() public {}

    // Function to withdraw all Ether from this contract.
    function withdraw() public {
        // get the amount of Ether stored in this contract
        uint amount = address(this).balance;

        // send all Ether to owner
        // Owner can receive Ether since the address of owner is payable
        (bool success, ) = owner.call{value: amount}("");
        require(success, "Failed to send Ether");
    }

    // Function to transfer Ether from this contract to address from input
    function transfer(address payable _to, uint _amount) public {
        // Note that "to" is declared as payable
        (bool success, ) = _to.call{value: _amount}("");
        require(success, "Failed to send Ether");
    }
}
```



## Fall back function

A fallback function has 2 main purposes on Ethereum:

- Coin management = accept and allocate coins in a meaningful manner.

- Error-handling = if a user calls a function in a contract that does not exists, the fallback function will be executed.

Therefore, the fallback function can implement some code to gracefully handle the situation.


```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract Fallback {
    event Log(string func, uint gas);

    // Fallback function must be declared as external.
    fallback() external payable {
        // send / transfer (forwards 2300 gas to this fallback function)
        // call (forwards all of the gas)
        emit Log("fallback", gasleft());
    }

    // Receive is a variant of fallback that is triggered when msg.data is empty
    receive() external payable {
        emit Log("receive", gasleft());
    }

    // Helper function to check the balance of this contract
    function getBalance() public view returns (uint) {
        return address(this).balance;
    }
}

contract SendToFallback {
    function transferToFallback(address payable _to) public payable {
        _to.transfer(msg.value);
    }

    function callFallback(address payable _to) public payable {
        (bool sent, ) = _to.call{value: msg.value}("");
        require(sent, "Failed to send Ether");
    }
}
```

## Receive function

The receive function is also a breaking change since Solidity 0.6.0. Like the new fallback function we have seen above, you declare it as follow:

```
pragma solidity >0.6.0;
contract Example {
    
    fallback() external {
        // ...
    }
    
    receive() external payable {
        // ...
    }
}
```


In the new ```receive()``` function, the payable keyword is mandatory (As opposed to the new ```fallback()``` function, which can be specified payable or not).

If present, the ```receive()``` ether function is called whenever the call data is empty (whether or not ether is received).




