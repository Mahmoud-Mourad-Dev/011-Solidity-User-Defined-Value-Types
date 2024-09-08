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
