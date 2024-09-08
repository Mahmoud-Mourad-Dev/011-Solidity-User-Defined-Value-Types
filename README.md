# 011-Solidity-User-Defined-Value-Types
A user-defined value type is defined using type C is V, where C is the name of the newly introduced type and V has to be a built-in value type
```solidity
//SPDX-License-Identifier:MIT
pragma solidity ^0.8.24;
type wieght is uint256;
contract userDefindType{
    wieght public _wieght;
    
}
```
The function C.wrap is used to convert from the underlying type to the custom type. Similarly, the function C.unwrap is used to convert from the custom type to the underlying type.
```solidity

//SPDX-License-Identifier:MIT
pragma solidity ^0.8.24;
type wieght is uint256;
contract userDefindType{
     wieght public _wieght;
    constructor(uint256 startWieght){
        _wieght = wieght.wrap(startWieght);
    }

    function getWieght() public view returns(uint256){
        return wieght.unwrap(_wieght);
    }
    
    }
```

Set a new weight using `wrap`
```solidity
//SPDX-License-Identifier:MIT
pragma solidity ^0.8.24;
type weight is uint256;
contract userDefindType{
     weight public _weight;
    constructor(uint256 startWeight){
        _weight = weight.wrap(startWeight);
    }

    function getWeight() public view returns(uint256){
        return weight.unwrap(_weight);
    }
    function changeWeight(uint256 newWeight) public  {
        _weight = weight.wrap(newWeight);
    }    
    }
```
Explanation:
type Weight is uint256;: This defines a new type Weight that behaves like uint256, but it is a distinct type and cannot be assigned to a uint256 variable without conversion.
wrap and unwrap:
Weight.wrap() is used to convert a uint256 to the Weight type.
Weight.unwrap() is used to convert a Weight type back to a uint256.
The type C does not have any operators or attached member functions. In particular, even the operator == is not defined. Explicit and implicit conversions to and from other types are disallowed.

Example Use Case:
Consider a scenario where you're working with different units of measurement like Weight, Length, and Time. You can define user-defined value types to ensure these units are never accidentally mixed up:
```solidity
type Length is uint256;
type Time is uint256;

contract Measurement {
    Length public length;
    Time public time;

    function setLength(uint256 _length) public {
        length = Length.wrap(_length); // Safe assignment of Length
    }

    function setTime(uint256 _time) public {
        time = Time.wrap(_time); // Safe assignment of Time
    }

    // This will throw an error if you try to assign Length to Time or vice versa
    // time = length; // ERROR
}
```
Example: Currency Types
```solidity

//SPDX-License-Identifier:MIT
pragma solidity ^0.8.24;
// Define user-defined value types for different currencies
type USD is uint256;
type EUR is uint256;

contract CurrencyConverter {
    // Exchange rate (just a fixed example rate, 1 USD = 0.85 EUR)
    uint256 constant usdToEurRate = 85;

    // Function to convert USD to EUR
    function convertUSDtoEUR(USD amountInUSD) public pure returns (EUR) {
        uint256 usdAmount = USD.unwrap(amountInUSD); // Unwrap the USD type to get the uint256 value
        uint256 eurAmount = (usdAmount * usdToEurRate) / 100; // Simple conversion
        return EUR.wrap(eurAmount); // Wrap the result in the EUR type
    }

    // Function to convert EUR to USD
    function convertEURtoUSD(EUR amountInEUR) public pure returns (USD) {
        uint256 eurAmount = EUR.unwrap(amountInEUR); // Unwrap the EUR type to get the uint256 value
        uint256 usdAmount = (eurAmount * 100) / usdToEurRate; // Simple conversion
        return USD.wrap(usdAmount); // Wrap the result in the USD type
    }
}
```
Solidity Docs Example
```solidity
// SPDX-License-Identifier: GPL-3.0
pragma solidity ^0.8.8;

// Represent a 18 decimal, 256 bit wide fixed point type using a user-defined value type.
type UFixed256x18 is uint256;

/// A minimal library to do fixed point operations on UFixed256x18.
library FixedMath {
    uint constant multiplier = 10**18;

    /// Adds two UFixed256x18 numbers. Reverts on overflow, relying on checked
    /// arithmetic on uint256.
    function add(UFixed256x18 a, UFixed256x18 b) internal pure returns (UFixed256x18) {
        return UFixed256x18.wrap(UFixed256x18.unwrap(a) + UFixed256x18.unwrap(b));
    }
    /// Multiplies UFixed256x18 and uint256. Reverts on overflow, relying on checked
    /// arithmetic on uint256.
    function mul(UFixed256x18 a, uint256 b) internal pure returns (UFixed256x18) {
        return UFixed256x18.wrap(UFixed256x18.unwrap(a) * b);
    }
    /// Take the floor of a UFixed256x18 number.
    /// @return the largest integer that does not exceed `a`.
    function floor(UFixed256x18 a) internal pure returns (uint256) {
        return UFixed256x18.unwrap(a) / multiplier;
    }
    /// Turns a uint256 into a UFixed256x18 of the same value.
    /// Reverts if the integer is too large.
    function toUFixed256x18(uint256 a) internal pure returns (UFixed256x18) {
        return UFixed256x18.wrap(a * multiplier);
    }
}
```

