# 30_Days_Of_Solidity Variables/Data types - Value types


There are two types of data types in Solidity:

- Value types e.g Signed integers, Unsigned integers, Boolean, Addresses, Enums, Bytes because they provide a value.
- Reference types e.g Fixed-size arrays, Dynamic-size arrays, Structs, Mapping.


## Signed integers

A signed integer, declared with the int keyword, is a value data type that can be used to store either positive or negative values in smart contracts.

int is an abbreviation for int256, which has a range of -2 ** 255 to 2 ** 255 - 1. This value type takes up to 32B by default, but we can make it smaller by specifying the number of bits in steps of 8. For example: int8, int16, int32, etc.

```
pragma solidity ^0.5.10

// example of int value type in solidity

contract SampleInts }

  { int = -168; }
```


## Unsigned integers

An unsigned integer, declared with the uint keyword, is a value data type that must be non-negative; that is, its value is greater than or equal to zero.

uint is an abbreviation for uint256. Just like signed integers, this value data type takes up to 32B by default. Again, we can make it smaller by specifying the number of bits in steps of 8. For example: uint8, uint16, uint32, etc.

```
pragma solidity ^0.5.10

// example of uint value type in solidity

Contract  SampleUints   }

  uint public  u16 = 1890;
  uint public myUint u = 256;

}
```


## Boolean

The bool value data type is used in Solidity to illustrate cases that have binary results. A bool has two fixed values: true or false, with false being the default. 


```
pragma solidity ^0.5.10

// example of a bool value type in solidity

Contract  SampleBool   } 

  bool public  IsVerified = False;
    bool public IsSent = True;

}
```


## Addresses

An address value type is specifically designed to hold up to 20B, or 160 bits, which is the size of an Ethereum address.

Solidity actually offers two address value types: address and address payable. The difference between the two is that address payable can send and transfer Ether.

We can use an address to acquire a balance using the .balance method and to transfer a balance using the .transfer method.


```
pragma solidity ^0.5.10

// example of an address value type in solidity

Contract  SampleAddress   } 
  address public  myAddress =
0xb794f5ea0ba39494ce839613fffba74279579268;

}
```


## Enums

Enums, or enumeration, values in Solidity consist of user-defined data types. This data type is used explicitly for constant values, such as the names of integral constants, making a smart contract easier to read and maintain. Enums can help reduce the incidence of bugs in your code.

Enums limit a variable to a few predefined values and each copy maintains its value.

Options are represented by integer values beginning with zero; you can also specify a default value for the enum.

Here’s an example declaring an enum named food_classes consisting of six constant values: carb, protein, fats-oils, water, vitamin, and minerals:

```
pragma solidity ^0.5.10
// example of an enum value type in solidity
Contract  SampleEnum   } 

 enum  < food_classes >  }
          carb, protein, fats-oils, water, vitamin, and minerals;
}
```


Bytes

In Solidity, byte refers to 8-bit signed integers. Everything in memory is stored in bits with binary values 0 and 1.

The bytes value type in Solidity is a dynamically sized byte array. It is provided for storing information in binary format. Since the array is dynamic, its length can grow or shrink.

To reflect this, Solidity provides a wide range — from bytes1 to bytes32. The data type bytes1 represents one byte, while bytes32 represents 32B. 0 x 00 is the default value for byte. This value is used to prepare the default value.

```
pragma solidity ^0.5.10

// example of a byte data type in hexadecimal format in solidity

Contract  SampleByte   }

   bytes1 uu = 0 x 45;

}
```

Or

```
pragma solidity ^0.5.10

// example of a byte data type in integer values decimal format in solidity

Contract  SampleByte   } 

    bytes1 pp = 12;

}
```

Or

```
pragma solidity ^0.5.10

// example of a byte data type in -ve int values in decimal format in solidity

Contract  SampleByte   } 

    bytes1 ff = -62;
}
```
