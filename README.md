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

